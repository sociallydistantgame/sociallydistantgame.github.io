---
title: "January 2024 Devlog: New UI and first steps toward a proper story"
description: "After several months of massive amounts of work, it’s time for a proper Socially Distant devlog. With a new UI, massive improvements to the terminal and shell, modding support, game scripting support and more, the game is finally getting closer to a proper alpha and private testing. Let’s look at what’s been worked on, what it all means for Socially Distant, and what the plans are going forward."
category: "Devlog"
pubDate: "Feb 7 2024"
heroImage: '/assets/blog/img_vQ2NgAbK7uL8m.png'
---

This devlog was originally written on **January 31st, 2024** as early-access content on Patreon.

## Complete user interface refresh
The most exciting visual change to the game is the new UI. It’s not finished, and needs a lot of polish, but it’s likely to be very close to the final version of the game’s interface layout. Many elements of the new U, such as the new font, are also used on the Socially Distant website.

![Screenshot of the new UI](/assets/blog/img_vQ2NgAbK7uL8m.png)

The new UI brings the game’s more important elements, like the terminal, into prominent focus. It keeps many layout elements of the old UI, like the dock, while fleshing them out and adding new quality-of-life features to them.

![Screenshot of the application launcher window](/assets/blog/img_QGX3qaRjBtcEL.png)

A work-in-progress application launcher window was added to the game’s desktop environment. The launcher lets you open hacking tools and other useful utilities outside of the main applications visible on the dock.

![Desktop with no windows open, during the day](/assets/blog/img_17gu40gQrkyaq.png)

You can also hide whatever primary app you’re focusing on, like the terminal, to reveal the desktop background.

![A message dialog asking you if you want to quit Socially Distant](/assets/blog/img_Fgvs1V0hCV9MG.png)

Dialogs, like the “Quit Socially Distant” one above, have been redesigned as well.

![Screenshot of a floating File Manager and Text Editor window](/assets/blog/img_yyZfrLcKLQJvV.png)

Two new utilities were added to the game: the file manager and text editor. The file manager obviously is used for browsing and managing files on your computer or remote file servers. The text editor can be used to read text files you find on a hacked computer, or to take quick notes during gameplay.

![Screenshot of the Socially Distant End Session dialog](/assets/blog/img_CvwQrZloXnq1U.png)

A new “End session” button was added to the status bar, allowing you to log out of your computer and return to the game’s main menu. The “Quit Socially Distant” button now quits directly to desktop.

## Future UI concepts
Along with changes in the current game build, several concepts were made for future changes to the user interface.

![A concept showing what Socially Distant would look like while you're being hacked](/assets/blog/img_WJH7he2JML69m.png)

This concept shows what the desktop might look like when an active malware threat is found on your computer.

![Concept for the future login screen](/assets/blog/img_XRwopHFugiPwc.png)

This is a concept for the new main menu, which is meant to look likee a login screen.

![Concept for the character creator screen](/assets/blog/img_tBQj2JM5vsAMX.png)

This is a concept for the “Create New User” screen. It shows what input fields and dropdowns look like in the new interface.

Some more concepts were made for different layout and widget styles.

<div class="grid grid-cols-1 md:grid-cols-2">

![Concepts for various basic form elements](/assets/blog/img_I8kqoVmClaNyi.png)

![Concepts for different kinds of layouts](/assets/blog/img_zDRQMkTYZjFbM.png)

</div>

## Terminal and Shell changes
Several changes have been made to the terminal to make it more user-friendly and easier to work with.

![Screenshot of the terminal with text selection](/assets/blog/img_sMItkPeejFuUy.png)

The line editor has been re-written. The line editor is what lets you actually type commands into the shell, and to answer questions a command asks you. It now supports selection, proper tab completion, word wrapping, erasing, copy/paste, and everything else you’d need. It also now supports password entry.

![Screenshot showing the two different player shell prompts](/assets/blog/img_H1L9yHxR54lyy.png)

Proper support for variable and command substitution has been added to the shell. This is very useful when writing your own shell scripts to use during hacking.

![Screenshot showing a shell 'if' statement](/assets/blog/img_kCbM1RsSFqmUQ.png)

You can also now do if/else statements in the shell. This allows you to write shell scripts that, for example, behave differently when you run them as admin.

![Image showing a shell function being run](/assets/blog/img_ErVtj8rJ0N4D4.png)

You can also now define functions. Functions are groups of commands that can be ran as if they are a command themselves. These are also extremely useful when writing shell scripts.

![Image showing a forkbomb](/assets/blog/img_QBzAUv8p46Y8m.png)

And yes, this means support has been added for the classic forkbomb. But, be careful! It doesn’t behave like a forkbomb in real life, but **it will crash the game**.

## Modding support
Support is being added for mods in the game. Mods can add custom story content to the game, and through the use of the Socially Distant Runtime, will soon be able to modify the game’s interface and extend the virtual OS.

The Socially Distant Runtime repository is managed by a bot, and updates as the game is worked on. Note that it does not accept contributions, since it must remain binary-compatible with the game for mods to work. Information on how to mod the game will be written closer to release.

Game Scripts
Another extremely important feature being worked on in the game is support for game scripts. This feature is under-the-hood, but it’s important for making it easier to design missions and other narrative elements in the game.

Game scripts are used by the game itself, and can be used by mods. They use the same language as the in-game shell, but have access to commands that allow you to control the game itself instead of controlling your in-game computer.

As with the modding support, documentation on how to write game scripts will be provided closer to release. Suffice to say, they’re really powerful and really important.

## So that’s it.
We’ve seen major changes to the game’s interface, lots of quality-of-life additions to the game, and an entire scripting system for writing the game’s story. But what’s next?

In the next devlog, you can likely expect:

 - A look at the game’s web browser, and the new social media website.
 - A demo of the chat conversation system using game scripting.
 - Support for multiple tabs in the terminal, web browser, and other apps.

Finally, in late 2023, we planned on releasing a theme editor for the game. With the new changes to the UI, this plan needed to be cancelled. Instead, we plan on releasing a demo version of the game itself that lets you play through the first mission of all three Lifepaths. We hope this gets you excited, and that it lets everyone get a feel for what the game is like to play. We’re still a long way before that’s ready, but stay tuned!