---
layout: post
title: "Truncating SQL DateTime in SQL Server 2008"
date: 2016-05-22 12:40:08 -0400
comments: true
categories: [Today I Learned]
---

Recently a coworker was writing a T-SQL script to compare ten-minute timeslices against each other. He was trying to figure out an easy, performant way to remove the ones place so that he could treat all timeslices from 01:00.00 to 01:09.99 as the same datetime, and so on. I found a few StackOverflow resources like <a href="http://stackoverflow.com/questions/8896663/a-way-to-extract-from-a-datetime-value-data-without-seconds">this one</a> but none that did exactly what I wanted. I adapted those with some trial and error into this:
<!-- more -->

{% codeblock Ten Minute Time Slices - TIL.sql %}
DECLARE @testDate DATETIME = '2016-05-13 12:57:21.097'
 
select dateadd(minute, (datepart(minute, @testdate) / 10) * 10, dateadd(hour, datediff(hour, 0, @testDate), 0)),
	   dateadd(minute, (datepart(minute, @testdate) / 10) * 10 + 10, dateadd(hour, datediff(hour, 0, @testDate), 0))
{% endcodeblock %}