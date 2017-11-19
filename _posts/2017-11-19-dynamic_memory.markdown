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

# Snake Feedback

As part of my snake feedback I've taken a closer look at dynamic memory and how it's being handled in my codebase for the endless runner. I've ported most of my architecture to the endless runner codebase and modified how dynamic memory was used.

For instance, game_data used to have a shared_ptr to the InputManager and StateManager, but these managers don't need to be allocated on the heap, the stack is sufficient, so I removed that entirely. In order to accomplish this, I now return a pointer to the managers by using the address-of operator

One of the consequences to doing this was that the functions could no longer be declared const. This shouldn't be an issue since using the Input Manager and State Manager would usually not be done in a const function, but I may have to do a workaround for this if it arises. A simple solution would be to just make them volatile.

## Before

```C++
std::shared_ptr<InputManager> GameData::getInputManager() const
{
	return input_manager;
}

std::shared_ptr<StateManager> GameData::getStateManager() const
{
	return state_manager;
}
```

```C++
InputManager* GameData::getInputManager()
{
	return &input_manager;
}

StateManager* GameData::getStateManager()
{
	return &state_manager;
}
```

todo
