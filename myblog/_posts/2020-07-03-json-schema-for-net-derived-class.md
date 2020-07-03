---
layout: post
title:  "Json Schema Example For .Net Derived Types"
date:   2020-07-03 06:04:29 -0400
categories: json schema
---
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

Now for the examle of a schema for inherited classes. The objects that I will be serializing will look like this: 
```csharp
public class Garage
{
    public string Name { get; set; }

    public Vehicle[] Vehicles { get; set; }
}

public class Vehicle
{
    public string Make { get; set; }
    public string Model { get; set; }
    public Color Color { get; set; }
}

public class OffRoadVehicle : Vehicle
{
    public string OffRoadUniqueProperty { get; set; }
}

public class Truck : Vehicle
{
    public string TruckUniqueProperty { get; set; }
}

public class RaceCar : Vehicle
{
    public bool WonMedals { get; set; }
}
```
The garage object will produce json that looks like this:
``` json
{
  "Name": "My Garage",
  "Vehicles": [
    {
      "$type": "GarageApp.OffRoadVehicle, GarageApp",
      "OffRoadUniqueProperty": "the best off road vehicle",
      "Make": "Custom",
      "Model": "Custom",
      "Color": "Black"
    },
    {
      "$type": "GarageApp.RaceCar, GarageApp",
      "WonMedals": true,
      "Make": "Formula 1",
      "Model": "Custom",
      "Color": "Red"
    },
    {
      "$type": "GarageApp.Truck, GarageApp",
      "TruckUniqueProperty": "A great truck",
      "Make": "GMC",
      "Model": "Sierra",
      "Color": "Blue"
    }
  ]
}
```
So the json schema that I came up with to validate that uses the `oneOf` property and then proceeds to specify the schema of each derived vehicle type importantly including the `$type` property. I mark them all as required to check to make sure it is valid:
``` json
{
    "$id": "https://example.com/person.schema.json",
    "$schema": "http://json-schema.org/draft-07/schema#",
    "title": "Garage",
    "type": "object",
    "required": [
        "Name",
        "Vehicles"
    ],
    "properties": {
        "Name": {
            "type": "string",
            "description": "The Garage's name."
        },
        "Vehicles": {
            "type": "array",
            "description": "The list of vehicles",
            "items": {
                "oneOf": [
                    {
                        "type": "object",
                        "required": [
                            "$type",
                            "Make",
                            "Model",
                            "Color",
                            "OffRoadUniqueProperty"
                        ],
                        "additionalProperties": false,
                        "properties": {
                            "$type": {
                                "const": "GarageApp.OffRoadVehicle, GarageApp"
                            },
                            "Make": {
                                "type": "string"
                            },
                            "Model": {
                                "type": "string"
                            },
                            "Color": {
                                "type": "string"
                            },
                            "OffRoadUniqueProperty": {
                                "type": "string"
                            }
                        }
                    },
                    {
                        "type": "object",
                        "required": [
                            "$type",
                            "Make",
                            "Model",
                            "Color",
                            "WonMedals"
                        ],
                        "additionalProperties": false,
                        "properties": {
                            "$type": {
                                "const": "GarageApp.RaceCar, GarageApp"
                            },
                            "Make": {
                                "type": "string"
                            },
                            "Model": {
                                "type": "string"
                            },
                            "Color": {
                                "type": "string"
                            },
                            "WonMedals": {
                                "type": "boolean"
                            }
                        }
                    },
                    {
                        "type": "object",
                        "required": [
                            "$type",
                            "Make",
                            "Model",
                            "Color",
                            "TruckUniqueProperty"
                        ],
                        "additionalProperties": false,
                        "properties": {
                            "$type": {
                                "const": "GarageApp.Truck, GarageApp"
                            },
                            "Make": {
                                "type": "string"
                            },
                            "Model": {
                                "type": "string"
                            },
                            "Color": {
                                "type": "string"
                            },
                            "TruckUniqueProperty": {
                                "type": "string"
                            }
                        }
                    }
                ]
            }
        }
    }
}
```
You can test that example out [here](https://www.jsonschemavalidator.net/s/GETdmRYH).

You also might have noticed the `additionalProperties` which I set to false. This disallows any other properties to be added to the input json.

To validate a schema in a .net application be sure to get the [Newtonsoft.Json.Schema from nuget](https://www.nuget.org/packages/Newtonsoft.Json.Schema/).

### Conclusion
Json schema is a powerful way to check to make sure your json is in the form you want it, nothing more, nothing less. This example showed how to make a schema that will validate derived classes in an array.