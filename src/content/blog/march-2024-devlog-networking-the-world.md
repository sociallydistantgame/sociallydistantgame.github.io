---
title: "March 2024 Devlog: Networking the World"
description: "We may have missed the February devlog this year, but that doesn't mean we haven't been hard at work. Let's take a look at what's being added in March of 2024 to Socially Distant."
pubDate: "March 30 2024"
heroImage: https://cdn.acidiclight.dev/original/1X/406655a28714328060f2f5075f5ed57806c5e3ca.png
category: Devlog
---

The start of 2024 has been a very hectic period for Socially Distant. With a major UI rework in January, a lack of a devlog in February, and a major overhaul of the acidic light community services in March, we're really happy to announce we've pulled ourselves out of the rut. Now, it's time to make this game actually happen. So let's find out what's been added in March 2024!

This devlog was originally posted as early-access content for all the lovely folks [supporting acidic light on Patreon](https://patreon.com/acidiclight)!

## Git Commit Log
Starting this month, we have adopted a better way of managing work on the game's Git repo. At the start of a month, we check out a new branch named after the current year and month. At the end of the month, this branch gets merged into `master`. Let's see what's been added in the game's `work/24.03` branch!

- Add support for scanning a network for nearby hosts
- Add support for domain name resolution (both forward and reverse)
- Add support for port scanning
- Add support for importing networks as .network assets
- Add support for creating networks based on network scripts
- Implement spawnisp world command
- Add support for storing public IP addresses in the save file
- Add support for network scripts adding domain names to the save file
- Fix hackable generation from network scripts
- Implement setplayerisp world command
- Add support for scanning subnets in nmap
- Add support for domain resolution in ping COMMAND
- Decrease packets processed per simulation update
- Improve reliability of ping packets
- Move network simulation to another thread
- Use ConcurrentQueue<T> for Wire<T> terminals
- Use ConcurrentQueue<T> for simulation node packet transfer queues
- Optimize the fuck out of network simulation
- Fix port scanning
- Fix issue where simulation thread doesn't shut down properly leading to resource leak in Unity Editor
- Optimize simulation loopback packet delivery
- Fix issue where saves do not load after unloading a save
- Make ThreadSafeFifoBuffer even more thread-safe

## Hacking and Networking
The theme of this devlog is all about networking the world. That's because most of what's been worked on this month is related to the game's hacking system. More specifically, we've been working on fleshing out the game's network simulation to support everything needed for hacking.

### Network recon tools
A skilled hacker never goes into a network blind. Before trying to break into a network, it's a good idea to know what you're up against. We've added a few new network recon and discovery tools to the game that help you find your way around the in-game Internet.

#### The `ping` command

The `ping` command lets you check if a computer is online by pinging its hostname or IP address. This is most useful for determining whether a given computer or network is actually online and reachable.

![Me pinging the default gateway of my local network](https://cdn.acidiclight.dev/original/1X/6655b22f708f2105454173a382dec726aadb000b.png)

#### The `ipconfig` command
This command is most useful for getting a basic landscape of your computer's network setup. It will give you information about all network interfaces on your device. 

![Me running the ipconfig command](https://cdn.acidiclight.dev/original/1X/4ce5fe9bcf587ecbd17a3a2da72b690dc93c375d.png)

You can use this to help identify what kind of device you're connected to as well. For example, a phone might have one network connection for Wi-Fi and another for mobile data. It may even have another connection for a wireless hotspot, which you could pivot onto other devices through.

#### The `nmap` command
This is your swiss army knife when it comes to network recon. This command lets you scan networks for devices, and scan devices for open ports. 

There are no screenshots to show the command working in-game since it is too early in development to be presentable, but here's an idea of how it'll work.

Running the command with no arguments will scan the local network for devices that aren't your own.

```bash
┌─[ritchie@ritchie-pc - ~] - [Monday 12:04 AM]
└─ $ nmap
Scanning for online hosts in local network 192.168.1.0/24...

192.168.1.1
192.168.1.3

┌─[ritchie@ritchie-pc - ~] - [Monday 5:54 AM]
└─ $ 
```

Running it with a single argument will scan that host for open ports. You can use either a hostname or IP address.

```bash
┌─[ritchie@ritchie-pc - ~] - [Monday 5:54 AM]
└─ $ nmap localhost
Starting port scan on host localhost (127.0.0.1)...

SCAN RESULTS
============

port    STATUS      SERVICE
22      open        ssh
80      open        http
443     open        https

┌─[ritchie@ritchie-pc - ~] - [Monday 7:38 AM]
└─ $ 
```

Running it with a single argument in the CIDR format (for example, `nmap 204.31.0.0/16`) lets you scan that IP address range for online hosts. This lets you find public networks on the in-game Internet. However, this is a (real-life) CPU-intensive operation and will take a while for large IP address ranges.

```bash
┌─[ritchie@ritchie-pc - ~] - [Monday 5:36 AM]
└─ $ nmap 204.0.0.0/8
Scanning subnet: 204.0.0.0/8

204.31.98.39
blackwall.net

Scan found 2 host(s).
Script execution took 00:03:28.5549704

┌─[ritchie@ritchie-pc - ~] - [Monday 9:12 AM]
└─ $ 
```

The above output has the game's script debugger enabled, which prints how long it takes for commands to execute. That command scanned 16 million IP addresses in just under 4 minutes. Whoa!

### Domain name resolution
This is an under-the-hood change to the game, but it vastly affects gameplay. Essentially, in-game computers and networks can now have domain names. So, instead of a computer needing to be referred to by an IP address like `226.142.36.47`, it can also be referred to by a domain name like `acidiclight.dev`.

This is the case for every network-related command, you can use hostnames and domain names OR regular IP addresses. 

In the future, you'll be able to add your own hostnames to things that don't already have them, by adding them in `/etc/hosts` as a root user. This would be useful if you've hacked into a network and would like to give it a friendly name so you can come back to it later. The game might even do this automatically but we're not sure yet.

Furthermore, reverse domain lookups are now possible. This allows commands like `nmap` to display friendly hostnames alongside IP addresses if it can successfully look up a hostname for a given address.

## Multi-threading
Remember how we said scanning large IP ranges with `nmap` is intensive on your CPU? That's because it is. The game's network simulation is now multi-threaded. The network simulation will use as much of your CPU as it can to get packets transferred to where they need to go as fast as possible. This doesn't mean that the game will always use your entire CPU, just that the more packets there are in transit, the more CPU will be used.

Doing large `nmap` scans like the example above results in millions of packets being generated. This can seriously overload the network simulation with work. However, we have also made some optimization to prevent this from happening in a lot of cases.

 - Sending a packet to an IP address that's known not to be in the in-game world will result in a fake ping timeout, instead of the simulation having to generate a real one.
 - Anything sent within the `127.0.0.0/8` subnet is IMMEDIATELY dispatched back to your own computer by the simulation, just like in real life.
 - In-game commands like `nmap` will throttle themselves when they've sent too many packets at once. This causes the command to take longer, but will stop the simulation from being overloaded too badly.
 - Development builds of the game will log an error to the Unity console if a simulation update takes more than 1 second. This allows us to investigate why, and hopefully optimize whatever's causing it.
 - Nodes in the simulation are updated in parallel to ensure as many packets are delivered in parallel as possible.

The simulation thread is also deactivated when the game is loading or when you're in the main menu or character creator. In the future it will also deactivate before saving the current game, or when no network activity has happened in a while.

Ultimately, multithreading the simulation prevents the simulation from interrupting the game's UI. No matter how overloaded the simulation gets, the game itself will never hang waiting for simulation updates. The worst that'll happen is your CPU being over-utilized in extreme cases, which you'll notice as the entire desktop (on your real OS) drops frames. This will settle down once the simulation finishes processing traffic.

We may add a simulation benchmark feature to the game, which would generate a worst-case network and measure how badly your system struggles to run it. This would just be for fun, since normal gameplay **should never overload the simulation.**

## New wallpapers
Our brilliant logo and wallpaper graphic designer, **Nich Guilbault**, has updated the four Socially Distant wallpapers. You'll be able to see them used in future screenshots, as well as throughout the acidic light community and this website. We hope you like them! <3 

## Community overhaul
Throughout the month of March, the entire acidic light community has been overhauled.

[This website](https://sociallydistantgame.com/) as well as [acidiclight.dev](https://acidiclight.dev/) have been completely redesigned, and are now open-source on GitHub! Instead of being hosted on Wordpress, they are now generated from their respective Git repositories and hosted on GitHub Pages. This makes them **much faster** to browse through. They also have proper support for social embedding. We'd like to thank community members **Kainoa,** **TheEvilSkeleton,** and **Brodie Robertson** for helping with the transition! <3

The unused acidic light IRC server was shut down, alongside the community Minecraft server. A backup of the Minecraft world will be posted on the Socially Distant forum sometime soon.

Speaking of the forum, CDN support for image uploads has been added. This means that images uploaded to the forum will be delivered to your device much faster than they would have previously been, and that there's a lot more space to handle user uploads. In fact, the images in this devlog are all hosted via the forum's CDN.

The acidic light wiki has been renamed to the [acidic light community wiki](https://wiki.acidiclight.dev/). Both that wiki and the [Socially Distant Online Manual](https://man.sociallydistantgame.com/) have been upgraded to the latest (at the time of writing) MediaWiki version, and both support logging in with your acidic light community account. They also run on the same core MediaWiki install, meaning that features available on one wiki are also available on the other.

All community services are now hosted in Toronto, Canada across multiple servers. This means there is no longer a single point of failure, making the entire community's infrastructure more reliable. And did we mention it costs us less? Currently, half of the server upkeep cost is covered by our amazing Patreon supporters!

All of these changes to the way the acidic light community is run may allow for online multiplayer for Socially Distant in the future. This is something we don't officially have plans for at launch, but if there's enough interest for it, it's definitely something that's possible now.