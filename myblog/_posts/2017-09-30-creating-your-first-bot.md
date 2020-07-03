---
layout: post
title:  "Creating your first bot"
date:   2017-09-30 06:04:29 -0400
categories: azure bot framework
---
### Creating a bot
Ever since Microsoft announced the ability to create bots they have been of interest to me. With all the talk at Ignite this week I decided to create one myself and blog each step here.

The first step is [go here](https://dev.botframework.com/) and click on Create a Bot or Skill.
![](https://jweiler.ghost.io/content/images/2017/09/create-bot.PNG)
You will have to sign in (I used my personal Microsoft account) and then if you don't already have any bots you will see a create button which will take you into the Azure portal.
![](https://jweiler.ghost.io/content/images/2017/09/create.PNG)
In Azure you need to give you bot a name and fill in a few more things and hit Create. When the resource was created Azure took me to a page to register my app and choose a template for my Bot.
![](https://jweiler.ghost.io/content/images/2017/09/register.PNG)
When you hit create it will take you to another page where you have the option to edit the code online or download the source code to edit locally. I chose to edit online and it takes you to an online editor that looks very much like VS Code! 

An easy way to see what your bot does it go to [https://dev.botframework.com/bots](https://dev.botframework.com/bots) and click on your bot and in the top right there should be a Test button which opens a chat window for you chat with your new bot. 

If you click on Channels you will see it has Skype set up as one of them by default. If you  click on Skype it will bring the bot into Skype for you and you can start chatting there with it.
![](https://jweiler.ghost.io/content/images/2017/09/channels.PNG)

The basic template just echoes back what you tell it and it does so with an `EchoDialog` class.
```csharp
[Serializable]
public class EchoDialog : IDialog<object>
{
    public async Task StartAsync(IDialogContext context)
    {
        context.Wait(MessageReceivedAsync);
    }

    public async Task MessageReceivedAsync(IDialogContext context, IAwaitable<IMessageActivity> argument)
    {
        var message = await argument;
        await context.PostAsync("You said: " + message.Text);
        context.Wait(MessageReceivedAsync);
    }
}
```
You can easily make changes to the code in here and to get it to update the bot you have to open the Console (Ctrl+Shift+C) and type build.cmd and enter.

Now my bot got just a little bit smarter.
![](https://jweiler.ghost.io/content/images/2017/09/weather.PNG)

Next time I want to make a bot that actually interacts intelligently with a person.

#### Conclusion
This was a very brief overview of how to get your first bot up and going and hopefully it can pique your interest and make you want to go make a bot for yourself. For more advanced bots you can check out the samples that Microsoft [has here](https://github.com/Microsoft/BotBuilder-Samples). Also Stackoverflow also is building a bot and you can check that out [here](https://github.com/Microsoft/BotFramework-Samples/tree/master/StackOverflow-Bot).

Have you built any Bots?





