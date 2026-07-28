---
title: "Server hardware upgrades"
date: 2026-07-28T12:00:00-04:00
image: "/images/server-upgrades/cover.png"
draft: false
---

For a few years now, I've been using a small form factor HP gaming desktop as a server to run my website and various other services. The "HP Pavilion Gaming Desktop 690-00xx", which I received for free, features an AMD Ryzen 5 2400G (yes, that is a laptop chip in a "gaming" desktop), 6GB of DDR4 RAM, a 128GB SATA SSD, and 1TB of spinning rust.

While this machine has served me well for the low price of $0, I have been pushing it to its limits for a while. The relatively weak CPU combined with the low amount of memory means that I can't run too much on it at once. I ended up using an Asus Chromebox 3 CN65 (codename TEEMO) as a secondary server to put less load on the HP system. While this did help, I also ran into limitations of the Chromebox's Intel Core i7-8550U CPU still not being quite as powerful as I'd like. Having multiple servers also means I need two different systems to cooperate with each other. One of them hosts nginx and uses certbot to get ssl keys, while the other hosts some services which use the same ssl keys. I have a cron job to transfer the keys between the systems, but this is not ideal as it adds more complexity and another thing which could fail.

Another issue I've been wanting to solve for a while is file storage. My files are currently scattered across several devices with no clear organization system. I want a central system where I can store files (with redundancy) and access them across all of my devices. I also need bulk data storage for my services, including [files.tree123.org](https://files.tree123.org). I currently have bulk data storage on the 1TB HDD in the HP system, but 1TB of data storage isn't much and I will eventually need more than the drive can provide. Another advantage of having a networked storage server is the ability to quickly and easily share files across any device running any OS.

To solve both these problems, I want to build a new server. My goal for this new server is to build an Ultimate Everything Server™. This new server will take on the role that my current HP + Chromebox combo has while also providing serveral TBs of networked storage for me and my services. It will also being significantly faster and able to handle more tasks at once.

# The base system

I didn't want to spend too much money on this project (my upper limit was around $500 USD), and given the current RAM/SSD/HDD crisis, this means I will have to reuse some hardware I already own. Luckily, I had some hardware laying around and doing nothing! A few years ago, I found an old custom built gaming system locally that was being given away for free. I was skeptical of a free PC, but after seeing that it has liquid cooling, I knew it must have somewhat powerful hardware. After picking it up and taking a look, I discovered that it has an Intel Core i7-9700K, 32GB of DDR4-3200 RAM, an ASUS ROG Maxiumus XI Hero motherboard, an NZXT 360mm AIO cooler, and an ASUS ROG 850W platinum PSU. I gave the whole PC a good clean and fired it up, and sure enough it actually worked.

While this gaming hardware may not seem like the best fit for a server, it is still very capable hardware and buying a whole system would cost way too much in this economy. There were a few issues though. The PSU didn't come with any cables beyond the 8-pin CPU power cable, the 24-pin motherboard power cable, and a single SATA power cable being used by the AIO cooler. Since this is a gamer-y case, it only has space to mount two 3.5" HDDs. I plan on using 8 HDDs, so this case will not work.

# Extra hardware

The first extra piece of hardware I had to get was the case. After doing a bit of research, the [Darkrock Classico Storage Master](https://www.amazon.com/dp/B0CQZS7KN5) seemed to be the best budget-friendly case with the ability to mount 10 3.5" HDDs. While this case did seem very appealing, there are some issues with it (you get what you pay for), but more on that later.

The next issue to tackle is powering 8 drives. I found a seller on eBay who sells aftermarket SATA power cables for my PSU, which I ended up ordering two of. With two SATA power connectors per cable, I can use 4 SATA power splitters to power a total of 8 drives while still using the original SATA power cable to power the AIO cooler.

Now for the most expensive part, the drives themselves. The unfortunate truth is you have to get really lucky to find drives for a reasonable price in 2026. New drives are absurdly expensive, and refurbished drives also climbed up in price. I chose to play the "eBay Lottery" and got 10x 4TB SAS 7200RPM drives for $260. These drives were once part of an IBM PowerSystem and came in caddies, which would have been originally slotted into a PowerSystem. After removing each drive from its caddy, they can be used as normal SAS drives. In order to use these drives in my server, I would need a SAS HBA. I found a listing on eBay for a SAS HBA which includes two cables to break out the two SAS connections on the HBA into a total of 8 connections for the drives.

# Assembly

Assembly in this case starts out just like any other ATX system. I put the motherboard in and screwed it down in place. The I/O shield is integrated on this board so I did not have to install that.

Before mounting the radiator, I chose to route the radiator fan cables and plug in the CPU power cable. These would be very hard to access with the raditor installed so it makes more sense to do it now. Now I can mount the radiator, but before I could do that, I had to remove the top HDD cage to give myself enough room to mount it.

Next to go in the case is the PSU. This mounts just like any standard ATX PSU. Since this case includes a fan mesh below the PSU, I mounted the PSU with the fan facing down for better airflow. At this point I installed the 24 pin motherboard power cable, the fan cables, and all of the front panel cables.

Now it's time for the HBA. The SAS HBA I bought is fanless, and I heard they can get quite hot, so I had to find a way to cool it down. Luckily, I had a random 12v fan from a different project which I could repurpose for this build. The fan only has two wires, but it can be plugged in to a standard fan header just fine, with the only catch being no speed control.

Finally, the drives can go in. I first had to remove each drive from its caddy. There were two screws on each side securing the drive to the caddy, but I still couldn't get the drive out with those screws removed. The caddy surrounds the drive and seems to be clipped in place. I ended up using a screwdriver to unclip the plastic, damaging most of the caddies in the process. While I would have preferred to not cause damage, these caddies are effectively worthless, so nothing of value was lost. Each HDD cage can now be unscrewed, have the HDDs installed, and be screwed back into the case.

After all of that, the build is now complete!

![Front side of the final build](/images/server-upgrades/front.png)
![Back side of the final build](/images/server-upgrades/back.png)

# Final thoughts

My thoughts on this Darkrock case are complicated. I love the idea of this case, but the execution leaves me a bit disappointed.

The pros of this case are the cheap price and lots of mounting space for various hardware components including HDDs, radiators, and vertical PCIE devices. This case even included some fans and SATA data cables, which I was not expecting at all for a case this cheap.

But everything has a cost, and while this case may not cost a lot of money, it will cost you your sanity. Building in this case is annoying at times. The included bag of screws has every screw type mixed together, so I had to sort the pile of screws to find the correct types I needed. The HDD cages take up a lot of room in the case and can block access to install a radiator. The gap between the back panel of the case and the frame is quite small and the cables connected to my HDDs would not fit. I had to leave half of the back panel not connected to the case because of this. The cable management clip in the back was too small to fit the 24 pin power cable. The HDD cages were slightly annoying to access and felt quite cheap and flexible. The front panel connector was split into several cables instead of having a single large connector (I understand that not all boards share the same common front panel header layout, but a nicer solution I've seen from other brands is to include a breakout adpater). All of these small issues lead to an unpleasant building experience.

Aside from the case, I am overall quite happy with my new server. As of writing this I have used the server for a good few months and have had no major issues. Despite the frustrations, I did enjoy working on this project, and I finally got to crimp an ethernet cable to use on this build! It also feels nice to finally put some spare hardware to work instead of sitting and collecting dust.
