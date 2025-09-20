---
title: "Is taking the toll worth it?"
meta_title: "A technical and reflective look at whether paying for a toll road is worth the time saved"
description: "A reflective and technical look at whether paying for a toll road is worth the money compared to the free road"
date: 2025-09-19T12:00:00Z
image: "/images/notificaciones-carretera/carretera-mexico-toluca-maps.png"
categories: ["Projects", "Engineering"]
author: "Erick López"
tags: ["traffic","python","selenium","paying","highway"]
draft: false
---
# Is taking the toll worth it?

![carretera mexico toluca](/images/notificaciones-carretera/carretera-mexico-toluca-maps.png)

I drive the México–Toluca highway every day to get to work, and I often wonder whether paying the toll is always worth it.

When I'm in a hurry I tend to take the toll road, but I'm not the only one: many times the toll booth line is long and, when I see the free road clear, I think it wasn't worth paying.

## First solution (manual)

When I'm with someone I ask them to check traffic on Google Maps and tell me whether to take the toll or the free road. It's practical because we both know the route and can decide before we reach the fork.

However, this fails when I travel alone: it's unsafe to handle the phone or Android Auto while driving.

## Proposed technical solution

Since I don't always have a passenger, I looked for a way to get that information automatically. Android Auto can read messaging notifications via TTS (Text To Speech), so the idea was to receive a message with the recommendation and have it played aloud in the car.

For scheduled notifications I prototyped a solution using Selenium to control WhatsApp Web from a secondary session.

With an additional SIM and account I can sign in to a browser controlled by Selenium and send messages to my personal number with the recommendation ("Take the free road" / "Take the toll road").

### Defining routes and decision points

My commute is quite regular: I work Monday to Friday and pass the same decision points. The first step was to identify in Google Maps the points where the routes split and where they converge again so I could measure travel time for each option.

![punto de desviación en la carretera con la posibilidad de eligir cuota o libre](/images/notificaciones-carretera/desviacion-carretera.png)

Google Maps encodes coordinates in the URL, which makes it easy to define the segments I want to measure and compare:
[https://www.google.com/maps/dir/19.3005883,-99.3786061/19.3903276,-99.2390682/...](https://www.google.com/maps/dir/19.3005883,-99.3786061/19.3903276,-99.2390682/@19.3630724,-99.3662328,12.5z/data=!4m5!4m4!1m1!4e1!1m0!3e0!5m1!1e1?entry=ttu&g_ep=EgoyMDI1MDkxNy4wIKXMDSoASAFQAw%3D%3D)

### Rules and heuristic

I don't want to calculate minutes while driving: I need a simple recommendation.

I defined a simple rule: if taking the toll saves me more than 20 minutes compared to the free road, take the toll; otherwise, take the free road. The rule compares estimated travel time and the fixed toll cost.

![codigo regla](/images/notificaciones-carretera/codigo-regla.png)

### Implementation and notifications

I preferred not to rely on paid messaging platforms due to cost and privacy concerns.

Although I'm a .NET developer, I prototyped with Python and Selenium to automate WhatsApp Web and send notifications. Persisting the browser session lets the bot act without signing in every time.

The flow is:
- measure estimated time for both routes using Maps APIs or simulating web queries;
- apply the heuristic (time difference vs cost);
- send a message to my number with the recommendation;
- Android Auto plays the notification via TTS.

## User experience

The system gave me the information when I needed it and worked well, though not always: maintaining automated WhatsApp Web sessions and a home service (I ran it on a k3s cluster in a mini-PC) requires upkeep.

I learned a lot about Selenium, session persistence and deploying to Kubernetes. It was more work than I expected, but perfect for learning.

![notifiaciones whatsapp](/images/notificaciones-carretera/notificaciones-whatsapp.png)

## Monetization attempt

I ran a pilot with users from a Facebook group where people share highway reports. I thought others might have the same problem; I estimated the service could scale to about 30 people.

I posted the invite several times and got four users for the trial. Over three weeks I sent periodic surveys; only two people responded.

![introducción a los usuarios](/images/notificaciones-carretera/intro-usuarios.png)

At the end of the trial I thanked participants and proposed a monthly subscription, but no one agreed to pay for the service.

## Reflection

Although no one was willing to pay, the project left me with a lot of learning: automation techniques with Selenium, session management, Kubernetes deployment, and the difficulty of promoting a service on social media.

It also reminded me that even if something isn't profitable, it's worth doing if you learn and gain practical experience.