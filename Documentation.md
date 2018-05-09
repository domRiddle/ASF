# Documentation

This is an attempt to write a very detailed technical explanation how ASF works in detail - it's **definitely** not a text for typical user that just wants to make ASF "work", you have **[Setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** in this case.

**This page is still work-in-progress**

---

## Preface

We'll mainly focus on interesting aspects of ASF and offer explanation for more complex functions or features. This documentation won't include an explanation of every single line of code, but it will include explanation of how ASF achieves various Steam activities that might not be obvious at first glance. It's especially important for us to include information about various built-in functions, features, hacks and workarounds - as those are typically invisible to end-user and you might not know about their existance without carefully examining the source code - that's also their purpose, they just work.

ASF changes **rapidly**, therefore keeping this documentation up-to-date with code changes might be very hard and time-consuming in long-run. We will try to update it as often as possible, but we cannot guarantee that it'll always stay up-to-date in regards to latest ASF release. Current version of documentation is based on ASF **V2.2.3.7**.

---

## Steam services

In documentation there will be often references to various Steam services, and you should know the difference between then.

- Steam Network, is that part of Steam that is restricted to Steam clients only, inacessible without its implementation.
- Steam API, is the most common part of Steam that is available for majority of third-party services.
- Steam Community, is the part designed for Steam end-users only, providing you with web-based functionality.
- Steam Store, is very similar to Steam Community, but has functions related to licenses mainly.

---

## Integration

The primary difference when comparing ASF to other similar tools, such as Idle Master, is the fact that those two programs work in totally different way. IM is accessing Steam Community badge page, reading from it which games need to be farmed, then emulating those games being launched, which triggers official Steam Client into sending appropriate request with "hey I'm playing these games" to Steam Network. Probably you can already notice various limitations of that method:
- We can't access Steam Network at all
- We can't operate without Steam client at all
- We can't make Steam client do what we want, we must "trick" it into thinking that something is happening

For example, redeeming keys with IM is impossible by definition, since you can't really trick Steam client into redeeming the key in any possible way, so you must either do that manually, or with third-party tool that will **still** do it manually, but semi-automatically just for you.

ASF is based on SteamKit2 - project that aims to create open-source implementation of Steam client that could be used in C#-based applications. Without SK2 ASF would not exist, as it's the entire "heart" that allows ASF to communicate with Steam Network and fix all IM flaws mentioned above. This basically means that we're not on official Steam client's mercy, we can communicate with Steam Network directly, and we don't even need official Steam client to be installed or running, which means that ASF is a standalone app that can work fully by itself, with no extra dependencies (apart from all dependencies required to run the binary properly, that is).

---

## Authentication

ASF maintains two sessions - Steam Network session, and Steam Web session (`ISteamUserAuth`). Steam Web session is **binded** to Steam Network session, so it's automatically invalidated the moment Steam Network session is gone. As you can guess, Steam Web session is required for communicating with Steam Community or Steam Store.

ASF uses login keys mechanism - the same one that is being used in official Steam Client. This mechanism is very simple and allows ASF to skip password/2FA/SteamGuard authentication, if previous session was already authenticated successfully. When we don't have any old login key, for example on first ASF run, we need to log in through standard login + password combination, and extra 2FA/SteamGuard code (if needed). Once we log in successfully, ASF automatically receives from Steam Network special login key, which can be used for next login **instead** of password. Every login key can be used only once, which means that ASF automatically gets new one from Steam Network once we log in again using our current one. Login key is saved in `BotName.db` and encrypted using your chosen `PasswordFormat`. Login keys can't be used outside of Steam Network.

---

## Events

ASF is heavily based on Steam Network events, which further enhance it with nearly real-time reaction to what is currently happening. For example, thanks to those ASF is able to check status of game being idled as soon as there is item notification, or check active trades as soon as there is trade notification. Those two are the most obvious ones that you can also notice in your Steam client, but there are many of those that are not visible right away, such as event related to updated list of licenses, which allows ASF to start idling as soon as your account receives any new game, be it through a gift, cd-key or any other meaning. Thanks to that, ASF is able to start idling **immediately** after you redeem new cd-key, without a need of restart or a command.

---

## Cards farming

Cards farming process might look simple, but it's very complex code-wise. After logging in, we must check all our badge pages in Steam Community to get list of games that can be idled. After that setup, we must exclude known false-positive apps, such as Steam Winter/Summer sale - those are hardcoded into the app, but user can also add them in `Blacklist`.

At this step we have a full list ready, so we sort it according to `FarmingOrder` (if needed). Then we start idling, depending on chosen cards farming algorithm.

When we get to the point of farming a single game, ASF will also check if farming it is even possible, by asking Steam Network about release state of that game. If game is not released yet, it's temporarily purged from the list, as we're not able to idle it yet. Sadly it's not possible to ask this for multiple games, as various Steam glitches are happening, so we can't really ask that earlier, for example before deciding to farm hours of that game.

Idling game consists of sending `GamesPlayed` request to Steam network in which we include `appID`s of games we're playing. ASF then periodically (and on each item dropped event) checks if given game is idled already - if yes, we can purge it from the list and move on, until all games are fully farmed.

---

## TODO