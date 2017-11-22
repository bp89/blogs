---
layout: post
title: 'UI-Automation : Creating Standalone Nightwatch - Cucumber - Chrome - NPM integration application'
categories: [QA, Engineering]
tags: [Automation, UI-automation, Testing ]
comments: true
---

There had been times when Manual testers were stealing the show. But as applications started growing and new approaches 
started taking front seat, job of  a QA has grown to test all parts of application and then their integration. Being a Product Owner QA also needs to ensure that
developed software is matching client expectations. 

Hence, arises a need to write acceptance test. Manual Testers used to manage a huge database of such tests and execute those after each feature integration. But this
again was tedious and time consuming. Hence, automation was ultimate solution. An application could be automation tested using different appraoches nowadays. One of my favourite is
using cucumber to write the Given-When-Then style test cases. Alternatively AAA i.e. Avail, Act and Assert. On other hand nightwatch beautifully integrates especially for
people having knowledge of javascript. 

Let's straight away create a sample application and your first automation test.

If you are not aware how cucumber works. I would recommend you first go through cucumber and how it works.

First thing first, 

1. Create a diretory say **ui-automation**
2. Now go to directory and run `npm init` or `yarn init` (yarn is an efficient parallel to npm package manager. Read more [here](https://yarnpkg.com/en/)) 
3. It will prompt a interactive commandline. Let's just click enter till `npm init` finish.
4. You should now notice `package.json` file is created inside ui-automation directory something like below:-

		{
		  "name": "ui-automation",
		  "version": "1.0.0",
		  "main": "index.js",
		  "license": "MIT",
		  "dependencies": {
			
		  },
		  "devDependencies": {}
		}
5. Now let's add nightwatch dependency by running

> npm install nightwatch 

or 

> yarn add nightwatch

6. Also, add dependency for `cucumber`, `nightwatch-cucumber`

> npm install cucumber nightwatch-cucumber

Now, package.json should look like below:-

{
  "scripts": {
    "nightwatch": "nightwatch"
  },
  "dependencies": {
    "cucumber": "^3.1.0",
    "cucumber-pretty": "^0.0.5",
    "minimist": "^1.2.0",
    "nightwatch": "^0.9.16",
    "nightwatch-cucumber": "^8.2.10"
  }
}

Notice we have added a script as well `"nightwatch": "nightwatch"`. It will allow us to run test cases by running `yarn nightwatch` or `npm run nightwatch`

7. Now let's create a bin directory inside ui-automation and download and put following inside `bin` directory:-
	i. selenium standalone server jar from [here](http://www.seleniumhq.org/download/). You could also download non-java versions but let's stick to java for now.
	ii. chrome driver from [here](https://sites.google.com/a/chromium.org/chromedriver/downloads) 
	
8. Now add a directory say `tests`. Here, we'll put our test cases.

9. Now add a `nightwatch.config.json`  as below

		module.exports = require('nightwatch-cucumber')({
			cucumberArgs: [
				'--require',
				'step_definitions',
				'--format',
				'node_modules/cucumber-pretty',
				'--format',
				'json:reports/cucumber.json',
				'features'
			]
		});
		module.exports = {
			"src_folders": [
				"test-cases"
			],
			"output_folder": "reports",
			"custom_commands_path": "",
			"custom_assertions_path": "",
			"page_objects_path": "./pageObjects",
			"globals_path": "globals.json",
		
			"selenium": {
				"start_process": true,
				"server_path": "../bin/selenium-server-standalone-3.7.1.jar",
				"log_path": "",
				"port": 4444,
				"cli_args": {
					"webdriver.chrome.driver": "../bin/chromedriver"
				}
			},
		
			"test_settings": {
				"default": {
					"launch_url": "http://localhost:4000",
					"selenium_port": 4444,
					"selenium_host": "localhost",
					"silent": true,
					"screenshots": {
						"enabled": true,
						"path": "screenshots"
					},
					"desiredCapabilities": {
						"browserName": "chrome",
						"marionette": true,
						"javascriptEnabled": true,
						"acceptSslCerts": true,
						"chromeOptions": {
							"args": [
								"start-fullscreen"
							]
						}
					}
				}
			}
		};

Notice, `cucumberArgs` which specify cucumber configuration. Second argument from top is to specify step definition path. Last value in the array is path to directory where cucumber features will reside.
`src_folders` specifies the directory where test cases will be looked up.
`page_objects_path` is used to specify nightwatch pageObjects. You could refer nightwatch official website to understand pageObjects.
`globals_path` is used to specify file where we'll keep the global variables which we can access throughout application. Add a file named globals.json inside root folder &  add following

		{
		  "waitTime": 1000
		}
`selenium` section is used to configure selenium i.e. what port and host it runs on.
`cli_args` inside selenium section is to specify webdriver path. We have specified for chrome for now. 
`test_settings` is used to specify test configurations e.g. what driver to use and how to use. It contains a `default` object which is to specify default settings



 Now add folders named `features` & `step_definitions` inside root folder.
 
 Let's add our first feature `google.feature` as below:-
 
 Feature: Google Search
 
	   Scenario: Searching Google
	 
		 Given I open Google's search page
		 Then the title is "Google"
		 And the Google search form exists

Notice right after `Given`, `Then` and `And` are the sentences which are known as in cucumber step definitions. Let's now write step definitions for google.feature.
 
	 const {client} = require('nightwatch-cucumber');
	 const {defineSupportCode} = require('cucumber');
	 
	 defineSupportCode(({Given, Then, When}) => {
		 Given(/^I open Google's search page$/, () => {
		 return client
			 .url('http://google.com')
			 .waitForElementVisible('body', 1000);
	 });
	 
	 Then(/^the title is "([^"]*)"$/, (title) => {
		 return client.assert.title(title);
	 });
	 
	 And(/^the Google search form exists$/, () => {
		 return client.assert.visible('input[name="q"]');
	 });
	 
	 });

Like i mentioned earlier you should know how cucumber works in advance. But let's try to understand above test case. We we declared a step `Given`, in callback we are saying
client.url to navigate to  `http://google.com` url and then wait for 1000 ms for body tag/element to appear.

`Then` check the title  passed in feature is matching`regex`. Similarly inside And we are asserting `input[name="q"]` to be present.


Now finally run `yarn nightwatch`. Also, you could install cli for nightwatch that will allow nightwatch running directly using nightwatch command.


Below is the finall folder structure :- 

	├── bin
	│   ├── chromedriver
	│   └── selenium-server-standalone-3.7.1.jar
	└── tests
		├── features
		│   └── google.feature
		├── globals.json
		├── nightwatch.conf.js
		├── package.json
		├── pageObjects
		├── step_definitions
		│   └── google.js
		├── test-cases
		│   └── google.js
		└── yarn.lock


It might be a time taking affair initially as you'll have to understand cucumber, nightwatch pageObject etc.


Hope it helps!



