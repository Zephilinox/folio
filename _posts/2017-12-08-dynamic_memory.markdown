---
layout: post
title: Procedural Generation
date: 2017-12-08 T18:00:00.000Z
description: Determining how to procedurally generate obstacles
published: true
category: LLP
tags:
      - LLP
      - Endless Runner
---

# Procedural Generation
So in this endless runner, I've decided to mostly copy the Moonlight Mori game that was linked as an example on the game brief.

In order to replicate the way it works, procedurally, I've made an entity spawner that starts off in the middle of the screen and then moves randomly left and right.

Sometimes the spawner will spawn a good spawn, a bad spawn, or nothing at all. This is a pretty simple way of spawning these obstacles procedurally.
By modifying the chance of the obstacles spawning, I can increase the difficulty over time.

# Differences
While Moonlight Mori is the inspiration, it's a very simple game that doesn't really increase in difficulty.
I plan to change this by increasing the speed at which obstacles spawn, or increasing the rate at which the player "moves down" (really, everything else is just moving up)

I also plan to implement obstacles which move left to right, which is something not present in the moonlight mori game.

I believe with these adjustments, and a harder scaling on the odds of a bad orb spawning over time, I can make a much more difficult endless runner.

# Random Chance
In order to implement all this, I've used the C++11 random generators.

```C++
	std::random_device rd;
	std::mt19937 mt;
	std::uniform_real_distribution<float> random_percentile;
```

which I then use like so:

```C++
	//Random chance to spawn or not to spawn
	float percentage = random_percentile(mt);
	SpawnState ss = SpawnState::NONE;
	if (percentage < 0.4f)
	{
		ss = SpawnState::GOOD;
	}
	else if (percentage < 0.8f)
	{
		ss = SpawnState::BAD;
	}

	//Random chance to move specific amounts
	//subtract 0.5 to make it negative 50% of the time, thus move left
	float distance_chance = random_percentile(mt) - 0.5f;
	float distance = TILE_SIZE * 4 * distance_chance;
```

This is much better than the C-style rand() function, which has certain issues and limitations.

Furthermore, here is a copy of my initial thoughts regarding this project. It's likely a lot of this will change as I work through the project.

# Initial Thoughts

Endless Runner based on Moonlight Mori

320x640
32x32 tiles
(10x20)

Player stands still but can move left/up/down/right
Everything else moves up to make it seem like the player is running downwards constantly
Pick up orb for 1 point
Hit obstacle to die

every 10 points, game increases in speed by 1%
every 20 points, game creates more obstacles and less points
at 50 points, some obstacles move
at 100 points, move obstacles move
at 200 points, most obstacles move
at 400 points, all obstacles move

Procedural generated level (such as obstacle placement)
	In order to spawn obstacles do this:
	start off at the bottom-middle of the screen. every tick of some kind, randomly decide to move left, right, not move. based on choice, move a random amount between two values. randomly decide if it spawns obstacle or point. This creates a moving side-to-side path that spawns obstacles or points at at random and with random left/right spacing between each one, though up/down spacing remains consistent

Start Menu, Game Over Screen
	Easily done

High Score Table, Persistent. (save to file)
	Simple enough, just read through a file, load values in to vector, sort them, then display top 3
	file has 1 value per line, so 3 lines
	when the player hits the game over screen, delete the file, swap current score if it's larger than the lowest score, then recreate the file with the values.
	
Images
	need 32x32 ground texture
	need 32x32 player character
	need 32x32 point/obstacle
	
Classes
	GameState
		Score
			Keep track of score, difficulty
		Player
			Movement
		EntitySpawner
			Keep track of spawn line pos, determine where it moves, and create either a GoodOrb or BadOrb
		Collisions
			Determine if player is colliding with entities from the EntitySpawner
			
