---
layout: post
title:  "Microsoft Cognitive Api"
date:   2017-02-17 06:04:29 -0400
categories: microsoft cognitive services
---
I had recently heard about Microsoft's Cognitive Api. I decided I wanted to learn more about how to use them. This will explain my journey as I learned.

#### Start Here
You can check it out [here](https://www.microsoft.com/cognitive-services/en-us/). From there you can sign up, I used my Microsoft account to do so. You will get an email which you need to verify your email. Then you can check which of the cognitive services you would like to try. Once you subscribe you will get keys to use for each of the services.

#### Computer Vision
I was interested in the computer vision service so that's what I started with. There is a Nuget package that makes it easy to use [Microsoft.ProjectOxford.Vision](https://www.nuget.org/packages/Microsoft.ProjectOxford.Vision/). You simply use it like this.

```csharp
var client = new VisionServiceClient("yourAPIKey");
var file = new FileStream("Pictures\\surface2.jpg", FileMode.Open);
var result = await client.DescribeAsync(file);

txtResults.Text = JsonConvert.SerializeObject(result, Formatting.Indented);

```
There I am using the `DescribeAsync` method on the picture of a Surface 2 and turning the result I get into Json. The following is what I get for this picture 
![](https://www.jweiler.com/content/images/2017/02/surface2.JPG)
```json
{
  "RequestId": "62dd6247-7802-48ad-8c96-cec14735702a",
  "Metadata": {
    "Height": 2419,
    "Width": 3629,
    "Format": "Jpeg"
  },
  "ImageType": null,
  "Color": null,
  "Adult": null,
  "Categories": null,
  "Faces": null,
  "Tags": null,
  "Description": {
    "Tags": [
      "indoor",
      "laptop",
      "computer",
      "sitting",
      "table",
      "black",
      "top",
      "monitor",
      "desk",
      "screen",
      "phone",
      "keyboard",
      "laying",
      "man"
    ],
    "Captions": [
      {
        "Text": "a laptop computer",
        "Confidence": 0.90010949437817545
      }
    ]
  }
}
```
It is fairly confident that it is "a laptop computer". Since Microsoft says the Surface is a tablet that can replace your laptop, I guess that is a pretty accurate statement.

#### Sample
I made a little sample WPF app [here](https://github.com/jsweiler/ComputerVision) that shows use of a few methods on the `VisionServiceClient` Api. It has three images in the Images folder, two of which I use the `Describe` method on, and the other one I use the `GetTags` method.

You can learn more about how to call the Computer Vision Api [here](https://www.microsoft.com/cognitive-services/en-us/Computer-Vision-API/documentation/vision-api-how-to-topics/HowToCallVisionAPI). If you don't want to use the `VisionServiceClient` you can call the Api with a query string as well. Also you can create a Cognitive Services account on Azure, something I plan to look into and write about how to do that.

Have you used the Computer Vision Api before?