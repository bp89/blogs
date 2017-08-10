---
layout: post
title: 'Using “dependencies” in json schema (version : draft-v4)'
categories: [Programming]
tags:  [Json Schema, JSON]
comments: true
---

Json Schema has another interesting feature which allows value of some property of json schema to depend upon other fields value. This could be done using “dependencies” which allows specifying dependent object / property on the basis of value of the field which is using dependencies keyword.

An interesting scenario could be where you have an enumeration and want to choose another fields value based on the provided value.
Suppose we have scenario as below:

1. we have field minimumEligibilityAge, graduate and courseName.
2. Field minimumEligibilityAge will be used iff person is not graduate otherwise field courseName is used to provide the graduation course name.

Our schema without dependencies could be as below:

	{
	  "title": "Test dependncies",
	  "readOnly": false,
	  "$schema": "http://json-schema.org/draft-04/hyper-schema",
	  "description": "This a test to show how we can use dependencies in json schema",
	  "properties":{
		   "graduate": {
			  "title":"Graduate?",
			  "type":"string",
			  "enum":["Yes","No"]
		  },
		  "minimumEligibilityAge":{
			  "title":"Enter Age",
			  "type":"number"
			  },
		  "courseName":{
			  "title":"Enter Graduation Course Name",
			  "type":"string"
		  }
	  },
	  "type": "object",
	  "required":[
		  "graduate"
	   ]
	}

If we have to see a ui representation of json schema for better understanding, it would be like in image below :

![_config.yml]({{ site.baseurl }}/images/posts/jsonSchemaUiPresentation.png)


In the form boolean is represented by checkbox and number and string are represented by text box.
Now, let’s add dependencies to graduate field for above scenario.

	{
	  "title": "Test dependncies",
	  "readOnly": false,
	  "$schema": "http://json-schema.org/draft-04/hyper-schema",
	  "description": "This a test to show how we can use dependencies in json schema",
	  "properties":{
		  "graduate": {
			  "title":"Graduate?",
			  "type":"string",
			  "enum":["Yes","No"],
			   "options": {
			   "dependencies":[
				  {"id":"minimumEligibilityAge","value":"Yes"},
				  {"id":"courseName","value":"No"}
				  ]
			   }
		  },
		  "minimumEligibilityAge":{
			  "id":"minimumEligibilityAge",
			  "title":"Enter Age",
			  "type":"number",
			  "options":{"hide_display":true}
			  },
		  "courseName":{
			  "id":"courseName",
			  "title":"Enter Graduation Course Name",
			  "type":"string"
		  }
	  },
	  "type": "object"
	}

A ui representation for both the cases could be:
*Case 1:* when person is graduate:


![_config.yml]({{ site.baseurl }}/images/posts/jsonSchemaWhenPersonIsGraduate.png)

*Case 2:* when person is not graduate

![_config.yml]({{ site.baseurl }}/images/posts/jsonSchemaCasePersonNotGraduate.png)

The UI representation that we have used to illustrate the scenario is from JDorn. A temporary link for reference is [jdorn json editor](https://github.com/jdorn/json-editor). Alternatively, there are several ui representation frameworks available for json schema to html conversion. Also, as json schema is a standard, you could write a framework of your own and customise it.
Hope it helps!
