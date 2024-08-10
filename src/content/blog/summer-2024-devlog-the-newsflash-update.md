---
title: "Summer 2024 Devlog: The Newsflash Update"
description: "Yes, we missed the June devlog. But why? What progress did we make during June and July? And....what's this about a new game engine? Read on to find out."
pubDate: "Aug 9 2024"
heroImage: https://cdn.acidiclight.dev/original/1X/1b93f98b5d30d6bd6be7f8a4f8c42a1f25e93c8b.jpeg
category: Devlog
---

It's been a while! Half an entire season passed since the last devlog. What happened? What's been cooking over June and July? Is everything okay? That's what this devlog's going to tell you. So, sit back - grab your favourite beverage, and relax. Because this is going to be a huge one.

## Git Madness
Usually, we like to put a changelog here that came straight from the Git log from the last month. This time, that won't be possible. Socially Distant's entire Git workflow changed, and even if it hadn't, there's just so many changes that you'd be scrolling through them forever.

**The Move to Self-Hosted GitLab:** Traditionally, development of the game was done on GitHub. We recently open-sourced the game's base, while maintaining a private downstream fork containing the game's Career mode. However - in the interest of keeping things more flexible, and being able to adapt to the game's needs, we've moved development over to a self-hosted GitLab EE instance. This allows us to use Git our way, and do as many cursed things with it as we want without having to work around GitHub's UI or the limitations of a free plan.

**Changes to the way the game is developed:** We have decided to change the way new versions of the game are worked on. Instead of creating a single version branch of the game on the downstream fork, we now split things up into feature branches when it benefits everyone. The first features in the game to be worked on this way were the mission system and checkpoint system. We still maintain the private version branch, however we create a public merge request to upstream that allows the community to track upcoming changes (even if you can't see the actual code until it's merged). Again, this allows us to be far more flexible with how the game is made - giving room for a team to eventually grow.

