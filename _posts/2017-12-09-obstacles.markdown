---
layout: post
title: Obstacles
date: 2017-12-09 T14:00:00.000Z
description: Obstacles and their behaviour
published: true
category: LLP
tags:
      - LLP
      - Endless Runner
---

# Obstacles

So in this game there are a variety of obstacles that I've called "Pickup", these are good (scoring points), or bad (killing you and ending the game). The bad obstacles can also move, or remain stationary like good obstacles. I may even make the good obstacles move in the future, I'm unsure.

The way I'm doing it right now is to have the pickup class get passed an ENUM determining which type it should be, and changing behaviour based on that. I don't like this, but it's a quick and effective way of doing it. Ideally I would use either a factory or strategy pattern, as well as pooling (but that's a different concern)

It feels really hacky doing it the way I've done it, and it is, but for the purposes of this game I think it's sufficient.