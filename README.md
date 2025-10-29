# network-stream

## [TCP overview](https://learn.microsoft.com/en-us/dotnet/fundamentals/networking/sockets/tcp-classes)

To work with **Transmission Control Protocol** (TCP), you have two options: 

*   either use [Socket](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.socket) for maximum control and performance,
*   or use the [TcpClient](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcpclient) and [TcpListener](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcplistener) helper classes.

### [TcpListener](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcplistener?view=net-9.0)

[TcpListener](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcplistener?view=net-9.0) creates a [Socket](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.socket?view=net-9.0) to listen for incoming client connection requests. 

> [!NOTE]
> You can use either a [TcpClient](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcpclient?view=net-9.0) or a [Socket](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.socket?view=net-9.0) to connect with a [TcpListener](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcplistener?view=net-9.0). 
