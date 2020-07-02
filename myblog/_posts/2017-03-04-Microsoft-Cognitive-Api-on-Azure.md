---
layout: post
title:  "Microsoft Cognitive Api on Azure"
date:   2017-03-04 06:04:29 -0400
categories: microsoft cognitive services
---
#### Cognitive Services
Last time [I talked about](http://www.jweiler.com/microsoft-cognitive-api/) one way you can sign up and use Microsoft's Cognitive services. I also learned that you can use the azure portal to get started as well. To get started you will need an Azure account and will go to [portal.azure.com](https://portal.azure.com).

Start by clicking the new button on the top left.
![](https://www.jweiler.com/content/images/2017/03/azurenew.PNG)
Then click on Intelligence + Analytics and then on the right click Cognitive Services Api
![](https://www.jweiler.com/content/images/2017/03/cogapi.PNG)
Then you need to fill out a few things and click Create.
![](https://www.jweiler.com/content/images/2017/03/create.PNG)
I like to pin stuff to the dashboard so I can easily find it again. To use the Api we will need to know what our Api key is. This can be found by clicking Keys.
![](https://www.jweiler.com/content/images/2017/03/keys.PNG)
Since I will be using C# I used [this Quick Start guide](https://www.microsoft.com/cognitive-services/en-us/Computer-Vision-API/documentation/QuickStarts/CSharp) to get started.

Last time I used a Nuget package that made it really easy to use the Computer Vision Api. This time I decided to use `HttpClient` similar to what the quick start guide shows you.

I made a [little WPF app](https://github.com/jsweiler/CognitiveServicesAzure) that makes use of the Vision api. It asks you for an image and then posts that data as binary.

```csharp
var filePicker = new OpenFileDialog();
filePicker.Filter = "Image files (*.jpg, *.jpeg, *.jpe, *.jfif, *.png) | *.jpg; *.jpeg; *.jpe; *.jfif; *.png";
var result = filePicker.ShowDialog();
var imageFile = filePicker.FileName;
if (!File.Exists(imageFile)) return;
var binary = GetBytesFromPath(imageFile);

using (var httpClient = new HttpClient())
{
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "yourAPIKey");
    var requestParameters = "visualFeatures=Description&language=en";
    var uri = "https://westus.api.cognitive.microsoft.com/vision/v1.0/analyze?" +        requestParameters;
    using (var content = new ByteArrayContent(binary))
    {
        content.Headers.ContentType = new  System.Net.Http.Headers.MediaTypeHeaderValue("application/octet-stream");
        var response = await httpClient.PostAsync(uri, content);

        var json = await response.Content.ReadAsStringAsync();
        txtResults.Text = json;
    }
}

```
In the `requestParameters` I say visualFeatures=Description. You can learn more about the other options that [you can use here](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa).

In Azure I can take a look at how many calls I have made and errors that happened which is cool.
![](https://www.jweiler.com/content/images/2017/03/metrics.PNG)

Hopefully I made it clear how easy it is to set this up in Azure. I know I only scratched the surface on what you all can do with the Cognitive Services.

Have you used Microsoft's Cognitive Services? If so what did you think of them?
