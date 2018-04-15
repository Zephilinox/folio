---
layout: post
title: Ropes & UI
date: 2018-03-13 T17:00:00.000Z
description: No more ropes. Iron Man...?
published: true
category: P&G
tags:
      - P&G
      - VR
---

# Ropes

So I managed to convince the group that the difficulty in implementing rope physics was too high. We could've done it, but it would've taken a lot of effort.

Our little rope demo was completely broken, both in the feel of the rope and in the fact the physics kept spazzing out.

<img src="https://i.imgur.com/9I4SVBs.png"> 

# Iron Man

After deciding ropes weren't going to work for us we discussed other options.

The group didn't want to go down the route of making a game, and our other ideas didn't really work, so ultimately we decided to follow Lloyd's previous remarks about an Iron Man UI-style system.

Although I'm not sure it'll be an "Iron Man" UI, we are intending to do some different stuff with it. Looking at Diegetic, non-diegetic, and spatial UI.

Screen-Space UI is unworkable due to it being too close to the screen for VR users, making them feel sick or not enjoying it.

<img src="https://i.imgur.com/RC7qWvW.png">

We created a demo in which we had Diegetic UI with different z-depth elements. In this case, each button is a different distance back from the camera.

What we'd like to do is implement parallax, so that as you rotate your view the elements closer to you will rotate faster, the ones further back will rotate slower.

We also managed to make the UI elements interactible, so that you can move them around your view, including towards/away from you, by using the motion controllers.
