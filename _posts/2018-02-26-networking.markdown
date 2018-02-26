---
layout: post
title: Networking
date: 2018-02-26 T20:00:00.000Z
description: Creating the networking system for our framework
published: true
category: LLP
tags:
      - LLP
      - Birdman
---

# Networking System

Due to the next game being multiplayer I felt it was important to get started on implementing that system.
With a base networking system to work off of we can build on it without worrying about it changing too much later on.

Most networking systems work the same way, you send off UDP packets, often with a reliability layer built on top, with payload data that gets unserialized determining what the packet contains.
For instance, you can have a packet that gets sent out when an entity needs to be created on all clients, a "CreateEntity" packet, that then causes all clients to create that entity.
The server will determine what the network identity of the entity is, as well as which client owns it (and can therefore send packets targetting that entity (via its network ID) on other client machines)

There's still work to be done on it, such as handling the creation of entities and whether networked entities and stored with non-networked entities, but the basics are there.

The way it works is that we have a base Entity Class which has a EntityInfo member variable. This is a struct that contains the network ID, the owner ID, and the entity type (i.e. "Paddle" or "Ball")
The NetworkManager handles interfacing with the lower-level enetpp library, that handles the multithreading synchronisation of sending and receiving packets from ENet.
The Packet class handles the serialization and deserialization of POD information via the use of a vector of char's and memcpy.

Networked entities inherit from Entity and then overload the update, render, and packetReceived functions. packetReceived is called when a client/server receives a packet and it's been determined that this particular entity should handle it (i.e. the corresponding owner(master) of that entity on another machine has sent a packet for all non-owning (slaves) to use)

Here is the code for the Paddle class.

```C++

class Paddle : public Entity
{
public:
	Paddle(GameData* game_data)
		: sprite(game_data->getRenderer())
		, game_data(game_data)
	{
		entity_info.type = hash("Paddle");
	}

	void update(float dt) override final
	{
		sprite.update(dt);

		if (entity_info.ownerID == game_data->getNetworkManager()->clientID)
		{
			if (game_data->getInputManager()->isKeyDown(ASGE::KEYS::KEY_W))
			{
				sprite.yPos -= 1000 * dt;
			}
			else if (game_data->getInputManager()->isKeyDown(ASGE::KEYS::KEY_S))
			{
				sprite.yPos += 1000 * dt;
			}

			Packet p;
			p.setID(hash("Entity"));
			p << &entity_info
				<< sprite.xPos
				<< sprite.yPos;
			game_data->getNetworkManager()->sendPacket(0, &p);
		}
	}

	void render(ASGE::Renderer* renderer) const override final
	{
		renderer->renderSprite(*sprite.getCurrentFrameSprite());
	}

	void receivedPacket(uint32_t channel_id, Packet* p) override final
	{
		*p >> sprite.xPos >> sprite.yPos;
	}

	AnimatedSprite sprite;
	GameData* game_data;
};
```

We pass in game-data so it has access to useful managers, including the network manager, which then allows it to send packets.
We set the type of the entity to "Paddle", as a compile time string hash

we override update, which handles what to do when the machine with the entity is the owner (chcking owner ID is equal to clientID). You don't want non-owning clients to move entities with keyboard control for instance, but you still want these non-owners to update sprite animations or particles.

Every update we send a packet via the network manager containing the entity information and the data we're serializing (the sprite position).
The server (or clients if we are the server) receives this, checks the entity information matches its records, and if so sends the packet to that non-owning version of the entity.

```C++

void NetworkingState::onPacketReceived(const enet_uint8 channel_id, ClientInfo* ci, Packet p)
{
	switch (p.getID())
	{
		case hash("Entity"):
		{
			EntityInfo info;
			p >> &info;
			Entity* ent = getEntity(info.networkID);
			if (ent && //exists
				ent->entity_info.ownerID == info.ownerID && //owners match
				ent->entity_info.type == info.type) //types match
			{
				if (!ci || //client received packet, we trust the server
					(ci && info.ownerID == ci->id)) //received packet from client, make sure they aren't lying about what they own
				{
					ent->receivedPacket(channel_id, &p);
				}
			}
		} break;
	}
}

```

By having each entity handling the sending and receiving of it's own packets with the server/clients just checking to ensure the entity info is valid, we don't need to maintain a massive branching function in one location for each type of packet used anywhere.