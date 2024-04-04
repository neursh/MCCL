<h1 align=center>
  Minecraft Cloudflared
</h1>
<h2 align=center>
  A decentralized Minecraft server using Cloudflare services (WIP)
</h2>

## Why?
Not everybody own a server, or have the money to host a Minecraft server, or crazy enough to run the server on their machine for the whole day, even when they're not playing. Free server provider usually really bad in performace, sketchy and not close to some countries. Creating the whole experience fills with lags, low TPS, can't customize plugins & mods... It's a nightmare.

Then I think, why not make the server "decentralized", anyone could host the server, of course it will have security and stuff to prevent bad actors, and you can select who can host it.

## The stuff behind
This was inspired from how Git works, so I'm trying to recreate just a fraction of it, since the files in Minecraft server is quite big.

**The whole idea is**:
- To setup the main server to control other instances, I'll use the Cloudflare Workers to create a simple HTTP server.
- When a host starts a session, the program will check if any other session is running and watch how files and folders in the server changes through out the session.
- When the session ends, a record of timestamps with changed file will be uploaded to the control server.
- To prevent conflict, the server will create a random ID with the latest timestamp for each uploads and add it to the tracking table.
- So when a host ran the server locally and make unnoticed changes to the server, this will cause the date modified value of that files changes.
- To prevent tricks to change the modified dates, for each update to the latest save, the program will check file size of the of the file.

Even though using file hash is the best way to handle this, but due to the file sizes and how many of them, it can't be implemented. And you should only allow someone you trust to host the server.
