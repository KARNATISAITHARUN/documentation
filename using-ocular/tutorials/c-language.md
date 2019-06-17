# Using ShiftLeft Ocular to Analyze a C Language Application

You can use ShiftLeft Ocular to analyze your C language applications. This article illustrates how, by using the example of [CVE-2016-6480 Linux Kernel](https://nvd.nist.gov/vuln/detail/CVE-2016-6480). The vulnerability is a race condition that exists in the Linux Kernel version 4.7, in the `ioctl_send_fib in drivers/scsi/aacraid/commctrl.c` function.

## Downloading the Example

Use the following commands to download the Linux Kernal with the example vulnerability

```
git clone https://github.com/torvalds/linux
cd linux
git checkout v4.7
```
## Building the Code Property Graph (CPG)

Build the CPG for the vulnerable driver 

```
./fuzzyc2cpg.sh path/to/kernel/linux/drivers/scsi/aacraid
```

## Analyzing the Code

Analyze the interaction from user to kernel space with `copy_from_user`. To determine if any data from user space to kernel space is copied, use

```
cpg.call.name("copy_from_user").code.p
```

ShiftLeft Ocular returns

```
copy_from_user(&qd, arg, sizeof (struct aac_query_disk))
copy_from_user(&dd, arg, sizeof (struct aac_delete_disk))
copy_from_user(&dd, arg, sizeof (struct aac_delete_disk))
copy_from_user((void *)kfib, arg, sizeof(struct aac_fibhdr))
copy_from_user(kfib, arg, size)
copy_from_user((void *)&f, arg, sizeof(struct fib_ioctl))
copy_from_user(&fibsize, &user_srb->count,sizeof(u32))
copy_from_user(user_srbcmd, user_srb,fibsize)
copy_from_user(p,sg_user[i],upsg->sg[i].count)
copy_from_user(p,sg_user[i],upsg->sg[i].count)
copy_from_user(p,sg_user[i],usg->sg[i].count)
copy_from_user(p, sg_user[i],\n\t\t\t\t\t\t\tupsg->sg[i].count)
```

indicating that there doesn't seem to be any problems with data from user space to kernel space.

To look at some flows from copy_from_user, use

```
val sinkArguments = cpg.method.name("copy_from_user").parameter.argument
println(sinkArguments.reachableByFlows(cpg.identifier).p)
```

which provides a lot of information.

The query 

```
println(sinkArguments.reachableByFlows(cpg.identifier).l.size)
```

returns the number 302.

Of interest would be an estimate if the arguments of copy_from_user are sanitized. Since there are no direct definitions at if expressions, there is no reaching definition information out of them. But information that flows into if expressions is available. To do so, using Main.scala, add the following lines

```
val reachingDefs1 = cpg.method
                       .name("copy_from_user")
                       .parameter
                       .argument
                       .reachableBy(cpg.identifier)
                       .toSet
```

So far reachableByFlows has been used to construct and print out the flows. To filter all that detail, use 'reachableBy' to indicate to ShiftLeft Ocular to identify only the sources that are hit, rather than the details of the data flow paths. The following query collects the sources that are hit into a set

```
val reachingDefs2 = cpg.method
                       .name(".*less.*", ".*greater.*")
                       .parameter
                       .argument
                       .reachableBy(cpg.identifier)
                       .toSet
 ```
 
 This query:
 
* Restricts the flows running to expressions that involve the less or greater keyword. Note that internally each binary operation (+,-,>,< etc.) is also treated as a function. So look for its arguments.
 
* Tracks data dependency back to each identifier it hits and collected into a set. 

Now check if there is an intersection between these two sets, which provides an estimate on what arguments of `copy_from_user` might be sanitized

```
reachingDefs1.intersect(reachingDefs2).foreach(elem => println(elem.code))
```

This query returns

```
kmalloc(fibsize, GFP_KERNEL)
kmalloc(actual_fibsize - sizeof(struct aac_srb)\n\t\t\t  + sizeof(struct sgmap), GFP_KERNEL)
user_srbcmd->sg
kfib->header
size = le16_to_cpu(kfib->header.Size) + sizeof(struct aac_fibhdr)
actual_fibsize = sizeof(struct aac_srb) - sizeof(struct sgentry) +\n\t\t((user_srbcmd->sg.count & 0xff) * sizeof(struct sgentry))
* upsg = (struct user_sgmap64*)&user_srbcmd->sg
i = 0
user_srbcmd->sg
usg = kmalloc(actual_fibsize - sizeof(struct aac_srb)\n\t\t\t  + sizeof(struct sgmap), GFP_KERNEL)
user_srbcmd->sg
* upsg = &user_srbcmd->sg
user_srbcmd = kmalloc(fibsize, GFP_KERNEL)
i = 0
fibsize = 0
aac_fib_alloc(dev)
user_srbcmd->sg.count
kfib = fibptr->hw_fib_va
kfib->header.Size
actual_fibsize - sizeof(struct aac_srb)\n\t\t\t  + sizeof(struct sgmap)
fibptr = aac_fib_alloc(dev)
i = 0
fibptr->hw_fib_va
i = 0
```

The return shows that most potential checks involve some kind of a size element (as expected).

Some outputs from copy_from_user have `kfib` as their first argument, in order to determine if `kfib` is an interesting pointer which gives access to a header. The size of `kfib` seems to have an involvement in the check `kfib->header.Size`. To confirm this in the source code (commctrl.c:90), use

```
size = le16_to_cpu(kfib->header.Size) + sizeof(struct aac_fibhdr);
if (size < le16_to_cpu(kfib->header.SenderSize))
```

Use the following query to filter for `copy_from_user` looking for kfib as an argument

```
cpg.call.name("copy_from_user").code(".*kfib.*").l
```

or

```
cpg.call.name("copy_from_user").filter(call => call.argument.code(".*kfib.*")).l
```

To print the return

```
cpg.call.name("copy_from_user")
.filter(call => call.argument.code(".*kfib.*"))
.l
.foreach(call => println(call.code))
```

outputs

```
copy_from_user((void *)kfib, arg, sizeof(struct aac_fibhdr))
copy_from_user(kfib, arg, size)
```

Next, find flows from these sinks to a common ancestor which defines `kfib`, and to ensure that there is no other definition of `kfib` which might have a double fetch.

```
val cfu1 = cpg.call.name("copy_from_user")
   .code(".*kfib.*")
   .l
   .head
   .start
   .reachableBy(cpg.identifier)
   .toSet

  val cfu2 = cpg.call.name("copy_from_user")
    .code(".*kfib.*")
    .l
    .last
    .start
    .reachableBy(cpg.identifier)
    .toSet
    .intersect(cfu1)


   cfu2.foreach(elem => println(elem.code, elem.lineNumber.get))
```
This pattern is similar to reaching definitions to sanitizers. The start basically tells ShiftLeft Ocular to start a fresh traversal starting at the given node. In this case, head and last are filtered to get those nodes. The output is

```
(aac_fib_alloc(dev),71)
(kfib = fibptr->hw_fib_va,76)
(fibptr = aac_fib_alloc(dev),71)
(fibptr->hw_fib_va,76)
```
