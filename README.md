# An implementation of ping via arp lookup

Description:

Arp-ping.exe is an implementation of "ping" over arp lookup. It is similar in behavior to the "arping" *nix program, but is an independent implementation and works somewhat differently internally. In particular, arp-ping.exe does not require Cygwin to compile nor that there be a packet capture driver (pcap) installed.

Arp-ping.exe uses ARP resolution to find hosts. With this method, it is able to "ping" hosts that are firewalled - in particular it has been found to discover hosts using Windows Firewall as well as hosts using "stealth mode" in the Mac OSX Firewall. As the ARP protocol only functions at layer 2 of the OSI model, arp-ping.exe is only able to perform this task to hosts that are on the same local network that you are. If they are on the other side of a router or other layer 3 device you will get no results.

One caveat to be aware of. In my testing with windows XP, I found that XP would aggressively use the arp cache to answer the SendArp() function. Thus I have included a "-d" option to arp-ping.exe. This option executes a command line "arp -d *" before every ping attempt to avoid this problem. That command requires Adminstrator rights and may be expected to misbehave if you do not have them. In actual usage - this may or may not matter to you. If the host is already in the "arp -a" cache, that means it has been seen by the operating system recently anyway. If you are trying to watch a host that is going up and down, however, this would be problematic. Under Windows 7 I did not find this to be an issue - I was able to detect a host being connected/disconnected without any confusion with the arp cache. (TL;DR: Use the -d option while under XP and be an Administator for best results.)
# change log
 - Update - Aug 17, 2012. Added "-c" option to emit the date and time on each line.
 - Update - Aug 19, 2012. Added "-l" for additional debug logging, "-m X" to ignore failures that happen faster than X milliseconds and "-." to print a dot for ignored failures.
 - Update - Jan 31, 2014. No new version, but switched to statically compiled to remove .dll dependency
 - Update - Apr 13, 2016. Added "-w X" to wait X milliseconds (or so) and then abort the entire program in a fiery explosion (that is, the return value '3') if there is no response from the target. This is sloppy and somewhat imprecise - essentially its a second thread running a timer and aborting the whole process if it ticks over. Its not likely to get much better than that - the SendArp() function itself doesn't have an option for timeout so I can't force it to go any faster without these desperate measures.
 - Update - Sep 29, 2016. Tweaked the -w option so that it will accumulate over multiple timeouts for large values of -w.

# Auther:
https://www.elifulkerson.com/projects/arp-ping.php
