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

#### GameData.cpp
##### Before

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

##### After

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

Futhermore game_data itself was being held and passed around as a shared_ptr, but that isn't necessary since the owner of game_data (the game) outlives everything that uses it. Instead, I've made game_data a unique_ptr which better suits its needs, and it is now passed around as a raw pointer.

game_data could have been allocated on the stack as I did previously with InputManager and StateManager, however the constructor requires passing the renderer, which isn't in a valid state before it is initialised properly.

As I write this I realise I could just set the default renderer pointer to nullptr and assign the game as a friend class of game_data so I can call a protected function where I can pass in the renderer once it's initialised. I'll go do that now...

### Done

#### GameData.hpp

```C++
class GameData
{
	friend class EndlessGame;

public:
	GameData() = default;

	ASGE::Renderer* getRenderer() const;
	StateManager* getStateManager();
	InputManager* getInputManager();

private:
	void setRenderer(ASGE::Renderer* renderer);

	ASGE::Renderer* renderer = nullptr;

	/** Input Manager. A wrapper around key states from callbacks by ASGE::Input. */
	InputManager input_manager;

	/** State Manager. A wrapper around storing states in a stack. */
	StateManager state_manager;
};

Game.hpp no longer creates it as a unique_ptr, and Game.cpp had to be changed so that accessing it used the object access operator rather than pointer access operator (-> to .). Game.cpp now sets the renderer once it's initialised successfully, and game_data is passed to the states like so:

```C+++
	//IMPORTANT: MAKE SURE RENDERER IS INITIALISED, WHICH IT MUST BE BY NOW, BEFORE DOING THIS
	game_data.getStateManager()->push<MenuState>(&game_data);
```
	
As the lovely little warning comment in full caps explains, there is one drawback to this system. It's possible for the state manager to be created and a state to be pushed (which requires game_data) before the renderer in game_data is ready, meaning a state could then try to access it (unsuccessfully). Before the constructor for game_data ensured the renderer would always be valid. Personally I'm not sure if removing the need for game_data to be a unique_ptr is worth this extra consideration, however it's done now so I'll be keeping it.
