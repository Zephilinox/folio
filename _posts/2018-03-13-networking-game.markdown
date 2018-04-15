---
layout: post
title: Networking Game
date: 2018-03-13 T16:00:00.000Z
description: Our plans for the fifth game
published: true
category: LLP
tags:
      - LLP
      - Game5
---

# Game 5

<img src="https://i.imgur.com/VbiESOT.png"> 
 
We've started talking about tasks that require implementing and assigning them to each person.

I'll be handling all of the network related stuff.

At the moment my intention is to continue with the "Peer to Peer" model where the server and the client are combined (often referred to as a "Host")

In order to facilitate the sending of information, a thread safe queue will need to be created so that when receiving data, the packet information is pushed to a queue.

At the start of the frame, if there's anything in the queue then it will get sent to the appropriate entity based on the network ID of the entity it was targetting.

By doing it this way, entity's don't need to worry about thread safety when deciding what to do when they receive a packet.

Whether I adapt the existing MessageQueue we have to fulfil this purpose by making it thread safe will depend on if I want to replace the usage of Signals & Slots.

Replacing it means that making it thread-safe will be easier and that we won't have as many issues getting it to run on the lab PC's, but on the other hand it means less flexibility.

We've also decided the game will be about the Wild West, and we're thinking it'll be more like XCOM.

I'm not sure we'll end up with a finished game, the scope here is quite large and I don't think everyone will be pulling their own weight in the group.