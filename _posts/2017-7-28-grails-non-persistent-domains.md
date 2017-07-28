---
layout: post
title: Making a domain non-persistent
tags: Groovy, GORM, Grails, MapWith
---

In grails app, there might come scenarios where one need to create a non-persistent domain rather than creating a command obejct or POJO / POGO.
GORM comes with a handy static property mapWith which has default value GORM (which associates any domain with gorm persistence layer).
To make a domain non persistent set mapWith=”none”. For example, see Ticket domain below

    public class Ticket {
		String id
		List<Long> productInstanceId
		static hasMany = [productInstanceId:Long]
		static mapWith = "none"
	}

Advantages:-

1. Still behaves like a GORM Domain in most of the cases like Ticket domain above is still accessible by GrailsDomainClass
2. When using multiple database , still respects the convention over configuration policy.
3. Supports scaffolding while same can’t be done for command objects.

Also, at some point of time to check on a particular domain that what type of mapping it has, fetch the static property with domain class. For the domain above, it would be

>Ticket.mapWith

or

>Ticket.class.mapWith

If doing a dynamic check then can find it with the help of DefaultGrailsDomainClass.

    GrailsDomainClass domainClass = new DefaultGrailsDomainClass(Ticket.class)
    domainClass.mappingStrategy
