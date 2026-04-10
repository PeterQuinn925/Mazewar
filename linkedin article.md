https://www.linkedin.com/pulse/writing-bot-play-ancient-computer-game-peter-quinn-k7fjc/

I'm working on a bot to play Mazewar for an exhibit at the museum where I volunteer. 

We're building an exhibit at Museum of Digital Arts and Entertainment (MADE https://www.themade.org/) to show the history of First Person Shooter games. One of the games in this exhibit will be MazeWar on the PDP-10. https://en.wikipedia.org/wiki/Maze_(1973_video_game)
Article content
Imlac Terminal running Mazewar (via Wikipedia)


We don’t have a PDP-10, but there is an excellent emulation running on the Raspberry PI using https://github.com/obsolescence/pidp10. 

One of the things that makes Mazewar notable is that it is one of the first multi-player games. For logistics reasons, we can't have multiple players for this exhibit. Without an opponent, Mazewar is very boring. To help make it more interesting, I'm writing a bot to provide one or more enemies.

The computer that runs Mazewar is a PDP-10 that running a time sharing operating system that can handle many terminals. Mazewar runs on a specialized vector graphics terminal called Imlac. The Imlac terminals are actually computers in their own right.

I went down quite a number of blind alleys on this project, helped along these blind alleys by using Claude Sonnet 4.5.

My approach for this project was initially to just reverse engineer everything using Wireshark. Wireshark is a program that can watch the networking traffic on your computer. My thought was that I would run an Imlac emulator and Wireshark on my laptop and connect to the PDP-10 and use the data collected to reverse engineer the protocols needed to build my own bot. 

As a first person shooter, once you log into the computer and start the game, you see a 3D representation of a maze. The arrow keys move you forward, back, left, and right. You can also shoot. 

Wireshark was easy enough to install and write a filter to get just the traffic that I was interested in. I got the network traffic data and gave it to Claude. Claude accepted the data and produced a python program that it confidently said would be a working bot.

It was not. The code ran but there was no evidence of a bot running around in the maze. I went back and forth with Claude trying to get it to work. I ran Wireshark on the bot code and was eventually able to get it to log on. Claude had omitted a key control character needed to log on.

Then I spent quite a bit of time trying to figure out why I wasn’t getting any response once I the bot moved. I could see a large amount of data coming back from the PDP-10 when the game started, but basically nothing after that.

Turns out, I assumed incorrectly that the Imlac terminal was just a display and that every time you moved, the PDP-10 would recalculate your position and send the view back. This isn’t how it works. The big chunk of data sent upon game starting is the executable code that the Imlac terminal loads into memory and runs! The Imlac terminal is a computer running the game, drawing the visuals. All it sends to the PDP-10 is the position of the player and which direction they’re facing. 

I was going to have to load the maze into my bot and make sure it navigated properly. I found out during this process that the Imlac or my bot is responsible for making sure it only sent movement commands that were valid.

I had Claude attempt to reverse engineer the blob of binary data that came back from the PDP-10 during game initialization. Although it claimed to do it each time, it never actually accomplished it. I looked at it myself and I couldn’t figure out what I was looking at.

Thankfully I was able to find annotated source code for the Imlac side of things online. This cleared up a ton. It had the bitmap of the maze as octal words. I again tried to get Claude to decipher the octal representation, but I gave up after several attempts and wrote the code myself. What it had written was off by one and needlessly complicated.

At this point I have a bot that navigates the maze. I have some more work to do to finish it. I want to make it so that I can have more than 1 bot at a time. It uses a follow the right wall maze navigation method that is susceptible to getting stuck in a loop. I need to fix it so that if it gets to the same place more than once it turns in a different direction.

Fun project. I learned that while having Claude write code for me is seductive, it can be frustrating and ultimately take longer than putting in the effort myself. 
