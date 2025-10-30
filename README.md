# network-stream

## [TCP overview](https://learn.microsoft.com/en-us/dotnet/fundamentals/networking/sockets/tcp-classes)

To work with **Transmission Control Protocol** (TCP), you have two options: 

*   either use [Socket](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.socket) for maximum control and performance,
*   or use the [TcpClient](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcpclient) and [TcpListener](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcplistener) helper classes.

### [TcpListener](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcplistener?view=net-9.0)

[TcpListener](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcplistener?view=net-9.0) creates a [Socket](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.socket?view=net-9.0) to listen for incoming client connection requests. 

> [!NOTE]
> You can use either a [TcpClient](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcpclient?view=net-9.0) or a [Socket](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.socket?view=net-9.0) to connect with a [TcpListener](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcplistener?view=net-9.0). 


<br/>

### [Asynchronous programming patterns](https://learn.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/)

.NET provides three patterns for performing asynchronous operations:

#### [Task-based Asynchronous Pattern (TAP)](https://learn.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)

**Task-based Asynchronous Pattern (TAP)**, which uses a single method to represent the initiation and completion of an asynchronous operation. TAP was introduced in .NET Framework 4. 

> [!TIP]
> **It's the recommended approach to asynchronous programming in .NET.**

#### [Event-based Asynchronous Pattern (EAP)](https://learn.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap)

**Event-based Asynchronous Pattern (EAP)**, which is the event-based legacy model for providing asynchronous behavior. It requires a method that has the `Async` suffix and one or more events, event handler delegate types, and `EventArg`\-derived types. EAP was introduced in .NET Framework 2.0.

> [!CAUTION]
> **It's no longer recommended** for new development.

#### [Asynchronous Programming Model (APM)](https://learn.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm)

**Asynchronous Programming Model (APM)** pattern (also called the [IAsyncResult](https://learn.microsoft.com/en-us/dotnet/api/system.iasyncresult) pattern), which is the legacy model that uses the [IAsyncResult](https://learn.microsoft.com/en-us/dotnet/api/system.iasyncresult) interface to provide asynchronous behavior. In this pattern, asynchronous operations require `Begin` and `End` methods (for example, `BeginWrite` and `EndWrite` to implement an asynchronous write operation).

> [!CAUTION]
> This pattern **is no longer recommended** for new development.

<br/>

### Comparison of patterns

For a quick comparison of how the three patterns model asynchronous operations, consider a `Read` method that reads a specified amount of data into a provided buffer starting at a specified offset:

```C#
public class MyClass  
{  
    public int Read(byte [] buffer, int offset, int count);  
}  
```

The TAP counterpart of this method would expose the following single `ReadAsync` method:

```C#
public class MyClass  
{  
    public Task<int> ReadAsync(byte [] buffer, int offset, int count);  
}  
```

The EAP counterpart would expose the following set of types and members:

```C#
public class MyClass  
{  
    public void ReadAsync(byte [] buffer, int offset, int count);  
    public event ReadCompletedEventHandler ReadCompleted;  
}  
```

The APM counterpart would expose the `BeginRead` and `EndRead` methods:

```C#
public class MyClass  
{  
    public IAsyncResult BeginRead(  
        byte [] buffer, int offset, int count,
        AsyncCallback callback, object state);  
    public int EndRead(IAsyncResult asyncResult);  
}  
```




