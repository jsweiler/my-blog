<!-- ---
layout: post
title:  "Json Schema Example For .Net Derived Types"
date:   2020-07-03 06:04:29 -0400
categories: json schema
--- -->
If you have ever worked with json you probably have needed to validate it to make sure it is structured properly for your application to understand. I recently had to validate some json where the object had an array of items and those items each derived from a base class. I'll describe how I made a json schema file and then how to validate it using [Newtonsoft's Json schema library](https://www.newtonsoft.com/jsonschema).

For information about json schema be sure to check out the official site [here](https://json-schema.org/).

A very simple json schema for a `Person` object that has a few properties would look like this.
``` json
{
  "$id": "https://example.com/person.schema.json",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Person",
  "type": "object",
  "required": ["firstName", "lastName"],
  "properties": {
    "firstName": {
      "type": "string",
      "description": "The person's first name."
    },
    "lastName": {
      "type": "string",
      "description": "The person's last name."
    },
    "age": {
      "description": "Age in years which must be equal to or greater than zero.",
      "type": "integer",
      "minimum": 0
    }
  }
}
```
### Using Json Schema validator
It's fairly simple and you can test that schema out against an actual json document like this at this url: [https://www.jsonschemavalidator.net/s/TIIl3bA9](https://www.jsonschemavalidator.net/s/TIIl3bA9)
``` json
{
  "firstName": "John",
  "lastName": "Doe",
  "age": 21
}
```
Try changing the input json to change the `firstName` to something like `first` and you will see that the validation will fail.

