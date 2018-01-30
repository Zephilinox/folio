---
layout: post
title: Dialogue System
date: 2018-01-30 T20:00:00.000Z
description: Creating the dialogue system for our birdman game
published: true
category: LLP
tags:
      - LLP
      - Birdman
---

# Dialogue System

In order to ensure we could create our game, I planned and created the basis of the dialogue system for it. Though the dialogue system has deviated from the plan, and will likely deviate again from it's current state based on our needs, the plan itself was helpful in getting it started.

```C++
basic idea:

Have a DialogueTree, where you can add Dialogue
A dialogue has a name and functions it calls to get appropriate strings and perform actions
You assign a lambda (or ordinary functions) to return the dialogue string, as well as one to determine the next dialogue to play
Dialogue tree creates/maintains actors. actors have set/get flag functions for bools
based on mount & blade dialogue scripting

Example Code:

DialogueTree dialogues;

dialogues.addDialogue("strange_npc", "start",
[&](Actor& actor)
{
	if (!actor.getFlag("met_player", false)) //set it to false if it doesn't exist. Otherwise, exception thrown.
	{
	
		return "What? What do you want? Leave me be, peasant. I have no time for beggars.";
	}
	
	return "What? You're still here? What is it now.";
},
[&](Actor& actor)
{
	if (!actor.getFlag("met_player"))
	{
		actor.setFlag("met_player", true);
	}
	
	return "start2";
});

dialogues.addPlayerOption("strange_npc", "start2",
[&](Actor& actor)
{
	if (actor.getFlag("has_hat"), false)
	{
		return "Where did you get that hat?"
	}
	
	return ""; //empty string means invalid dialogue
},
[&](Actor& actor)
{
	return "start_hat";
})

dialogues.addPlayerOption("strange_npc", "start2",
[&](Actor& actor)
{
	return "Nothing, nevermind.";
},
[&](Actor& actor)
{
	return "bye";
})

dialogues.addDialogue("strange_npc", "start_hat",
[&](Actor& actor)
{	
	return "Ah, that's a story for another day.";
},
[&](Actor& actor)
{
	return "";
});

dialogues.addDialogue("strange_npc", "bye",
[&](Actor& actor)
{	
	if (dialogues.getPlayer().getFlag("super_cool_sword_thingy", false)
	{
		actor.setFlag("plans_to_steal_sword", true);
		return "Seeya...";
	}
	return "Seeya.";
},
[&](Actor& actor)
{	
	if (actor.getFlag("plans_to_steal_sword")
	{
		//do steal sword cutscene if we had a cutscene thingy to do this
	}
	
	//returning an empty string means the dialogue is over
	return "";
});

dialogues.play("strange_npc", "start");


class Actor
{
public:
	const std::string& name;
	
	bool getFlag(const std::string& flag)
	{
		return flags[flag];
	}
	
	bool getFlag(const std::string& flag, bool value)
	{
		if (!flags.count(flag)
		{
			setFlag(flag, value);
		}
		
		return getFlag(flag);
	}
	
	void setFlag(const std::string& flag, bool value)
	{
		flags[flag] = value;
	}
	
private:
	unordered_map<std::string, bool> flags;
}

class Dialogue
{
public:
	Actor actor;
	const std::string& name;
	function text;
	function next_dialogue;
}

//issue get dialogue shouldn't be unique name for actor etc
//have multiple dialogues with the same name if the actor is the player, in this case a list of dialogue options is displayed for the player to choose from

class DialogueTree
{
public:
	bool addDialogue(const std::string& actor, const std::string& name, function text, function next_dialogue)
	{
		if (dialogues.count(name))
		{
			return false;
		}
		
		Dialogue d;
		d.name = name;
		d.text = text;
		d.next_dialogue = next_dialogue;
		
		Actor a;
		a.name = actor;
		d.actor = a;
		
		dialogues[name] = d;
	}
	
	bool addPlayerDialog(const std::string& actor, const std::string& name, function text, function next_dialogue)
	{
		if (dialogues.count(name))
		{
			return false;
		}
		
		Dialogue d;
		d.name = name;
		d.text = text;
		d.next_dialogue = next_dialogue;
		
		Actor a;
		a.name = actor;
		d.actor = a;
		
		dialogues[name] = d;
	}
	
	Dialogue* getDialogue(const std::string& name);
	{
		if (dialogues.count(name))
		{
			return &dialogues[name];
		}
		
		return nullptr;
	}
	
	Dialogue* getCurrentDialogue();
	{
		return current_dialogue;
	}
	
private:
	std::unordered_map<std::string, Dialogue> dialogues;
	Dialogue* current_dialogue;
}

```