---  
slug: new-jdk-14-socket-timeout-control
title: New JDK 1.4 socket timeout control
created: 2002-11-25 08:00:00+00:00
---  
[Core Java Technologies Technical Tips](http://developer.java.sun.com/developer/JDCTechTips/2002/tt1119.html#2)

```java
int timeout = 500; // half a second
SocketAddress socketAddress =
  new InetSocketAddress(host, port);
Socket socket = new Socket();
socket.connect(socketAddress, timeout);
```



[From an archive on my old blog](http://web.archive.org/web/20030716140757/http://www.obrain.com/Eamonn/archives/000005.html)
