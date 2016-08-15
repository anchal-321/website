+++
type = "blog"
author = "axeld"
title = "BFS incompatibilities"
date = "2005-10-17T14:05:00.000Z"
tags = ["bfs", "cd boot"]
+++

First of all, I successfully booted Haiku from CD-ROM from several machines today. It took a bit longer than I thought, as no emulator that I have access to seems to support multi-session CDs, and not every BIOS I have works by the book. The boot device selection is still very simplistic, so it might not end up booting completely from CD if you just inserted it, and didn't choose "Boot from CD-ROM" in the boot loader - but you'll have to bear with that currently. I'll probably fix that tomorrow.<br />Anyway, you could build you own bootable CD image with the "makehaikufloppy" script that's now in our top-level directory (it's still rough, and you have to build the whole system manually or via "makehdimage" before). You just need "mkisofs" and use the resulting image in this way:<br /><br /><span style="font-family:courier new; font-size:85%; color: rgb(153, 153, 153);">$ mkisofs -b boot.image -c boot.catalog -R -o &lt;output-ISO-image&gt; &lt;path-to-directory-with-boot-image-and-other-stuff-that-should-go-into-the-boot-session&gt;<br /></span><br /><br />As those of you, that attended the Haiku presentation at BeGeistert are aware of, we initially had some problems getting Haiku to run.<br />The reason behind these problems were incompatibilities between our version of BFS and Be's version: the log area is currently written differently in both implementations. As soon as you mount a dirty volume with the wrong operating system, chances are that blocks are written in random order to your hard drive, and thus, corrupting the file system - as Haiku currently crashes rather often, you get into this situation faster than you'd like.<br />Since we expect early adopters to double boot into BeOS (and for good reason), we should definitely get rid of this annoying risk to lose data. It will also be much easier to track down real remaining problems in BFS if all these simple traps are removed.<br />Therefore, this will be the next thing to work on for me. I will start to understand Be's logging format, and then I'll make ours compatible. Thanks to the wonders of "fsh" (no reboot necessary to uncleanly unmount a disk) this shouldn't take too long - I expect to get it done sometime tomorrow.