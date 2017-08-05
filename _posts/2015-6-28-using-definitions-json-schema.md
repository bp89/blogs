---
layout: post
title: 'Using definitions internal to json schema and external to json schema (version – draft -04)'
tags:  [Json Schema, JSON]
comments: true
---

There could be times when you end with a generously large schema with lots of redundant properties due to certain business requirements.

Json schema comes with a good feature named as definitions. It is used to contain repeating properties in one place and then referencing these using $ref and also overriding some property if needed.

Let’s take a look at schema below:
	
	{
     "title": "Contact Details",
      "readOnly": false,
      "$schema": "http://json-schema.org/draft-04/hyper-schema",
      "description": "Contact Details",
        "properties":{
        "contactDetails": {
              "title": "Contact Details",
              "properties": {
                "primaryAddress": {
                  "title": "Primary Address",
                  "readOnly": false,
                  "strictProperties": true,
                  "description": "",
                  "propertyOrder": 1,
                  "type": "string",
                  "options": {
                    "hidden": false
                  }
                },
                "secondryAddress": {
                  "title": "Secondry Address",
                  "readOnly": false,
                  "strictProperties": true,
                  "description": "",
                  "propertyOrder": 2,
                  "type": "string",
                  "options": {
                    "hidden": false
                  }
                }
              },
              "type": "object"
            }
        },
        "type":"object"
    }

Primary Address and Secondy address are two simple schema properties for now but these could be big object properties which may contain phone number,email address etc details as well. For easy understading let’s go with simple schema above. One thing to notice is that both primaryAddress and secondryAddress are similar except their title and being two separate objects.
This could be reduced to below using $ref combined with definitions:
	
	{
     "title": "Contact Details",
      "readOnly": false,
      "$schema": "http://json-schema.org/draft-04/hyper-schema",
      "description": "Contact Details",
      "definitions":{
          "address": {
                  "title": "Address",
                  "readOnly": false,
                  "strictProperties": true,
                  "description": "",
                  "type": "string",
                  "options": {
                    "hidden": false
                  }
                }
      },
        "properties":{
        "contactDetails": {
              "title": "Contact Details",
              "properties": {
                "primaryAddress": {
                  "title": "Primary Address",
                  "$ref":"#/definitions/address",
                  "propertyOrder":1
                },
                "secondryAddress": {
                  "title": "Secondry Address",
                  "$ref":"#/definitions/address",
                  "propertyOrder":2
                }
              },
              "type": "object"
            }
        },
        "type":"object"
    }

If you notice, schema size has been reduced and this could be significantly reduce, prove very helpful and make schema highly maintainable in case where reusable properties are bigger objects containing nested group of other objects.
To put more light on what happened, let go through the schema with definitions and $ref usage:
1. Definitions refer to shared properties which could be reused throughout schema or by some external schema.
2. $ref says either reference external schema or schema itself. Hence, it acts as JSON pointer.
3. Now in $ref ‘s value # refers to current document and could be prepended with relative url or some external url to specify external json as current refernce. # alone means current document #/definitions means definitions in current document. And hence #/definitions/address means address property/object contained within definitions in current document.
For external json schema let’s use http://myjson.com and see an example below:
Here we could add our sample extenal json file which contains our reusable definitions and get the url like in image below.

![_config.yml]({{ site.baseurl }}/images/posts/sampleOfUsingExternalJsonFile.png)

You could try it right now.
I have added below schema to the server:

	{
	  "$schema": "http://json-schema.org/draft-04/schema#",
	  "type": "object",
	  "definitions": {
		  "address":{
			  "title":"Address",
			  "properties":{
				  "street_address":{
					  "type":"string"
				  },
				  "city":{
					  "type":"string"
				  },
				  "state":{
					  "type":"string"
				  }
			  },
			  "type":"object"
		  }
	  }
	}

and got https://api.myjson.com/bins/3bkzl url. Keep in mind myjson.com could at some point may not work. Hence, we could also create our own local file server for same.

In our another json schema which want to re-use extenal schema could do it like below:

	{
	  "$schema": "http://json-schema.org/draft-04/schema#",
	  "type": "object",
	  "properties": {
		"residential_address": { "$ref": "https://api.myjson.com/bins/3bkzl.json#/definitions/address" },
		"office_address": { "$ref": "https://api.myjson.com/bins/3bkzl.json#/definitions/address" }
	 
	  },
	  "additionalProperties": false
	}

Last but not least, let’s understand the pattern which is:

>[url]#/definitions/[property]

 + url – url of external json schema
 + *#* `–` pound stands for current document which is the one if specified by url else current document in which we are writing it.
 + property – is the property from definitions in schema specified. It could be / seperated nested value e.g. in our case “https://api.myjson.com/bins/3bkzl.json#/definitions/address/city”
