---
layout: post
title: Dynamic Memory
date: 2017-10-19 T20:00:00.000Z
description: Fixing issues relating to incorrect usage of dynamic memory
published: true
category: LLP
tags:
      - LLP
      - Endless Runner
---

todo

```C++
InputManager* GameData::getInputManager()
{
	return &input_manager;
}

StateManager* GameData::getStateManager()
{
	return &state_manager;
}
``
