---
layout: post
title: Working with SimpleDateFormat
tag: Java, Date Format, Calendar
---

Here is how, you can get different date format using SimpleDateFormat and Calendar class.

	public class SDFTest {
		public static void main(String[] args) {
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
			Date myDate = sdf.parse('2012-03-05');
		}
	}   
    
Below are some of the formats you can use.  

| __Date Format__ 					| __Example__							|
| yyyy.MM.dd G 'at' HH:mm:ss z 		| 2001.07.04 AD at 12:08:56 PDT 		|
| EEE, MMM d, ''yy 					| Wed, Jul 4, '01						|
| h:mm a							| 12:08 PM								|
| hh 'o''clock' a, zzzz				| 12 o'clock PM, Pacific Daylight Time	|
| K:mm a, z							| 0:08 PM, PDT							|
| yyyyy.MMMMM.dd GGG hh:mm aaa		| 02001.July.04 AD 12:08 PM				|
| EEE, d MMM yyyy HH:mm:ss Z		| Wed, 4 Jul 2001 12:08:56 -0700		|
| yyMMddHHmmssZ						| 010704120856-0700	 	 				|
| yyyy-MM-dd'T'HH:mm:ss.SSSZ		| 2001-07-04T12:08:56.235-0700			|

{% feed_meta %}