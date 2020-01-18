---
layout: post
title:	"Lost Ark Unexpected Data Mining"
date:	2020-01-17 09:20:00
categories:
    - blog
tags:
    - reversing
    - windows
    - lostark
---

![Screenshot]({{ site.url }}images/screenshots/screenshot_0.png)

Yay its my first post and this is my first time reversing or hacking in Unreal Engine.

Today, I’ll talk little bit about how game reversing lead me to [new upcoming recently announced classes \[0\]][0]. (Notice: They only just announced them, so there is no actual information about them).

Right now I’m working on a bot for Lost Ark. The main reason is to get an advantage in PvP and not waste any time on routine jobs like getting resources for healing potions.

The bot should go through the level and collect all the possible obtainable resources. (We define it via a menu, which resources are needed)

But I’ve run into a problem. I can’t get the names of the entities on the level.

They have names defined by the engine, like: “EFSkeletalMeshActor” + ID of the entity of the class on the current level. So, the final result is; for example are “EFSkeletalMeshActor_1” but it could be Mining/Lumbering/Herbalism resources.

We can’t handle gathering resources with the bot because it can’t distinguish the real names of resources.


This is how they look in game:

![Screenshot]({{ site.url }}images/screenshots/Screenshot_4.jpg)

So, I’ve started to reverse it and trying to find their category or something that can help me.

I've found some interesting things for the upcoming update.

There is data in the game which is responsible for players [class division \[1\]][1] and their sub-classes.

(This enum is deprecated for some reason, [real enum \[2\]][2] is much bigger and contains pre-defined types for future new sub-classes like: PLAYER_CLASS_SPECIALIST600 = 600)

~~~
// Enum EFGame.EFConst.PlayerClassDeprecated
enum class EPlayerClassDeprecated : uint8_t
{
	PLAYER_CLASS_DEPRECATED_NA     = 0,
	PLAYER_CLASS_DEPRECATED_WARRIOR = 1,
	PLAYER_CLASS_DEPRECATED_MAGICIAN = 2,
	PLAYER_CLASS_DEPRECATED_FIGHTER = 3,
	PLAYER_CLASS_DEPRECATED_DELAIN = 4,
	PLAYER_CLASS_DEPRECATED_HUNTER = 5,
	PLAYER_CLASS_DEPRECATED_SPECIALIST = 6,
	PLAYER_CLASS_DEPRECATED_BERSERKER = 7,
	PLAYER_CLASS_DEPRECATED_DESTROYER = 8,
	PLAYER_CLASS_DEPRECATED_WARLORD = 9,
	PLAYER_CLASS_DEPRECATED_ARCANA = 10,
	PLAYER_CLASS_DEPRECATED_SUMMONER = 11,
	PLAYER_CLASS_DEPRECATED_BARD   = 12,
	PLAYER_CLASS_DEPRECATED_BATTLE_MASTER = 13,
	PLAYER_CLASS_DEPRECATED_INFIGHTER = 14,
	PLAYER_CLASS_DEPRECATED_FORCE_MASTER = 15,
	PLAYER_CLASS_DEPRECATED_BLADE  = 16,
	PLAYER_CLASS_DEPRECATED_DEMONIC = 17,
	PLAYER_CLASS_DEPRECATED_REAPER = 18,
	PLAYER_CLASS_DEPRECATED_HAWK_EYE = 19,
	PLAYER_CLASS_DEPRECATED_DEVIL_HUNTER = 20,
	PLAYER_CLASS_DEPRECATED_BLASTER = 21,
	PLAYER_CLASS_DEPRECATED_ASTROLOGER = 22,
	PLAYER_CLASS_DEPRECATED_MUSICIAN = 23,
	PLAYER_CLASS_DEPRECATED_ALCHEMIST = 24,
	PLAYER_CLASS_DEPRECATED_SCOUTER = 25,
	PLAYER_CLASS_DEPRECATED_LANCE_MASTER = 26,
	PLAYER_CLASS_DEPRECATED_HOLYKNIGHT = 27,
	PLAYER_CLASS_DEPRECATED_MAX    = 28
};
~~~

If you've played the game at least once then you know that this enum contains more than we get in-game.

So, what does it actually show us?

That there are un-released classes like Specialist which contain these sub-classes:

~~~
PLAYER_CLASS_SPECIALIST        = 601,
PLAYER_CLASS_ASTROLOGER        = 602,
PLAYER_CLASS_MUSICIAN          = 603,
PLAYER_CLASS_ALCHEMIST         = 604,
~~~

Also, I’m reversing the CIS region's version of the game (Its approx. 1 year behind the Korean region's game version), so it doesn’t have new warrior sub-class Holy-Knight or new class called Assassin but the engine contains data for them because they are surely going to be added in the future.

So, what else we can see?

The Hunter class is going to get a new sub-class named 'Scouter'.

~~~
PLAYER_CLASS_HUNTER            = 501,
PLAYER_CLASS_HAWK_EYE          = 502,
PLAYER_CLASS_DEVIL_HUNTER      = 503,
PLAYER_CLASS_BLASTER           = 504,
PLAYER_CLASS_SCOUTER           = 505,
~~~

The Assassin class is named “Delain” in the engine and also contains an un-released sub-class named 'Reaper'.

~~~
PLAYER_CLASS_DELAIN            = 401,
PLAYER_CLASS_BLADE             = 402,
PLAYER_CLASS_DEMONIC           = 403,
PLAYER_CLASS_REAPER            = 404,
~~~

[Original post \[3\]][3] was by me on UC but I decided to post it here too, also describing how I found these things.

I know its nothing “big” but it was interesting to find something like this.

If you want to learn more about the game engine for finding anything else interesting then here is a [link\[4\] [4] to a dump of the Game Engine.

Well, I don’t know how to end this post but here is my method for obtaining the un-released data from the game engine.

Thanks for reading!

## Credits

~~~
# https://github.com/realrespecter (Korea SDK Dump)
# KN4CK3R (UnrealEngine SDK Dumper)
# TheFeckless (Tutorial on reversing Unreal Engine)
~~~

[4]: https://github.com/cpz/Lost-Ark-SDK
[3]: https://www.unknowncheats.me/forum/other-mmorpg-and-strategy/308687-lost-ark-sdk-information.html
[2]: {{ site.url }}images/screenshots/Screenshot_2.jpg
[1]: {{ site.url }}images/screenshots/Screenshot_1.jpg
[0]: https://lostarkdatabase.com/kr-lost-ark-season-2-updates-expedition-territory-island-housing-system-genderlock-removal-new-classes/
