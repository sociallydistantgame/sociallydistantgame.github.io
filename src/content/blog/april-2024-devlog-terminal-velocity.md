---
title: "April 2024 Devlog: Terminal Velocity"
description: "While March saw lots of work related to the game's networking simulation code, this month is all about making the game's command-line and interface much better... with a hint of gameplay work!"
pubDate: "April 30 2024"
heroImage: https://cdn.acidiclight.dev/original/1X/6d88470a31cef897a9ec36f04ad5e6b2707dd9e0.jpeg
category: Devlog
---

In March, we worked on the game's networking simulation. That's the part of Socially Distant that makes the hacking mechanic possible.  After optimizing it for performance, it was time to move onto other more player-facing parts of the game. That's what this month's all about.

This month is all about making the game's terminal far more performant, adding lots of quality-of-life improvements to the game's UI, defining an art style, and even working on some core gameplay code! So, with that all said, what's actually been worked on this month? Well, you're reading this - so let's find out!

## Let's Git To It!

....get it? Or...should I ask, `git` it? Because that's what this section's for. Just like the last devlog, it's time for the Git changelog for this month.

Throughout the month of April, a total of 50 commits were made by Ritchie to the `work/24.04` branch. Of the 50 that were made, 48 were logged as notable changes to the game. Here's what they were:

- Update version string
- Slightly optimize terminal resize detection
- Add support for thread-safe Terminal rendering
- Remove old/stale code from terminal emulator
- Move terminal emulation code to a separate thread
- Add support for cancelling Terminal programs with CTRL+C
- Add support for window hints
- Add support for terminal background blur
- Add settings listeners for reacting to settings changes in the UI
- Add accessibility toggle for disabling terminal background blur
- Use background blur for window overlays
- Render all UI to the main camera
- Fix layer ordering of UI top-levels
- Add support for bloom
- Add support for turning off bloom in Settings
- Add LeanTween
- Fix window dragging
- Fix terminal selection dragging
- Remove the lost souls of the damned
- Use file chooser driver's path combine implementation instead of .NET's
- Fix tab windows not closing when all of their tabs are closed
- Fix issue where closing the only opened Terminal tab prevents new terminals from being opened.
- Add support for opening multiple Terminal tabs
- Fix YesNoCancel dialog boxes reporting a result of No when clicking Cancel
- Add support for changing terminal titles
- Limit titles of window tabs to one line
- Use WindowTitle console API to show status of running nmap scans
- Add support for changing player hostname via /etc/hostname
- Add support for reading Unity log in-game via dmesg command
- Add email viewer
- Add support for game changelog emails
- Fix Delete Account confirmation appearing behind the User Info overlay and being uninteractible
- Add support for importing mission scripts
- Allow mission scripts to be loaded by the System Module
- Add support for starting missions via emails
- Add support for script function libraries
- Add support for text links in Acidic UI
- Add support for performing shell URI navigations via mission script
- Add support for opening web URIs in Web Browser
- Fix browser links not actually opening when opening browser for the first time
- Add support for starting/stopping missions with mission URIs
- Add support for visually styling links
- Fix handling of newlines in ShellScriptImporter
- Fix window tab list not updating when a tab gets closed
- Add support for visually closing window tabs
- Fix occasional corruption when outputting text to Terminal
- Fix selection word snapping corrupting text in Terminal
- Improve handling of double/triple-click in Terminal

As you can see, that's **a lot** of work this month - so let's break it down even further.

## Multi-threaded Terminal
This section is part of why this devlog's title is Terminal Velocity.

Since this is a hacking game, you're going to spend a lot of time in a command-line. That means it is really important for the terminal in the game to be as fast as reasonably possible. The terminal shouldn't feel sluggish to use, it shouldn't lag, and it shouldn't ever drop the framerate when printing lots of output at once.

We made lots of strides in improving the terminal's render performance months ago. This was done by optimizing how the terminal generates Unity TextMeshPro markup, and changing how background colors are rendered. However we did notice a problem last month when testing `nmap` scans of huge IP ranges. The sheer amount of text output from `nmap` would be too much for the game to process in one frame, leading to massive (almost to 0 FPS) frame drops. Ouch!

This month, we worked on multi-threading the terminal. The terminal now runs using two CPU threads - part of it runs on the main game thread and accepts keyboard/mouse input from Unity as well as presents the finished TextMeshPro markup to the screen. The other part of the terminal that's actually responsible for processing input/output from the game's shell, is now run on a separate thread from Unity.

Ultimately, what this means is that - even if the terminal has lots of text to process, it can take its time to process it without locking up the rest of the game. A single terminal just may not be able to respond to inputs while processing text, but you will likely never actually run into that.

It did, however, force us to make shell I/O in the game thread-safe. For the mod developers out there, this means you can now safely print text to the Terminal from any thread.

