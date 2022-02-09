# No Comment Explanation and Information

I'm going to share more information about nocom here, the parts that didn't make sense to put into a Fit script.

**Edit 7 July 2021: I started writing the wiki page [here](https://2b2t.miraheze.org/wiki/Nocom)**

**Edit 8 Feb 2022: I made the source code of nocomment-master public [here](https://github.com/nerdsinspace/nocomment-master)**

Here's the -250k to +250k overworld heatmap at 4K resolution:
![Image](rect_4k.png)

And [here](https://commons.wikimedia.org/wiki/File:2b2t_Nocom_Overworld_Heatmap.png) is 8K resolution.

# Suggested prereqs

[The FitMC video](https://www.youtube.com/watch?v=elqAh3GWRpA)

Also the teaser trailer is cool: [YouTube (lower quality)](https://www.youtube.com/watch?v=3ayxeruAan8) • [Discord (higher quality)](https://cdn.discordapp.com/attachments/737054779582185522/864051484203286529/nocomment_teaser_trailer.mp4)

Also if you want to see very cool timelapses / renders of nocom data look at these two youtube channels
* [MLGaeming](https://www.youtube.com/playlist?list=PLOxa3ecQg7Kixbh1ZXrxJpmoIUUJxy_fv)
* [Negative_Entropy](https://www.youtube.com/user/mcmichiel2307/videos)

## Corrections to Fit's video
* It did not cause an "out of memory" on 2b2t, but rather tick lag to the point where the PaperMC watchdog process would print out the stacktrace. Not a big deal though since the end result is the same (server crashes with stacktrace).
* Fit implies that it was never used to lag / queue skip, but it was used that way as early as April 2018.
* The order of events around BibleBot is incorrect, BibleBot was only locked in late 2020, more than a year AFTER Hause implemented the first packet limit. Not a big deal though. Perhaps this was god's way of showing that he was upset with this exploit. We'll never know for sure though.
* I (leijurv) was only brought on in 2020. He didn't say explicitly, but he implied 2019.
* Remote viewing was actually possible from the very beginning. For example, while it was implied that the dipper nation remote viewing screenshots were from my system, they were actually from the earlier method of simply clicking every block in the chunk. This was known and used from day 1, I cannot take credit for it. In fact, I wouldn't be told about nocom until over a year after those posts.
* Obviously, this should go without saying, but the exploit cannot literally "predict the future". When Fit was talking about Beardler's stash/base, that has been misunderstood by a lot of people. What happened was that the exploit tracked many people to that location, it was keeping up with downloading the blocks at the base, and it saw a lot of TNT being placed. The conclusion of "this means the base is about to get griefed" was NOT made by the exploit or any autonomous software. Really, we caught wind that this was happening, and I pulled up the latest remote downloader data, and took those screenshots fit showed of builds covered in TNT. Obviously, I saw that and thought "it's about to be griefed". Later on, Fit asked for a timelapse render of accounts traveling to the location nether-side, which I also made from historical track data. We also were able to pull up who was most strongly associated with that base in the past, demonstrating that it actually was Beardler's stash (or, at least, that beardler's account was more active at that location than boofer's). So, on the one hand, it was fully autonomously collecting this data, such as players converging on the location and TNT placements, and that is data that we could view and connect the dots on to understand what happened, but it did not connect those dots itself. It's not like we got a automated notification "big base has a lot of TNT being placed on it right now" (although, to be clear, something like that would not have been difficult to create, an automated notification like that would have been perfectly possible given the data that we were collecting, we just... never decided to do that).

Other than that it is **essentially perfectly accurate**, Fit did an impressively good job editing it all together with all the media and such that we dumped on him. This video is different from most because the script was edited for clarity and technical correctness by all of us, and we worked closely together, such as making certain heatmap renders and timelapses based on what Fit wanted to show and talk about.

## Is my base compromised by nocom?
Did you:
* Spend a nontrivial amount of time logged in at the base (leaving its chunks loaded) from mid July 2018 to August-ish 2019? (and the base is within the first few million out from spawn)
* Travel to the base (at any distance from spawn) by any means other than teleporting, from late March 2020 to early July 2021? (such as by walking, elytra flying, boat phasing, anything)

If either of those is true, then more likely than not, yes it's in the database.

# The entire thing summed up in three sentences

The exploit allows us to ask 2b2t "Is the block at **this** coordinate in a currently loaded chunk?" and get an immediate yes or no answer.

If the chunk is loaded, 2b2t replies telling you the block at that coordinate (like: "x=0 y=0 z=0 is bedrock"), if the chunk isn't loaded, 2b2t does not reply.

There is no distance limit; you can get data from millions of blocks away.

# But how often can you do that?

From 2018 to late 2019, there was absolutely no ratelimit applied to this packet. If you had gigabit upload, you could saturate it with CPacketPlayerDigging packets. The practical limit was that eventually 2b2t would crash if ticks started to take many tens of seconds to process the backlogged packets, but that was a hard limit to reach.

The question of "how rapidly can you spam it" is important because it defines the difference between a simple spiral scan being plausible, and a much more complicated system being necessary.

Hause implemented a 500 packet per second limit in late 2019, with a server kick if it was exceeded. Due to jitter, movement packets, lag, and other factors, we practically were able to send 440 exploit packets per second. This is factor 22 because it's 22 packets per tick. In late May 2020 it was limited down to factor 14 (ish), then down to factor 8 (ish) the next day, where it remained until July 2021, when it was limited to factor 2. At this point, Hause appears to have realized that he could not ratelimit it lower without kicking vanilla behavior such as digging netherrack with Eff V. (he actually limited it down that far, to factor less than 1, then relaxed it back up to 2). On 2021-07-15 he actually patched it.

# PaperMC (origins of the exploit)

This exploit only exists in [Paper](https://papermc.io/). The patch is [here](https://github.com/PaperMC/Paper/blob/ver/1.12.2/Spigot-Server-Patches/0196-Fix-block-break-desync.patch), it's the 196th patch to Paper for 1.12.2, called "Fix block break desync". It does indeed fix block break desync, it just also causes the exploitable behavior of revealing what chunks are loaded.

## PaperMC in depth

The reason why this has flown under the radar for so long is that there is no actual "exploit" or "backdoor" in the sense that you might think. In other words, the server doesn't "misbehave" or do anything suspicious. It's fully expected and intended behavior, the code doesn't do anything sneaky or surreptitious, it's actually perfectly simple.

The fact that you can click a block and get its contents (due to this patch) is a well-known fact. Many mods have used this over the years. For example, Orebfuscator is a server plugin to combat X-Ray. It doesn't send where ores actually are until you get close to them. Sadly, you can simply abuse this patch and click every block in every chunk in render distance, and the server will dutifully tell you which blocks are truly stone and which are truly ore. This anti-orebfuscator module has been in many mods. Constantly punching blocks near to you to fix ghost blocks or desync is also a feature in many clients.

Ghost blocks happen when your client and the server fall out of agreement on what blocks are where. For example, you might mine a block, but the server TPS is low so it doesn't accept that you did that. The server puts the block back. This doesn't happen automatically, the server has to detect this has happened and explicitly tell your client to put the block back how it should be. This fixes the desync. As another example, imagine you walk forward 4 or 5 blocks, and mine a block. But it turns out the server was lagging out so it rubberbands you back and resets the block back to what it was, since it didn't think you were actually close enough to reach it.

That last bit is what this patch does. If you attempt to mine a block that's unambiguousuly too far away to reach (6 blocks away), this patch puts the block back how it was. If this patch weren't here, then in the example I just mentioned you'd get rubberbanded, but the block wouldn't necessarily get put back. It would desync and become a ghost block. This patch is what patches that.

So this is known and desirable behavior on the part of 2b2t, it's just that not many people thought of intentionally going outside one's render distance to extract information. The realization is that you can click absolutely any block, anywhere on the server, even a million blocks away, and learn if it's currently a loaded chunk, by if the server responds to you or remains silent.

Using common sense, the Paper developers _intended_ for this patch to reply to the player only if the chunks were loaded _by **your** player_, as that would make logical sense (that's all the blocks you could reasonably be digging in good faith). The problem is that the way the code was written, the server will reply to you if the chunk is loaded _by **any** player_ on the server, which is clearly an unintended side effect.

# How it was exploited by us

## Spiral

The simple approach to get coordinates from this exploit is to click one block per render distance, going out in a spiral.

This works, and we did this for a bit over a year, starting July 2018 on the day it was added. Mostly fr1kin running a bot that would spam this packet, while mining for ores with Baritone to prevent an AFK kick.

Chunks could be downloaded for remote viewing by simply clicking every block in the chunk.

## Adaptive tracking system

### tl;dr
With the ratelimit, we needed a smarter system. It was no longer practical to happen upon people at their base through a brute force spiral search, we don't have the packet budget for that anymore. So, instead:

* Multiple bots in the overworld and nether
* Intentionally offset shift schedules so that, even with queue and a 6-hour kick from 2b2t, there is always at least one online
* Start by clicking the axes and diagonal highways every once in a while, to catch players traveling
* Keep up with them as they travel on the highway, as they turn off and travel to their base/stash/wherever, and tell the overworld bot to check that dimension once they disappear from the nether
* Keep track of how long they spend in each area, if it's 90 minutes in one area and far away from spawn / highways, count that as a base
* Start downloading bases by incrementally clicking the blocks in it, one at a time, looking for and then expanding upon any differences from vanilla terrain gen at that location
* Store all this in a giant Postgres database (by the end, 1.7 terabytes, 13.5 billion rows)
* Use the data for web UI, queries, reports, etc. (e.g. print out bases with the most chests, travel to them ingame, steal all the items)

### Longer explanation
[Particle filter](https://en.wikipedia.org/wiki/Particle_filter)

[The specific section that Fit showed my screenshot of](https://en.wikipedia.org/wiki/Particle_filter#Monte_Carlo_filter_and_bootstrap_filter)

[Monte Carlo localization](https://en.wikipedia.org/wiki/Monte_Carlo_localization)

[The paper that I referred to most often, it helped me decide to use stratified resampling instead of multinomial](https://users.isy.liu.se/rt/schon/Publications/HolSG2006.pdf)

#### Battleship metaphor

Btw I told Fit to use the battleship metaphor so it's not stealing for me to use it here, quite the opposite!

Anyway, instead of ships, the hits or misses are against player render distances. Every player on the server loads the chunk they're standing in, plus 4 in every direction in a square, for a total of 9x9. This is just how the 2b2t server software happens to be configured; other servers will have a different sized square. So instead of puny little battleship ships we have nice large 9x9 squares. but the problem is that, well, they move. They can overlap each other, and you don't get "double hit" you just get "hit". They can teleport, such as loading a pearl or respawning to a bed. they leave and join the server. They go through nether portals. (and if they're in or near new chunks, the shape can be literally anything, far less than 9x9 in size, but thankfully that doesn't come up that much)

2b's world is still far too big to randomly try coordinates. Sure, you can try a hundred a second. Imagine you did a cute little spiral going out from 0,0. Since render distances are 9 by 9, we can actually just skip to one chunk in every 9. Even spiraling like this, it would take days to spiral out to where the interesting bases, in the mid hundred ks range. And people would certainly slip through the cracks, because **you can scan 24/7, but you only get a hit if someone is online at that exact moment**, loading the chunks. Very few bases have players online all the time or even most of the time. If people play 2 hours a day, so just under 10% of each day, you'll miss over 90% of them on any given run because they're just not online.

We start with two bots on offset shift schedules in the nether for continuous coverage. These bots have changed, but for most of the time it was `Babbaj` and `Elon_Musk` on nether duty. They scan the nether highways (really, punch one block per every 9 chunks, expanding outwards on every highway and diagonal, sort of like radar).

When we get a hit, we've found a player traveling. Perhaps traveling to a base?

So..... we just keep up with them. We devised a system, using a monte carlo particle filter to simulate and track movement, that uses about 2 checks per second to keep up with a player as they travel at arbitrary speed. Elytra, boat, entity speed, sprinting, walking, anything. Even pig god mode! Even spectator mode! All we care about is if chunks are loading, that's what we can see.

Basically, we built a machine that plays this game of battleship against 2b2t, really well, using up all the hundred some checks a second we get, and uses it to follow the battleships as they move around the board.

And when a battleship disappears from the nether board, we look over at the overworld board and keep going (the bots coordinate with each other of course).

In this way, we simply, just, follow people to their bases by keeping up with them as they load chunks. From our observation posts we can check chunks all over the map, at any distance.

okay that was the late preschool / early kindergarten explanation. let's get technical.

The monte carlo particle system simulates 1000 potential player positions and velocities (referred to as "particles"). As hits and misses come in, the more likely particles reproduce, and the less likely particles are culled. Across many generations, the population settles down around a very accurate position and velocity, to sub-chunk accuracy after just a few generations, which are one a second. For example, for someone on a nether highway moving at a constant speed, this is able to keep up with 1 check a second, once it gets their trajectory down cold.

We also incorporated some other strategies. For example, when someone joins the server, we lookup the most recent time they left, then we lookup all the tracks that went cold within 1 minute of that time, for each of those tracks we send a few checks sprinkled around the area to try and pick up where we left off with tracking them. A few other techniques like this allow tracking to be extremely resilient. After the massive ratelimit in May 2020 (likely due to 0neb), it was just barely at the edge of what's even survivable for this exploit, but we pulled through, tweaked priorities, decreased scan net sizes, wrote some special cases for stationary players, and more. it took a week of fixing (of which 90% was complaining and 10% was development), but we got back up online.

It's easy to forget, but this isn't just for checking chunks. the server is actually sending us the block! We created a very efficient base downloader by essentially "paint bucketing" or "flood filling" the modified blocks compared to 2b2t's seed to get all ground level structures. this allowed us to remotely tell what coords were interesting bases and which were just shitshacks. You'd be shocked how far people go out, millions of blocks, just to build a tiny fishing hut and afk. This downloader is quite complicated and runs 24/7 afk. It seeks out the locations in the chunk with the most changes, and rechecks around them periodically, as often as once every thirty minutes. This allows us to keep up with bases as new buildings get built or torn down. We also redownload from scratch every three weeks. So, we don't just have coords of every base, we don't just have a world download of every base, we actually have the entire history of the construction and possibly destruction of the base, block by block, hour by hour, quite high resolution.

### Nitty gritty
How do we actually make this happen, nitty gritty? The old version was super simple. A forgehax module was written to do the spiral search and write down the coords to a file. It's actually exactly the same as that elytra stash finder, except it does it remotely and doesn't have to fly. Fr1kin ran it at his house for over a year. The files would be sent and added to a website that showed em all.

We had to do something a little bit more complicated here. The base requirement was the communication between overworld and nether bots, and it sorta spiraled (lol) out of control from there.

So, we start with the primary bot instance, which is a DigitalOcean droplet. This was picked to have the absolute lowest ping to 2b2t. (well, once you get close enough, it starts to not really matter though, since everything goes through proxypipe, so you really don't want to get close to 2b2t, but rather close to the closest proxypipe endpoint to 2b2t. and not really closest, but lowest latency) Anyway, DigitalOcean NYC1 is perfect, with pings consistently under 20ms. This allows us to squeeze the absolute maximum packets per second out of the connection, with close to instant detection of network lag causing us to throttle down to almost nothing. This allows the highest sustained checks per second across the day.

The server ran a very cute headless minecraft we developed. It's able to save on RAM by having a single Java process have multiple fully fledged minecraft instances. To be clear, this isn't anything fake like reminecraft or a proxy. This is a **true** **full** minecraft client, just with all the rendering and keyboard and mouse stuff ripped out.

So that's the bots, but what does the master do? First and foremost it provides a layer of abstraction over the bots and 2b2t. TCP connections can sometimes drop, the bots can get kicked from 2b2t, a bot can randomly die and respawn in a different dimension (rarely), etc. So, there is a system of priority based queues and checks. A "task" can be created, which is a straight line list of chunks to check. For example, a task might be "start at 0,0, go +x and send a thousand checks, spaced out one every nine chunks". Or a task could be as simple as "check this particular chunk". The world and connection system handles the actual communication to the bots, and all that. If a bot drops offline, the pending tasks are reassigned to another in the same dimension. If they all drop, it's held for when they reconnect.

Sitting on top of all that, there is the system of scanners and filters. A scanner is something that tries to fish out people to begin with. The simple mid-priority ones are just highways. We also do the overworld ring at 2k out VERY commonly (once every sixteen seconds, at the moment). A little bit lower priority is the retry scanner, which looks at past bases and tries to suss out other members of the base that login at different times of day. It is continuous but low level: using up 5 checks a second. And finally lowest priority is the spiral scanner which just scans outward to 300k over and over. It can take hours to days. It isn't really crucial, it only exists as a last resort "Well if you aren't doing anything else important, might as well". The priority system is simple and strict: all the highest priority tasks are done before any lower priority ones get to start. If all the time is spent on the filters (which are highest priority), the scanners will never run at all.

TODO put in a table here of what each priority level is

Once we get a hit from one of the scanners, we turn to the tracky tracky manager. This checks if we already have a filter tracking this particular player, and if not, starts a new one. These filters are by default in monte carlo mode, as described earlier. The monte carlo mode is a great all-arounder. It has a lot of backups where, when it loses track, it's able to take more of the check budget to find them again, but when it has a good lock on their position and speed, it subsides back down to about 1 check a second minimum. The average is a bit over two checks a second. When it thinks it might have lost someone, it can go up to ten per second, and if it's completely lost someone (defined as no hits in 5 seconds), it declares the track over, it's lost them. This triggers a hail mary check where we do a grid of 11 by 11 checks spread out 9 chunks apart. This is really expensive as you can see: that comes out to 121 checks which is a full second of our budget. It's worth it given how rare it is. We also trigger a similar check in the other dimension, assuming they went through a portal. But when it works, it works well. When the most recent hit in this mode is within a render distance of all hits in the last 30 seconds, we switch to stationary mode. This is because we have determined pretty much exactly what chunk they must be standing in (to explain the last couple dozen hits). We switch to just checking that one chunk over and over. We slowly get less frequent, all the way down to 1 check every 8 seconds. This is 10x slower than the monte carlo mode. It turns out, the vast majority of our active tracks are using this at any given time. Generally people are either traveling, or are effectively stationary. If they never move outside of their render distance from that point, the stationary filter will just comfortably check in every 8 seconds. If the stationary filter misses, we switch back to monte carlo since they're clearly on the move now. 

All of these hits and tracks are stored to the database. Every single time we get a response from 2b2t of "HIT" (this chunk is loaded), we save it to a giant table of hits. We've had over three billion of these. Each hit is also associated optionally with a track. This is what makes the data useful. Every track is assigned an ID. When a track switches modes from monte carlo to stationary and back, we don't change the track ID. But when a new track has to be started, such as when they change dimensions, it gets a new ID. To keep track of that, each track gets a "previous track id". This allows us to trace back the history of how people have traveled.

A hit is as simple as an ID number, coordinate, server ID (almost always 1, for 2b2t, but we've briefly ran this on 9b and constantiam too), dimension, track id (optional), and whether or not it's legacy. Legacy is almost always false, it's just for the hits we imported from the 2018 / 2019 era. There are about three billion of these.

A track is a bit more complicated, it has an ID of its own, the ID of the first hit in this track, the ID of the most recent hit in this track (this is updated constantly), the timestamp of that most recent hit, the dimension and server ID, the previous track ID, and again whether or not it's legacy. There are about ten million of these.

For example, when a player logs into the server, we look up the most recent time they left, then we lookup all tracks that were last updated around there, give or take a minute. We take a guess that maybe this player logging out was what caused those tracks to end, so we attempt to "resume" all those tracks from that time, by just rechecking their last successful hit. This is extremely successful and is a crucial component of tracking people.

Now we have the system that takes in this data and decides what to do with it. I haven't mentioned anything yet about deciding what's a base, and what's just an area someone's flying through, or how we determine who is where, or how we decide what areas we're going to world download.

This first step is called the aggregator. Every once in a while it looks through the hits table through all hits that are new since last time. It filters out hits that are within 100 chunks of a highway, or within 1500 chunks of spawn. It also ignores the nether. Then, it looks up our clustering entry for this chunk coordinate. This is a giant table called `dbscan`. Unlike the hits table, this table will only ever have one row for a given chunk coordinate in a given world. It keeps track of all the activity we've observed at that chunk. When a new hit comes in, we mark down the most recent hit as that timestamp. We also update our list of "timestamp ranges". this is our system for figuring out what areas are interesting and what areas aren't. It's actually really simple: if two hits come in with less than 2 minutes of time in between, we make an educated guess that a player was loading that chunk for that whole duration, and we add that range (from first hit to second hit) to our list of timestamp ranges for that chunk. If a new hit comes in that's also within 2 minutes of the most recent, we increase the range. If a new hit comes in that's outside the 2 minute range, we don't mark the range in between as occupied. This allows us to keep track of what chunks are actually occupied (i.e. bases) and which ones just have people passing through. For example, our stationary filter will do this easily. The monte carlo filter will as well, but unintuitively. Even though it spams checks all throughout your render distance to determine exactly where you likely are, it will in all likelihood hit any specific given chunk about once a minute. The purpose of the timestamp ranges system is so that we can keep track of how interesting a given area is, without bias by our two filter modes, which are wildly different from each other: stationary sends less than one tenth the amount of checks per second as monte carlo, and monte carlo spams checks all throughout the 9x9 render distance while stationary just checks one chunk over and over. Creating this way to count up how much time has been spent at a given spot, while maintaining completely neutral behavior between either filter being used, was the hardest part here.

Then, once any given chunk has built up 5 minutes of online time, we mark it as a node. This means that the chunk could be interesting but we don't know. This can happen in as little as 3 checks each spaced 2 minutes apart. Marking a chunk as a node adds it to a GiST index. (remember: this is a Postgres table so we can put whatever partial indexes we want on it) In this case, we chose to put an index on all rows with is_node true; that gives us the ability to query all nodes within a radius of 512 blocks efficiently.

Whenever a chunk is marked as a node, or a node gets a new hit that lengthens its timestamp ranges, we queue it for an update by the next step. We have a "to update" table with a node ID, a lower bound, and an upper bound. These are initially set to 1 hour and 6 hours in the future. If a new hit comes in, we push the lower bound to an hour in the future from the new hit, but we don't touch the upper bound. When either the lower or upper bound is reached and becomes in the past, we update. In other words, we schedule an update at least 1 hour in the future, but no more than 6 hours in the future. If new hits come in, it can be pushed back to the original 6 hour mark, but no further. So updates happen at least every 6 hours, and at most every hour.

An update is what decides if the area is interesting, not if the chunk specifically is interesting. We query all nodes within 256 blocks of radius, and union all their timestamp ranges. For example, if one chunk has a timestamp range from 4pm to 5pm, and the adjacent one has a timestamp range from 4:30pm to 5:30pm, this comes out to 4pm to 5:30pm, so we would say 90 minutes of combined duration. It's very important that we combine the ranges properly and don't simply add up the durations. (otherwise we would be heavily biased towards monte carlo filter and away from stationary) Once all the nodes within a 256 block radius come out to 90 minutes of combined occupancy duration, we mark the center node we're looking at as a "core" node. This means we've made the decision that this area is probably a base and we're interested in keeping track of it. Then, we look at the entire 512 block radius, and mark everything in that range as being part of the same cluster. Only a core node can start a cluster, but once that's happened, normal non-core nodes can become members. This is done by setting the `cluster_parent` field to the ID of the parent. If new hits occur in between two different clusters, they can be easily combined by setting one core node's parent to the other, making one big cluster. This is called a [disjoint set data structure](https://en.wikipedia.org/wiki/Disjoint-set_data_structure) and it's very useful. We implement path compression and unions by rank.

The cluster retry scanner mentioned earlier takes advantage of this. We pick a random cluster (really, a random core node with no parent, which will be the root of a cluster), then pick a random chunk that's a member, then check that. This allows us to recheck bases without being biased towards bases with a larger area: we pick the cluster at random equally. If we picked chunks at random equally, it would be biased towards bases with more chunks.

Something else that uses this data is the associator. Running one day behind real time so as to not mess with any of this, the associator looks at all tracks that ended at interesting locations (not at spawn, not on the highway) and were more than 3 minutes long. For each one, it grabs all players that logged off between 6 seconds before and 1 second after. If there were more than 10 such players, it ignores since this was a server restart. Otherwise, it divides up 1 "association point" equally among all such players. Then, it looks at where the track ended. If it's within a cluster, it adds that association point fraction to THAT player <-> THAT cluster. Over time, this builds up who logs off where. It actually works pretty well.

Finally, the clusters are used by the slurper. This is what clicks blocks one at a time to download entire bases. It starts by ingesting the firehose of all hits (no matter if it's from a filter or a scanner) in real time. These are added to a queue then processed by marking the whole render distance as recently loaded, but only if it's a member of a cluster. Then, four times a second, we pick the furthest chunk away from spawn that we haven't picked like this in the last half hour, that was also marked as recently loaded in the last 30 seconds, then we "seed" that chunk. "chunk seeding" is the process of picking locations in the chunk that we'd like to check to determine if there's a base there or if it's untouched. We do this by asking the world generator for the entire chunk then picking 4 random coordinates on the surface. We click the top block, and the air block above it. If we've slurped this chunk before, we also do the highest non-air block and the lowest air block in the same way (this keeps underground bases and skybases with no connection to the ground up to date). We also click a few completely random blocks (just in case there's a skybase or underground base, that'll get it eventually).

Whenever we got a response in the slurper of what a block actually is, we first check if the response was loaded or unloaded. Remember any of these checks can fail because the chunk simply isn't loaded. If that's the case, we add the check to our list of unloaded responses. If we ever get a hit at that chunk in the future (by a scanner, filter, or this slurper), we will resend all the checks that got this unloaded response. But assuming the common case where the chunk is loaded, first we check if this matches the vanilla worldgen, and we check if it matches a previous check here, if any. So, we're comparing the current response to vanilla and to the previous response (if we got one). If it's different from the previous response, that means someone has modified blocks here since last time, so this is interesting. We "brush" check it, which means from -1 to +1 on all 3 axes we send checks to renew. If this block has changed, maybe adjacent ones have too. We do the same thing if it's different from world gen, but we don't force a renew. There is a system of "must be newer than", where the slurper can ask for a certain coordinate and also say that they want a response that's fresher than a given timestamp. If we have an older response saved in the database, it'll be discarded. If we have a newer response saved, we'll give it instead of actually asking 2b2t. The seeds are designed to catch additions to builds: changes in the highest or lowest Y coordinates in the build are common, maybe the tower is being built upwards, or the mine dug downwards. The "brush" system will refresh all those locations once a difference is found. And if that isn't smart enough to catch the changes, the base "must be newer than" is set to three weeks, so once every 3 weeks we will essentially "forget" the base and reslurp it from scratch. Within the meantime, we're just keeping up to date little areas every half hour that Might indicate a change.

There are a bunch of complicated systems underlying the slurper that let it do its job properly. For example, I mentioned earlier that when a connection is dropped, its pending tasks are redistributed to other connections; naturally the same thing is done for block checks. Additionally, the database is checked as a cache for checks we've already done etc. That's all standard stuff, the goal being to never ever waste a block check by sending two for the same location in quick succession, even if two different parts of the code asked at the same time. We have a very complicated priority upgrading system that runs the whole way through, from the slurper all the way to the bot minecraft instances on digitalocean. When we brush check from -1 to +1, we set the priority to the inverse of `abs(x)+abs(y)+abs(z)`. This means that the immediate adjacent blocks are higher priority than the diagonals which are in turn higher priority than the corners. However, if one of the immediate adjacent checks comes back to us and it turns out to be a difference too, we do the same thing, except now we're resending the same check but higher priority. The whole system is built to carry out this priority upgrade quickly and correctly, with the most important aspect being that we only do the upgraded check, and don't in any scenario leave the old low priority check to be run uselessly at a later time. With multiple bots taking these checks and carrying them out at a rate of hundreds per second 24/7, plus random network latency, plus 2b's latency, this is not that easy.

So that's how the slurper ended up with a table of over ten billion blocks, forming a record of the construction of just about every base.

# More stuff to write about TODO
* Manual bot kicks from 2b2t
* Tracking Hause in spectator mode
* How this interacted with the tp exploit
* Signs NBT
* Comdar
* Temp maps (april 1 maps)
* Highway data
* Player stats
* Proxy system
* Memes to cover up the exploit (in more depth), and other miscellaneous related gaslighting
* List of all bases that fell due to this
* Interesting human psychology things such as the massive flurry of bases RIGHT after hitting 100k or 1 million (on any axis)
* Visual patterns in the heatmap (long straight lines in the nether from boatphase, squares in the overworld from elytra stashfinder / world downloader)

The half million block wide view again:
![Image](rect_4k.png)

(okay actually it's X = -245760 to X = +245759 on horizontal, and Z = -138240 to Z = +138239 on vertical, those numbers being chosen because at this size, 3840x2160, one pixel comes out to exactly 8x8 chunks, which is very close to 2b2t's render distance, 9x9. so one pixel is basically one render distance. but it's much cooler to just call it the "half million block wide view" or round up to "-250k OW to +250k OW")

* Why leak the heatmap to 250k?
Answer: The other group that knew about the packet spiraled out to at least 266k (that was the furthest axis I saw in the first leak video). It turns out that that is pretty close to 3840x2160 at 1 pixel = 1 chunk, nether (`3840/2*8*16 = 245k`). So, it is aesthetically pleasing, and everything in this range has perhaps already been found by another group. So, everything sent to Fit, and the image shown here, are limited to 250k overworld and the equivalent 31.25k nether. That's why the heatmap that he showed cut off at 31,250 blocks nether-side. We think this strikes a good balance between showing cool stuff and not leaking everything in existence.

Proof of twitter: https://twitter.com/leijurv

Check out what the Grafana dashboard looked like by the end: https://www.dropbox.com/s/ktlvvtmppq64u8o/trimmed%20grafana%202.mov?dl=0
