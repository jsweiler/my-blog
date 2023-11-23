---
layout: post
title:  "Getting started with Rebus"
date:   2023-01-24 06:04:29 -0400
categories: rebus
---

## Learn how to use Rebus with an in memory transport.

I saw [Rebus](https://rebus.fm/what-is-rebus/) some time ago and thought it would be interesting to take a look at. Rebus is often used as an abstraction for things like Azure Service Bus and RabbitMQ, but it can also be used with in memory queues.

To get it installed in your project run the following command:

`Install-Package Rebus -ProjectName <your-project>`
I'm going to show a simple implementation within a console application. You can set it up like this:

```csharp
var activator = new BuiltinHandlerActivator();
Configure.With(activator)
    .Transport(t => t.UseInMemoryTransport(new InMemNetwork(), "test-queue"))
    .Logging(t => t.None())
    .Start();
```
I told it to use the in memory transport option and specified a name of the queue. At this point I could send a message like this:

`await activator.Bus.SendLocal(50);`
But nothing would be listening for an integer message yet because we haven't setup any message handlers. Let's say we want to handle a birthday event. We can create a class like this:

```csharp
public class HandleBirthdayEvent : IHandleMessages<Birthday>
{
    public Task Handle(Birthday birthday)
    {
        Console.WriteLine($"Congrats {birthday.Name} on turning {birthday.Age}");
        return Task.CompletedTask;
    }
}
```
Before we send the message we need to register our new HandleBirthdayEvent like this:

`activator.Register(() => new HandleBirthdayEvent());`
Then we can send the message to ourself with SendLocal like this:

`await activator.Bus.SendLocal(new Birthday {  Name = "Bob", Age = 50});`
Then we should see a Console.WriteLine message.

There are other ways to send a message. If we wanted to go a particular queue we could use this method to do so.

`await activator.Bus.Advanced.Routing.Send("test-queue", new Birthday { Name = "Bob", Age = 50 });`
Or another way is to configure Routing and to do it based on type we could add this to the Configure of rebus.

`.Routing(r => r.TypeBased().Map<Birthday>("test-queue"))`
Then we could simply send the message like this:

`await activator.Bus.Send(new Birthday { Name = "Bob", Age = 50 });	`
Because we configured the routing to use the test-queue for type Birthday we don't need to specify what queue we are sending to when we send the message.

Now to use this in real world application you would typically register Rebus with the .NET service provider just like many other things are added into your app. There is a big list of [samples](https://github.com/rebus-org/RebusSamples) that you should check out to get ideas of what you could all use Rebus for.