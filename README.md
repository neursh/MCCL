<h1 align=center>
  Minecraft Cloudflared
</h1>
<h2 align=center>
  A decentralized Minecraft server powered by Cloudflare services (WIP)
</h2>

## Why?
Not everybody own a server, or have the money to host a Minecraft server, or crazy enough to run the server on their machine for the whole day, even when they're not playing. Free server provider usually really bad in performace, sketchy and not close to some countries. Creating the whole experience fills with lags, low TPS, can't customize plugins & mods... It's a nightmare.

Then I think, why not make the server "decentralized", anyone could host the server, of course it will have security and stuff to prevent bad actors, and you can select who can host it.

You should only allow someone you trust to use this to host your server, if someone delete some files, it will be a permanent change.

## DIY
There are two things you must do for it to work:
- Setup [**MCCL Workers**](https://github.com/Neurs12/MCCL-workers).
- Distribute your version of [**MCCL Client**](https://github.com/Neurs12/MCCL-client).

## Technical side
- There's a custom file system in place to keep everything checked.
  - I call that NLock. It will create 2 files. Inspired by Javascript mapping system.
  - In MCCL, it will create `server.nlock` and `server.nlock.map.json`.
  - `server.nlock` contains all files in one big chunk.
  - `server.nlock.map.json` contains information about it.
      - Format: `{"filename": [modified_date, byte_offset_from_previous]}`
      - Sorted from the latest modified date.
  - When start MCCL, it will download mapping and check against the local server folder.
  - Then it will find all files that the modified date is not equals to the mapping and put it in update range.
  - In the process, if the files that already passed, but after them is a file that needed to be updated, then passed files will be redownloaded, this funny behavior due to limitation in Cloudflare R2 that not allows multipart ranges request.
- When upload, the whole thing will be uploaded, ready for next use.