## Adding LeanTween
[LeanTween](https://assetstore.unity.com/packages/tools/animation/leantween-3595) is a free Unity Asset Store package that makes it far easier to add animations and transitions to user interface elements. 

While this mostly remains unused in the game, and thus isn't easy to show, it is now used to fade in and out of dialog overlays. You'll notice it when going into System Settings, deleting a save file, or when a system error occurs. But when it comes time to further polish the game's UI, LeanTween'll be a huge part of how we do that. So it's a worthy addition to the game!

## Shell Experience Improvements
Lots of major improvements were made to the game's desktop interface and general UI. Let's take a look!

Many of the changes can be seen in this one single screenshot, save for the missing textures.

![Screenshot of Socially Distant's desktop in 24.04](https://cdn.acidiclight.dev/original/1X/6d88470a31cef897a9ec36f04ad5e6b2707dd9e0.jpeg)

First big feature is the ability to have multiple Terminals open at once. Only one can be visible at a time, but you can now have multiple tabs. This is also the case for the in-game web browser, but not for the mail viewer and chat. However, some other tools like the text editor will eventually support multiple tabs as well.

The various white boxes in the tab panel above the Terminal are the missing textures we mentioned. But - the one inside the tab button is the tab's close button, and the one next to the second tab is the New Tab button. So it's just like a real-life web browser.

Along with this, each Terminal can now have its own title. By using the `title <new_title_here>` command, you can rename the terminal you're currently in. This can be helpful for organizing which terminal you'll use for different things.

![A screenshot of the terminal's new title command being used to rename a tab to Recon Shell](https://cdn.acidiclight.dev/original/1X/7e103436851d86902fb0843a7c9f1719c7156d30.jpeg)

On the topic of renaming things, you can now rename your in-game computer by editing `/etc/hostname`.

```bash
┌─[ritchie@ritchie-pc - ~] - [Tuesday 6:43 PM]
└─ $ echo "exterminator" > /etc/hostname
Script execution took 00:00:00.0046905

┌─[ritchie@exterminator - ~] - [Tuesday 6:55 PM]
└─ $ 
```

As you can see, this change is immediate unlike how it would be on a real Linux system. But it is the exact same command, so you might've just learned something!

## Art Style and Graphical Effects
Let's go back to that earlier image of the desktop. Did you notice anything else about it?

![Screenshot of Socially Distant's desktop in 24.04](https://cdn.acidiclight.dev/original/1X/6d88470a31cef897a9ec36f04ad5e6b2707dd9e0.jpeg)

If you haven't, then we'll tell you!

### Background Blur
Blur Panels have been added to the game's UI. These are simple UI panels that, rather than having a solid background color like most in-game windows do, they instead have a translucent background and blur the contents shown behind them. In the above image, it's used in the terminal to show the desktop background behind it.

You can also see blur panels used in overlays, such as System Settings.

![Screenshot of System Settings with a Blur Panel for its background overlay](https://cdn.acidiclight.dev/original/1X/f1ed0d3090700f53be250323d75a87dd26082b6f.jpeg)

We think this adds a lot of depth to the UI and that slight bit of extra character as well.

If you find the translucent blur makes the terminal hard to read, you can turn it off in the game's Accessibility settings. This will revert it to use a solid background color like it used to.

### Bloom
We decided to add subtle bloom to the game. Bloom is an effect traditionally used to simulate the appearance of extremely bright objects glowing in a scene, as they would in real life. In this case, it's used to give brighter elements in the interface a subtle glow.

It's meant to be most visible during in-game night, but you can see it during the day as well - particularly coming from the desktop background. The effect is both meant to give the game a more Hollywood hacker look, while also simulating the same kind of glowing text effect I (Ritchie) experience when using a computer at night with my blindness.

Some players may find the effect distracting, so it can be disabled in **System Settings** -> **Graphics** -> **Desktop Effects.**

## Shell Navigation URLs
This feature is more or less developer-facing. But, as a player, you'll be using it a lot without realizing it.

If you're interested in the technical details of how they work and what can be done with them, you can read the modder's documentation for them over on the [game's online manual](https://man.sociallydistantgame.com/index.php?title=Shell_Navigation_URLs_(Modding)).

For the rest of you: this email has one in it!

![Screenshot of an email with a link in it](https://cdn.acidiclight.dev/original/1X/d394157c8dd6b320c8ddbba7d221f2c525be46e5.jpeg)

See that very obvious looking hyperlink in the email? That's a Shell Navigation URL being used in the UI. In this case, it's used to link to an in-game website. But it can also be used to link to missions, city locations, other emails, chats, networks, devices, and even to perform basic UI actions like s switching between apps or tabs.

## Email Viewer and Missions
As you saw in the above screenshot, both the Email Viewer and the game's mission system are now implemented. The Email Viewer, though still in need of a lot of polish, is mostly finished. However, most of the mission system is still missing and can't be shown. But missions can now be added to the game, started, and completed. So we're really excited to keep working on it in the next few builds!

## Parting Ways with Restitched
Ritchie, the benevolent dictator for life of Socially Distant's development, a.k.a me, a.k.a the person typing this, why am I talking in third person, made the extremely tough but necessary decision to step away from working on Restitched with Trixel Creative. 

While I wish the team luck with bringing Restitched to life, I felt it was time to move on. I'd like to use my skills, and everything I"ve learned from being with Trixel, to ship the hacking game I've dreamed of making ever since I was a little kid. I'll now be working on Socially Distant **full-time**.

I want to thank each and every one of my wonderful Patreon supporters for making that a thing I'm able to actually do. You guys are a huge motivation to work on the game and the support means way more than I can ever describe. <3

## That's the end for now!
That was a lot of stuff this month and the game is finally starting to become an actual....game. Next month, you can expect a lot of work on the game's chat and social media website. 

Also, did you know you can [apply to play-test these builds?](https://sociallydistantgame.com/playtest) Yeah - you can! If you're liking what you're seeing so far, and want to help improve the game even further by finding bugs I haven't yet, then go ahead and apply!

Lastly, my Patreon supporters always get access to these devlogs one week earlier than when they appear on the game's website. I also occasionally post screenshots and share future ideas I have for the game in the private Patreon chat on my Discord. If that's something you're interested in being a part of, and you don't mind throwing a couple bucks my way each month to help support the game, then [you know what to click!](https://patreon.com/acidiclight)