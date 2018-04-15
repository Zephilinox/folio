---
layout: post
title: Networking Game - Update
date: 2018-04-15 T16:00:00.000Z
description: Status of our networkign game
published: true
category: LLP
tags:
      - LLP
      - Game5
---

# Game 5

It has been one month since myself and chris started working on a networked game for LLP.
Neither liam nor brendon have actually contributed at all yet.

Brendon may contribute something in the future as LLP has been extended by a week, and chris has done some neat work recently on getting fog of war in to the game, however I've not done any work for at least a week and don't plan on doing any more.

The game is in a fairly unplayable state as it is, though there are some elements of game play it isn't by any means a finished game. The majority of my work has been on the lower level aspects of the game, such as all the architectural bits, including implementing networking.

Actually I ended up implementing networking twice, as the original way I had set up client/server didn't sit well with me. Now hosts have a dummy client that communites directly with their hosted server.

The architecture behind the networking implementation is sound, but incomplete. As a result some of the code became quite messy with workarounds being added due to limitations in my networking implementation. Well, the limitation is more to do with the "scene graph" (it's definitely not a graph), which wasn't developed at all properly, as such Units didn't have any way of accessing other entities and systems in the game, so some of their functionality that required networking was implemented in the game state directly.

I would've liked to have spent more time on it and refining it, perhaps I'll do so in the future.

Chris has done a great job implementing basically the entirety of the gameplay, map, and sprites in the game. This project wouldn't be anywhere close to where it is now without his effor, I just wish I could say the same for the other half of our group.

I've enjoyed working on networking more than I expected and learnt a lot from working out the implementation of it myself. I feel like I have a much better, though still incomplete, understanding of networking solutions present in existing game engines such as Unity or Godot. I would've liked to have implemented RPC's but was unable to figure out an effective solution for it. Something I'd like to do in the future is take a look at the GameNet github repo and see how that's handling RPC's with their use of templates.