# [Handle and raise events](https://learn.microsoft.com/en-us/dotnet/standard/events/)

Events in .NET are based on the delegate model. The delegate model follows the [observer design pattern](https://learn.microsoft.com/en-us/dotnet/standard/events/observer-design-pattern), which enables a subscriber to register with and receive notifications from a provider.

<br/>

### Syntax

```C#
class Counter
{
    public event EventHandler ThresholdReached;

    protected virtual void OnThresholdReached(EventArgs e)
    {
        ThresholdReached?.Invoke(this, e);
    }

    // provide remaining implementation for the class
}
```

<br/>

### Event

The event is typically a **member** of the event sender. For example, the `ThresholdReached` event is a member of the `Counter` class.

*   An event is a message sent by an object to signal the occurrence of an action.
*   The object that raises the event is called the **event sender**.
*   An event data object, which is an object of type `EventArgs` or a **derived type**.

To define an event, you use the C# `event` keyword in the signature of your event class, and specify the type of `delegate` for the event.

```C#
class Counter
{
    public event EventHandler ThresholdReached;
}
```

> [!NOTE]
> The event is associated with the `EventHandler` delegate.

> [!TIP]
> .NET provides the `EventHandler` and `EventHandler<TEventArgs>` delegates to support most event scenarios. These delegates **have no return type** value **and** take two parameters:
> *   an object for the **source of the event** and
> *   an object for **event data**.

<br/>

### Delegates

A delegate is declared with a signature that shows the **return type and parameters** for the methods it references.

*   Use the `EventHandler` delegate for all events that don't include event data.
*   Use the `EventHandler<TEventArgs>` delegate for events that include data about the event.

```C#
public delegate void ThresholdReachedEventHandler(object sender, ThresholdReachedEventArgs e);
```

> [!NOTE]
> *   A delegate is equivalent to a **type-safe** function pointer or a **callback**.

<br/>

### Event data classes

Data associated with an event can be provided through an event data class. .NET follows a **naming pattern** where all event data classes end with the `EventArgs` suffix.

You can create a class that derives from the `EventArgs` class to provide any members needed to pass data related to the event.

```C#
public class ThresholdReachedEventArgs : EventArgs
{
    public int Threshold { get; set; }
    public DateTime TimeReached { get; set; }
}
```

<br/>

### Event sender (provider)

The object that raises the event is called the **event sender**. An **event sender** pushes a notification when an event occurs.

To raise an event, you add a method that is marked as `protected` **and** `virtual` (in C#). The **naming convention** for the method is `On<EventName>`, such as `OnThresholdReached`.

```C#
class Counter
{
    protected virtual void OnThresholdReached(EventArgs e) { }
}
```

### Event receiver (subscriber)

Event receiver is method that receives (handles) the events it raises.

To receive notifications when the event occurs, **your event handler method** must subscribe to the event. This method must match the signature of the delegate for the event you're handling.

```C#
class ProgramTwo
{
    static void Main()
    {
        var c = new Counter();
        c.ThresholdReached += c_ThresholdReached;

        // provide remaining implementation for the class
    }

    static void c_ThresholdReached(object sender, EventArgs e)
    {
        Console.WriteLine("The threshold was reached.");
    }
}
```
