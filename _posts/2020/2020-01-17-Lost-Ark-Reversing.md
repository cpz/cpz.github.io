---
layout: post
title:	"Lost Ark Unexpected Data Mining"
date:	2020-01-17 09:20:00
categories:
    - blog
tags:
    - reversing
    - windows
    - screenshot
    - lostark
---

![Screenshot]({{ site.url }}images/screenshots/screenshot_0.png)

Today, I'll talk little bit about how reversing game leaded me to new upcoming recently announced classes (Notice: They only announced, so there no actually information about them).
Right now I'm working at Bot for Lost Ark. Main reason is get advantage in PvP and don't waste time on routine job like getting resources for healing potions.
Bot should go through level and collect all possible to get resources for us (We define it via menu which resources are needed for us).
But I've run in to problem. I can't get names of entities on the level. They have names defined by engine, like: "EFSkeletalMeshActor_Number" but it will be Mining resource or Lumbering or Gathering.
We can't handle gathering resources via bot because can't disguise resources.

Its how they looks in the game ():
![Screenshot]({{ site.url }}screenshots/Screenshot_4.jpg)

So, I've started to reverse and trying to find their category or something what can help me.
And found interesting things for upcoming update.
There a data in the game which is responsible for disguise players [class division \[1\]][1] and their sub-class.
(This enum is deprecated for some reason, [real enum \[0\]][0] is a lot of bigger and contains pre-defined types for future new sub-classes like: PLAYER_CLASS_SPECIALIST600     = 600)

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

If you played at least once in game then you know that this class contains more than we got in the game.
So, what actually it shows to us?
That there un-released class Specialist which contains these sub-classes:

~~~
PLAYER_CLASS_SPECIALIST        = 601,
PLAYER_CLASS_ASTROLOGER        = 602,
PLAYER_CLASS_MUSICIAN          = 603,
PLAYER_CLASS_ALCHEMIST         = 604,
~~~

Also, I'm reversing CIS Region version game, so it doesn't have Holy-Knight new warrior sub-class or new class assassins but engine contains data for them because they are sure going to be added in future.

So, what else we can see?

Hunter going to get new sub-class named as Scouter.
~~~
PLAYER_CLASS_HUNTER            = 501,
PLAYER_CLASS_HAWK_EYE          = 502,
PLAYER_CLASS_DEVIL_HUNTER      = 503,
PLAYER_CLASS_BLASTER           = 504,
PLAYER_CLASS_SCOUTER           = 505,
~~~

Assassin class has "Delain" name in engine and does contain un-released sub-class Reaper.
~~~
PLAYER_CLASS_DELAIN            = 401,
PLAYER_CLASS_BLADE             = 402,
PLAYER_CLASS_DEMONIC           = 403,
PLAYER_CLASS_REAPER            = 404,
~~~

Basically, I don't know how to end this post but there we go. Its how I got un-released data from game engine.

## Credits

~~~
[0]: https://github.com/realrespecter (Korea SDK Dump)
[1]: KN4CK3R (UnrealEngine SDK Dumper)
[2]: TheFeckless (Tutorial on reversing Unreal Engine)
~~~

[0]: {{ site.url }}images/screenshots/Screenshot_2.jpg
[1]: {{ site.url }}images/screenshots/Screenshot_1.jpg