**GitLab Pipelines:** As the game is being worked on, we now allow Linux and Windows builds of the game to be compiled automatically and downloaded. This doesn't mean you can download the Career Mode, but you can download builds of the base game for testing. This has even helped identify a [critical bug in the game's save system](https://gitlab.acidiclight.dev/sociallydistant/sociallydistant/-/issues/6) found by Ashe in the community.

## Becoming Independent from Unity
In the span of two weeks, Socially Distant has been completely reconstructed from the ground up to use a bespoke game engine designed specifically for the game's needs. As with any major project like this, it's not done - but the game is well beyond being at parity with the Unity version of the game.

**Why abandon Unity?** Because it's annoying to work with, constantly crashes, has APIs that remain undocumented, doesn't support features of modern .NET, doesn'y support NuGet, and is overall annoying to set up and use. The only reason we used it was some form of developer stockholm syndrome that has been cured.

## Newsflash!
Ah. Yes. The headline feature of June 2024, the News system. It sounds really dull and boring at first, and maybe it is - but it's an important feature in the game.

A new in-game news station, New Cipher Today, has been created. An in-game website now posts news articles describing events of the game as you play it. These articles give further insight into the game's world and the various going-ons of the game's story beyond what you yourself witness. Not only that, but news articles will now appear on the in-game social media site and in the Web Browser's homepage. You can visit New Cipher Today in-game at `web://newciphertoday.com/`. [It also exists in real life](https://newciphertoday.com/).

![Screenshot of a test News Article in the Web Browser](https://cdn.acidiclight.dev/original/1X/1b93f98b5d30d6bd6be7f8a4f8c42a1f25e93c8b.jpeg)

For the developers/modders out there, news articles are written using Markdown format just like actual articles on this site. The above screenshot shows a test article containing an image, showing the capability of our in-game Markdown renderer. **Note:** It is not using a browser engine.

![Screenshot of the browser home page](https://cdn.acidiclight.dev/original/1X/1e6ec8bad8d129ab244089fbe57fba740b91e294.jpeg)

Although this interface is incomplete, the above screenshot shows what the game's home page looks like. It shows the latest news and social posts in the game's world. It's also a way to find other websites in the game.

![Screenshot of the Web Directory](https://cdn.acidiclight.dev/optimized/1X/05e2a0f9836171d4644f783283b52507881a0f0b_2_690x388.jpeg)

![Screenshot of a search results page](https://cdn.acidiclight.dev/optimized/1X/74e2d0ac69ef2e862cef3c8781ef2f17068241c5_2_690x388.jpeg)

You can also use the home page as a search engine.

## New Character Creation Screen
As part of the port away from Unity, we decided to finally re-design the Character Creator screen. The Character Creator screen shows up when you first start the game and create a new save file.

![Screenshot of the "Enable Tutorials" prompt and the new Character Creator interface](https://cdn.acidiclight.dev/original/1X/94c0ed27e91f8bed11fb1fde0cc75425ad6ff4f2.jpeg)

**Ability to enable/disable tutorials:** You can now choose whether you'd like to have in-game tutorials or not during the playthrough.

![Screenshot of the new Lifepath Select screen](https://cdn.acidiclight.dev/original/1X/51f62418b3452de5c89b1d6a5a30e05baff22c03.jpeg)

**New Lifepaths:** Although this isn't finished, a different set of Lifepaths have been added to the game. Currently, the descriptions are missing and there's a missing third Lifepath option.

![Screenshot of the User Info screen](https://cdn.acidiclight.dev/original/1X/f05a1fa3c90dae29d919e5926e9c02703d4a8e88.jpeg)

**New User Info screen:** Previously, you needed to choose a _full name,_ pronoun, username, and hostname. This has been simplified. You only have to pick a character name, computer name, and pronoun preference.

## Mission System
Throughout July, we fleshed out the game's mission system and have practically finished its core functionality.

![Screenshot of a test mission email](https://cdn.acidiclight.dev/original/1X/6bf3df0bd900702d8e700b77d770f0227754a2e3.jpeg)

**Mission emails:** You can now receive missions through email. The above screenshot shows a test mission being started from email.

![Screenshot of the Objective List](https://cdn.acidiclight.dev/optimized/1X/370b31eb97e31f7dcd85894b5e55d2ac3c84f96f_2_690x388.jpeg)

**Objectives:** When you're playing a mission, information about the mission is shown in the Info Panel on the right side of the screen. Mandatory objectives, as well as optional bonus challenges, are listed in a checklist.

![Screenshot of a single objective](https://cdn.acidiclight.dev/original/1X/d693c9c00bc7137e27da3eec932bcd3480997458.jpeg)

Completing objectives removes them from the checklist. So does failing bonus challenges.

![Screenshot of an objective with a fail condition](https://cdn.acidiclight.dev/original/1X/740b6eb5d1db602726206ef58a62a2a03741e590.jpeg)

Some objectives have fail conditions. Behind the scenes, they're implemented using hidden objectives not shown on the checklist.

![Screenshot of the Mission Fail screen](https://cdn.acidiclight.dev/original/1X/d5b107bbe5bf5e234b4d5c9dafdd553d132fd340.jpeg)

When you fail a mission, the game gives you a chance to retry from the last checkpoint or abandon the mission. It also tells you why you failed.

![Screenshot of the Mission Complete screen](https://cdn.acidiclight.dev/original/1X/b0d7ab4c73b391d50bc29921346e56e786ba02f6.jpeg)

The Mission Complete screen looks similar. In the future, it will show stats about the completed mission as well as any rewards you've earned.

## Checkpoint System
As part of the mission system, the game now has a checkpoint system. The checkpoint system works by saving a copy of the entire game's state into a compressed file on disk, and keeping track of these checkpoint files inside the save file they belong to.

This allows the game to restore checkpoints _across game sessions,_ allowing the game to bring your playthrough back to a known-good state if something goes wrong. (Save corruption, a softlocked script, etc.)

This also means that you can quit the game during a mission and continue to play the mission where you left off. Instead of abandoning the mission, the game will instead restore the last mission checkpoint. This means there will be _minor_ progress loss if you quit during a mission, but this lets the game restore things to a known safe place to resume from.

Note: The checkpoint system doesn't (and probably never will) keep track of the state of the game's UI. That is, unless someone is willing to upstream that as a feature - it's A LOT more complex to program. :)

## Don't You Worry, We'll All Float On!
Floating windows have been fleshed out.

![Screenshot of two file managers and an application launcher window](https://cdn.acidiclight.dev/original/1X/db5634c979189a2b8283af9ff25804b69ea5364b.jpeg)

You can now have multiple windows of the same program open in their own floating windows. In the future, you'll be able to have them in tabs as well.

Floating windows now also show as items in the middle of the Dock. In the future, this will allow you to minimize and restore the windows. This hasn't yet been implemented.

## An Eventful Summer
A new, and important, under-the-hood change was made in the game. Socially Distant now has a global event bus. It allows any part of the game to communicate with any other part of the game by sending events over the bus.

For example, a mission might want you to hack into someone's laptop and download a file from it. When you hack into the device, the hacking system sends a "successful hack" event on the event bus. Then, when you download the file, a "read file" event gets sent from the laptop and a "write file" event gets sent from the device *you're* using. The mission system can then see all three of these events and work out that you've completed the objective.

Mods can even define their own events that can be sent and received over the bus, allowing mods to communicate with each other and the game. The event bus is also thread-safe, meaning even the Internet simulation can generate events. In the future, that'll let us implement intrusion detection into the game's hacking system!

## Under-the-Hood changes
We've already mentioned that the game has been ported away from Unity. But what did we port to? What else has changed?

Socially Distant uses a bespoke game engine. It sounds scary (engines take time to write), but we didn't do everything from scratch (as you shouldn't when your goal is to make a game). Socially Distant uses the **MonoGame Framework** as its core runtime, meaning that MonoGame is what handles lower-level rendering and audio. It also handles the game window, input devices, and part of the game's asset management. We don't, however, use most of MonoGame's higher-level APIs.

For text rendering, we use a custom abstraction over FontStashSharp that deals with font weights and italic variants.

The user interface system is completely custom, having being purpose-built for the game. It can be described as taking the Unity UI system and throwing 99% of it away, replacing it with things that are actually done right. The API is loosely inspired by the Unreal Engine's UI system, with a hint of Optimized ScrollView Adapter.

The game's engine also introduces our own content manager in place of MonoGame's. It allows us to load assets from other sources than just files on the disk. It also allows us to load all content of a given type (e.g, "find and load all missions in the game").

Although we love MonoGame and it works really well for the most part, there are still some remaining platform problems that need to be fixed. In the future, we plan to fork off of MonoGame itself and introduce the game engine's features directly into it - this allows us to tailor MonoGame to fit Socially Distant's needs.

## Under-the-hood changes (but for the game itself)
Here are some neat things Socially Distant supports now:

- The `DocumentAdapter`: This is the code that renders news articles. It also now handles emails, chat messages, social posts, and anything else where some kind of document needs to be rendered. It also handles things like mission attachments, meaning missions can be given to you in other places than just emails.
- Markdown renderer: The game can now fully render Markdown.
- Hook scripts: Modders can now use `sdsh` scripts to do things in response to certain game events, like loading into a new playthrough.
- Custom Tools: Tools are the main applications in the game (like Terminal and Web Browser) that you switch between with the Dock. Modders can now create their own.
- Widget Effects: Widgets in the UI can now have custom shaders/effects applied to them. This is how certain features like the background blurred panels are achieved.
- Theme System: All widgets in Socially Distant render using a visual style defined by the game. This forces the UI to stay visually consistent.
- Custom Mission Tasks: Mission Tasks are things the player has to do in order to complete/fail an objective. Modders can create their own.
- Custom Tray Actions: Tray Actions are the clickable icons in the right side of the Status Bar (e.g settings). Modders can define their own.
- Better Overlays: Overlays (like the "Mission Complete" screen and the System Settings screen) can now stack on top of each other. This allows for confirmation dialogs to appear inside other overlay screens, which (annoyingly) wasn't the case in Unity.
- Native rounded rectangles: Thanks to a lot of help from the community, we now have a native rounded rectangle renderer in the game.

## Remaining issues with the port
There are still some notable and some minor issues with the port that need to be worked out. If you'd like to contribute to the game, these are some good issues to start with.

1. There is no post-processing system yet. This means that effects like bloom and glitch bands are impossible for now.
2. Occasional word-wrapping issues: Some UI elements don't word-wrap properly even though most do. These could be simple layout bugs that haven't been worked out yet, and are likely not bugs with the text widget itself.
3. No UI animations: We don't have a tweening library or a way to do animations inside the UI system at all. Simple fades are possible if done manually using `RenderOpacity`, but that's it. This means it's not at all trivial to animate a toggle switch being flicked back and forth.
4. Missing CPU and OS info in `neofetch` (Windows/macOS): These platforms have their own way of exposing this info to us, and this was previously handled by Unity.
5. Improper window titles: You may have noticed some programs in the game, like Terminal, show names like "TerminalProgramController" in their tab titles. This is just because their proper names haven't been set.
6. Terminal background blur: Open Terminal and switch to another tool. Then switch back to Terminal. The background becomes solid. This is a bug with how `ToolManager` handles tool switching.
7. Word-breaking in `TextWidget`: If a word is too long to fit on a single line, TextWidget will not break it. This will lead to a visual bug.

## And that's all for now!
A lot of progress, huh? That's the idea.

As always, this devlog was made available to acidic light community Patreon supporters. We really wanna thank everyone who does that! Server upkeep costs for the community are being fully covered by Patreon contributions, and that makes shit a whole lot less terrifying. There's some super-secret Patreon-exclusive content coming down the pipes. So, if you find that exciting and wanna support Socially Distant's development, hey - ya get these devlogs early too! :)