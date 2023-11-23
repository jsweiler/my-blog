---
layout: post
title:  "using azure translator api"
date:   2023-02-21 06:04:29 -0400
categories: azure
---

The Azure Translator service is very handy if you have some text you need to translate from one language to another. There is more that 100 languages available. See the whole list here.

After creating a Translator resource in Azure it's very easy to get started translating text. You will need a Key and the Endpoint which can be found on the Keys and Endpoints tab:

![](/assets/images/2023-02-21-translator1.png)

With C# the code to translate some text is quite simple:

```csharp
var route = "/translate?api-version=3.0&from=zh-Hans&to=en";
var body = new object[] { new { Text = textToTranslate } };
var requestBody = JsonConvert.SerializeObject(body);

using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
    // Build the request.
    request.Method = HttpMethod.Post;
    request.RequestUri = new Uri("https://api.cognitive.microsofttranslator.com/" + route);
    request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
    request.Headers.Add("Ocp-Apim-Subscription-Key", "myKey");
    request.Headers.Add("Ocp-Apim-Subscription-Region", "eastus");

    var response = await client.SendAsync(request);
    var result = await response.Content.ReadAsStringAsync();
}
```
It's important to note that in my route variable I specify my from and to languages.

Then the result will be a json string that contains the translated results.

You can also try out the translator service from the overview tab of the Azure resource. This will show you what the response should look like from the API:

![](/assets/images/2023-02-21-translator2.png)

