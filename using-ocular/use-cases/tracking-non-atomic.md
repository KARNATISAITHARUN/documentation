# Tracking Non Atomic Data Types

In general, processes do not execute atomically, since the operating system may interrupt processes between essentially 
any two instructions, allowing other processes to run. If your application's process is not prepared for these interruptions, another process may be able to interfere with it, causing the data structures to end up in inconsistent states if arbitrary code is executed between them. 

A non atomic condition is a vulnerability to interference caused by untrusted processes. These are conditions caused by processes running other, different programs, which introduce other actions between steps of the program. These other programs might be invoked by an attacker. 

This use case illustrates how to use ShiftLeft Ocular to analze for non atomic data types, using the FreeRTOS real-time operating system kernel as the example target application. 

## 1. Download the FreeRTOS application.

[Download the FreeRTOS application](https://www.freertos.org/a00104.html) and unzip the source code into the ShiftLeft Ocular `subjects` directory, for example `subjects/FreeRTOS`.

## 2. Create and load the FreeRTOS application's CPG.

```scala
ocular> createCpg("subjects/"FreeRTOS")
```
FreeRTOS's CPG is automatically loaded into memory.

## 3. Declare an array of primitive types. 

```scala
ocular> val primitiveTypes = List("int", "float", "double", "void", "size_t", "ANY", "void", "char", "short")
```

## 4. Call a convenience function to get LineNumber.

```scala
ocular> def getLineNumber(ln : Option[Integer]) = (ln match { case Some(x) => x ; case None => 0 }).asInstanceOf[Int]
```

## 5. Declare a data structure of User Defined Types.

```scala
ocular> case class UDT(uType : String, uName : String,  methodName : String, fileName : String, lineNumber : Integer)
```

## 6. Acquire all identifiers into UDT data structure (negative filter on primitive types).

```scala
ocular> val udtList = cpg.identifier.l.map {
   i => UDT(i.typeFullName, i.name, i.start.method.name.l.head, i.start.file.name.l.head, getLineNumber(i.lineNumber))
} filter {
   p => !primitiveTypes.exists(e => p.uType.contains(e))
} distinct
```

## 7. Store findings in a multimap keyed by identified methodName.

```scala
ocular> import collection.mutable._
val udtMap = new HashMap[String,Set[UDT]] with MultiMap[String,UDT]
udtList.foreach { 
item => udtMap.addBinding(item.methodName, item) 
}
```

## 8. Store findings in a multimap keyed by identified type.

```scala
ocular> import collection.mutable._
val udtMapByType = new HashMap[String,Set[UDT]] with MultiMap[String,UDT]
udtList.foreach { 
item => udtMapByType.addBinding(item.uType, item) 
}

implicit def flat[K,V](kv: (K, Option[V])) = kv._2.map(kv._1 -> _).toList
```

## 9. Call a convenience function to get Range given start and end.

```scala
ocular> def getRange(start : Int , end : Int) = start to end toList
```

## 10. Give functionName return all callOuts within scope of the function.

```scala
ocular> def getCallOutDetails(fnName:String) = cpg.method.name(fnName).callOut.l.map(co => (co.name , getLineNumber(co.lineNumber))).sortBy(_._2)
```

## 11. Optimize replacement.

```scala
ocular> def getCallOutDetails(fnName:String) = cpg.method.name(fnName).callOut.l.par.map(co => (co.name , getLineNumber(co.lineNumber))).toList.sortBy(_._2)
```

## 12. Call a convenience function for holding functions with CRITICAL SECTIONS with ranges

```scala
ocular> case class WithCriticalSection(fnName : String, fileName : String, csRange : List[(String, Int, Int)])
```

## 13. Specify functions to optimize ranging (in case where multiple critical sections exists in a method (function)

```scala
ocular> def getBounds(dataTuples : List[(String,Int)]) =
  dataTuples.foldLeft((List.empty[(String, Int)], Option.empty[(String, Int)])) {
    case ((state, previousSignal), signal) =>
      if (previousSignal.exists(_._1.contains("EXIT")) && signal._1.contains("EXIT")) {
        (state.dropRight(1) :+ signal, Some(signal))
      } else {
        (state :+ signal, Some(signal))
      }
  }

def getRange(enterExits : (List[(String, Int)], Option[(String, Int)])) =
  enterExits._1.foldLeft(
      (List.empty[(String, Int, Int)], Option.empty[(String, Int, Int)])) {
      case ((state, accSignal), signal) =>
        if (signal._1.contains("ENTER")) {
          (state, Some(("CRITICAL_SECTION_RANGE", signal._2, 0)))
        } else {
          val enterExt = accSignal.map(elem => elem.copy(_3 = signal._2))
          (state :+ enterExt.get, Option.empty)
        }
    }._1
```

## 14. Get entire callOut trace for each method that encompasses "taskENTER_CRITICAL".

```scala
ocular> val callMap = cpg.method.filter(_.callOut.name("taskENTER_CRITICAL")).l map {
  s => Map(s.name -> (s.start.file.name.head, getCallOutDetails(s.name.replaceAll("\\*",""))))
} reduce(_ ++ _)
```

## 15. Get entire callOut trace for each method that DO NOT encompass "taskENTER_CRITICAL".

```scala
ocular> val callMapWithoutCS = cpg.method.filterNot(_.callOut.name("taskENTER_CRITICAL")).l map {
          s => Map(s.name -> (s.start.file.name.headOption.getOrElse("NOT_IDENTIFIED"), getCallOutDetails(s.name.replaceAll("\\*",""))))
        } reduce(_ ++ _)
```

## 16. Optimize replacement.

```scala
ocular> def timeTaken[R](block: => R): R = {
    val t0 = System.nanoTime()
    val result = block    // call-by-name
    val t1 = System.nanoTime()
    println("Elapsed time: " + (t1 - t0) + "ns")
    result
}

val mWithoutCS = cpg.method.filterNot(_.callOut.name("taskENTER_CRITICAL")).l

timeTaken {
   val callMapWithoutCS = mWithoutCS.map {
         s => Map(s.name -> (s.start.file.name.headOption.getOrElse("NOT_IDENTIFIED"),  getCallOutDetails(s.name.replaceAll("\\*",""))))
   } reduce(_ ++ _)
}
```

## 17. Filter callMap and fit into WithCriticalSection

```scala
ocular> val callMapFiltered = callMap map {
    case(k,v) => k -> (v._1 , getRange(getBounds(v._2.filter(t => t._1.contains("taskENTER_CRITICAL") || t._1.contains("taskEXIT_CRITICAL")))))
} map { case(k,v) => k -> WithCriticalSection(k,v._1,v._2) }
```

## 18. Navigate using an identifier, for example called `xQueueAddToSet`. Pick any type from `udtMapByType` say for instance tfp_format

```scala
…
"SIZEOF_LONG_LONG" -> Set(
    UDT("SIZEOF_LONG_LONG", "lng", "tfp_format", "../../Downloads/dahling-fw/Src/utils/tinystdio.c", 398),
    UDT("SIZEOF_LONG_LONG", "lng", "tfp_format", "../../Downloads/dahling-fw/Src/utils/tinystdio.c", 412),
    UDT("SIZEOF_LONG_LONG", "lng", "tfp_format", "../../Downloads/dahling-fw/Src/utils/tinystdio.c", 408)
  
...

val nonAtomicUsedInCS =callMap.getOrElse("tfp_format","NOT_FOUND")
val nonAtomicUsedInNoCS =callMapWithoutCS.getOrElse("tfp_format","NOT_FOUND")
```

If `(nonAtomicUsedInCS.size > 0 && nonAtomicUsedInNoCS.size > 0)`, this implies that a non atomic data type is used both in guarded context and not in guarded context, possibly leading to deadlock or starvation.

## 19. Determine the location details of where the non atomic data type is used.

```scala
ocular> val udtSet = udtMap("xStreamBufferReset")
val callSiteData = callMapFiltered.get("xStreamBufferReset").get

udtSet.foreach {
  udt => callSiteData.csRange.map { item => 
  	if((udt.lineNumber > item._2) && (udt.lineNumber < item._3)) {
  		printf("[%s] is bound in critical section at [%d] between [%d] and [%d] in methodName [%s] located at [%s]\n", udt.uName, udt.lineNumber, item._2, item._3, udt.methodName, udt.fileName)
  	}
  }
}
```
