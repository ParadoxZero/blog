---
layout: post
title:  "High-performance web server & caching strategy"
date:   2025-03-16
tags: design scalability
categories: code
author: Sidhin S Thomas
published: false
---

I'll talk about a system design problem and how to approach it from a Software Architect's point of
view. 

# Problem statement

Design a high-performance, highly scaled, cross - geo, web service which would be accessed by 
millions of users. For reference we can consider a service like Reddit.

## Non Functional Considerations (Requirements)

While we won't be analysing the actual functional design of the web service, we need some idea about
the non-functional ones.

Priority - 
1. High availability - The core services should always available for users to consume.
2. Reliability - The content created by the users should not be lost or corrupted.
3. Constistancy - It's okay if posts/ comments are displayed inconsitantly. 

### Availablity SLA

Generally we come up with SLAs for the services in terms like percentages. Sometimes called 
"five nines".

| Availability SLA (%) | Allowed Downtime per Year      |
|----------------------|--------------------------------|
| 99.0%                | 3 days, 15 hours, 36 minutes   |
| 99.5%                | 1 day, 19 hours, 48 minutes    |
| 99.9%                | 8 hours, 45 minutes, 36 seconds|
| 99.95%               | 4 hours, 22 minutes, 48 seconds|
| 99.99%               | 52 minutes, 35 seconds         |
| 99.999%              | 5 minutes, 15 seconds          |
| 99.9999%             | 31.5 seconds                   |

Getting to such SLA involves things more than just the design of system. We need to design the 
processes, investments and resources to match the SLAs. 

Things like Software Lifecycle process - Agile/ Waterfall, testing stragetgies, A/B testing, 
Feature rollouts impact the SLA much more than the system itself.


### Numbers

Assume an approx Daily active users – `1 Million`.

Assuming everyone uses the app for 5 minutes in average, with about 1 request (user action) every 30
seconds. Which means Every user will generate `(5 * 60) / 30 = 10` requests per day.

If the spread of load is even throughout the day it means – `1 million * 10 = 10 Million` requests 
per day. This becomes – `10,000,000/24*3600` requests per second `~ 0.1` transactions per second 
or 1 transaction every 10 seconds.

However, in real life we don’t get even spread, chances are people use social media like Reddit off
work hours more than office hours. In short we have to plan for a range of traffic instead of one.

This is especially relevant in modern cloud based architectures where you can scale up and down 
based on heuristics and metrics, which can save significant amount of costs.

Ideally we will come up with actual numbers based on historic data and user behaviour. But we need 
something to start with. So let's assume a speard of 10:1 peak vs average. 

#### Final Numbers

| Scenerio      | Expected Traffic |
| --------      | ---------------- |
| Average RPS   | 0.1 rps          |
| Peak RPS      | 10 rps           |

The task at hand is to balance the expected load with the non-functional requirements in **minimum 
cost possible**. A system that supports the team to achive SLA given perfect execution of the 
software process.

Because it's always possible to get more and more power machines, throw money into fancy SAAS 
offerings to manage the traffic. The trick is to do it effeciently. 

# Design

We will
