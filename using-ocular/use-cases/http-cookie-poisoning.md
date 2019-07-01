# HTTP Cookie Poisoning

The main purpose of an HTTP cookie is to identify users and prepare customized Web pages for consumers based on their profile. Cookie poisoning is the act of an attacker modifying a cookie for the purpose of abusing the functionality of an application. There are three areas of potential security risks with cookies: 

* Sensitive information may be added by the cookie as plain text, which is not recommended. A malicious user can sniff the connection to capture any data that is being transmitted over a network. Therefore, encryption should be used.

* The developer may not have set the `HttpOnly` attribute. `HttpOnly` is an instruction for the browser to not change the cookie nor give it to the JavaScript engine. Setting the `HttpOnly` attribute limits the damage done in case of an XSS vulnerability.

* The developer did not set the `Secure` attribute. Developers usually store all sorts of information in cookies. Because cookies are easy to manipulate, they can exchange information from the server to the browser and vice-versa. When the `Secure` attribute is not used, the risk is a man-in-the-middle attack.

The difficulty of finding a cookie poisoning vulnerability is that the use of cookies is usually intertwined among several other program statements. These other statements are more important to the functionality being implemented, which ends up hiding the importance of cookies (and their security risk). Thus, developers or security researchers may not feel the need to  pay attention to cookies.

Using ShiftLeft Ocular, you can identify cookie poisoning vulnerability by executing the following commands:

```scala
{
val source = cpg.method.fullName(".*Cookie.<init>.*").parameter
val sink = cpg.method.name("addCookie").parameter
sink.reachableBy(source).flows.passesNot("setHttpOnly").passesNot("setSecure").p
}

{
val source = cpg.method.name("addCookie").parameter
val sink = cpg.method.fullName(".*javax.servlet.RequestDispatcher.forward.*").parameter
sink.reachableBy(source).flows.passesNot("setHttpOnly").passesNot("setSecure").p
}
```
