---
layout: post
title: 'Quick start with Springboot using CLI'
tags : java , Spring Boot, Type Inference
---

SpringBoot is one of the main projects of Spring community which takes opinionated view of production ready applications. It helps you getting up and running a production grade spring application.
It comes with even a CLI to get you quick started with springboot. CLI stands for Command Line Interface which gives *spring* command using which the application could be run.
This CLI allows you to create a groovy script called Spring script in Springboot documentation which could be run as a spring application using simple command

> spring run file_name.groovy

A simple example of a Spring script could be as below:

{% highlight java linenos %}
    @Controller
    class Example {
        @RequestMapping("/")
        @ResponseBody 
        public String helloWorld() {
            "Hello Spring boot audience!!!"
        }
    }
{% endhighlight %}

This script just by default imports few annotation classes(e.g. Controller,ResponseBody and RequestMApping above) and add annotations when run with CLI. In normal spring application without a springboot CLI but with springboot would require to add the imports for these.
    
Below are the annotations added itself by CLI.

```groovy 
    @Grab("org.springframework.boot:spring-boot-web-starter:0.5.0")
    @EnableAutoConfiguration
```


`@Grab` annotation is used to add a starter pom. A starter pom is nothing but a collection of jars dependencies and plugins grouped together.
Added above is the web starter which provides sensible defaults for a spring web applications.

`@EnableAutoConfiguration` enables default confiurations and best tries to configure the application by looking up in added jar files. For example if you have embedded a tomcat-embed.jar then it considers that you want to use tomcat.

More, you can create FAT jars(A fat jar is simply a jar that contains all the required jars within single jar and hence can be run independently) using command below



> spring jar test.jar file_name.groovy


This will create a excutable jar which could be run using following basic java command

> java -jar jar_file

Springboot CLI could be installed if using GVM by running following command:

> gvm install springboot

gvm is independent of OS. Though if using MAC and using Homebrew, all you need to do to install the Spring Boot CLI is:

> $ brew tap pivotal/tap
> $ brew install springboot

Homebrew will install spring to _/usr/local/bin_

*Note:-* If you don’t see the formula, your installation of brew might be out-of-date. Just execute

> brew update

and try again.

If you are on a Mac and using MacPorts, all you need to do to install the Spring Boot CLI is:

> $ sudo port install spring-boot-cli 

Least but not last, springboot CLI should not be used for production stuff and just for testing and hands on purpose.
Finally, springboot CLI is no way required or must to run a springboot rich spring application, it is just a hands on tool.

