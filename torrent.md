Click [here](https://github.com/nerdsinspace/nocom-explanation/raw/main/nocom.torrent) to download the torrent file!

Magnet link: `magnet:?xt=urn:btih:6ZKW7CZTDTLTB7T6VYGUBTITT3YHG2OS&dn=nocom&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337` (click [here](https://tinyurl.com/nocomtorrent) for a tinyurl redirect to the magnet link)

In this data you can see hundreds of thousands of players being tracked at 1-second intervals. Tens of thousands of bases were downloaded, totaling over ten billion logged block events. It's a fascinating slice of life, essentially a year of history of thousands of anarchy Minecraft bases, updated at 15 minute intervals. [Here's](https://www.youtube.com/watch?v=QxOg4djjJks) an example of a timelapse made from that data, for one such base. [Here's](https://www.youtube.com/watch?v=5FiUjdgwG-Q&list=PLOxa3ecQg7Kixbh1ZXrxJpmoIUUJxy_fv&index=2) another and [another](https://www.youtube.com/watch?v=Zuu3gUOyxV0&list=PLOxa3ecQg7Kixbh1ZXrxJpmoIUUJxy_fv&index=3) and [another](https://www.youtube.com/watch?v=QZgnUbMuFL0).

The format is human-readable SQL dumps, compressed using gzip.

You can set your torrent client to download individual files if you like. For example if just want to make your own overworld heatmaps, you can only download `hits_overworld_heatmap.sql.gz`, which is less than 500 megabytes.

The two enormous files are `hits.sql.gz` which is every hit we collected (billions), and `blocks.sql.gz` which is every block the server told us about (over ten billion).

Explanation of each file:
* `april_fools_troll_blocks_backup.sql.gz` Blocks data collected on the April 1 temp map
* `associations.sql.gz` Associations between which players we think were at each cluster when
* `block_states.sql.gz` Lookup table for what each block state ID number means
* `blocks.sql.gz` Every block that we got from the server
* `dbscan.sql.gz` The clustering of chunks into bases
* `dimensions.sql.gz` Dimensions
* `highway_stats.sql.gz` A temporary table for seeing which highways were most active
* `hits.sql.gz` Every hit we collected, meaning that we observed a particular chunk to be loaded at a particular time
* `hits_grouped.sql.gz` Hits grouped by coordinate and by month
* `hits_nether_heatmap.sql.gz` Coordinate and total number of hits, in the nether
* `hits_overworld_heatmap.sql.gz` Coordinate and total number of hits, in the overworld
* `player_sessions.sql.gz` Log in and log out timestamps for players
* `players.sql.gz` Players
* `readme.txt` Links back here
* `servers.sql.gz` Servers
* `signs.sql.gz` We were able to read signs remotely for a short period of time
* `tracks.sql.gz` Metadata about each track (each hit is part of a track)
