---
title: "Unlimited Storage Hack : How many Accounts can you make ?"
seoTitle: "Unlock Unlimited Storage"
seoDescription: "Learn to create unlimited cloud storage by combining multiple free accounts into one virtual drive using rclone. Step-by-step guide for beginners"
datePublished: Mon Oct 20 2025 07:12:43 GMT+0000 (Coordinated Universal Time)
cuid: cmgyst2xq000102l5eyighcof
slug: unlimited-storage-hack-how-many-accounts-can-you-make
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1760943923295/05f7cd65-b367-4cef-a8ad-443a2fa3075b.png
tags: tutorial, cloud, computer-science, hacking, storage

---

## Introduction

Every cloud platform tries to sell you *more storage*, whether you need it or not.  
But what if you could combine all your free cloud accounts into one massive virtual drive — without paying a single rupee?

That’s the idea behind what I call the **Unlimited Storage Glitch**.  
It’s not a real “hack,” but a clever use of an open-source tool called **rclone**, which lets you connect multiple Google Drives, Dropbox, OneDrive, Mega, and more into one big drive.

Let’s break it down step by step — even if you’ve never used rclone before.

## Step 1: Install rclone

rclone is a powerful command-line tool that can talk to almost every major cloud platform — including Google Drive, Dropbox, OneDrive, Mega, Box, pCloud, and others.

Download and install it from the official site:  
https://rclone.org/downloads/

Once installed, open your **Terminal** (Linux/macOS) or **PowerShell** (Windows) and verify:

```bash
rclone version
```

If you see version info, you’re ready.

---

## Step 2: Connect Your Cloud Accounts (Remotes)

In rclone, every connection to a cloud platform is called a **remote**.  
You can add one for each account you have — for example, multiple Google Drives or different cloud providers.

Start by running:

```bash
rclone config
```

Then:

1. Choose `n` for **New Remote**.
    
2. Give it a **name** (e.g., `gdrive1`, `dropbox1`, `onedrive1`).
    
3. Choose the **storage type** (`drive`, `dropbox`, `onedrive`, etc.).
    
4. Follow the instructions to authorize access — rclone will open a browser window to log in.
    

Repeat this for every cloud account you want to add.

> Tip: Use short names like `g1`, `g2`, `o1`, etc., so it’s easier to type later.

---

## Step 3: Check Your Connected Remotes

Once connected, you can verify them with:

```bash
rclone listremotes
```

Example output:

```bash
gdrive1:
gdrive2:
dropbox1:
onedrive1:
mega1:
```

Each line represents one linked cloud storage account.

---

## Step 4: Understanding rclone Union

Now comes the interesting part.

A **union remote** is like a “meta drive” that merges multiple remotes into one.  
You can read from all of them at once, and rclone can automatically decide where to upload new files (for example, to the drive with the most free space).

Think of it as stitching multiple clouds together so they act like one.

You can include remotes from **different providers** — Google Drive, Dropbox, OneDrive, Mega — all in the same union.

---

## Step 5: Create the Union Remote

Open rclone config again:

```bash
rclone config
```

Choose:

1. `n` → New remote
    
2. Name it (e.g., `union1`)
    
3. Set **storage type** → `union`
    
4. When asked for *upstreams*, enter your remote names separated by spaces:
    

```bash
gdrive1: gdrive2: dropbox1: onedrive1: mega1:
```

Then configure the policies:

```bash
create_policy = epmfs
action_policy = all
search_policy = ff
```

**What they mean:**

* `create_policy = epmfs`: Uploads to the drive with the most free space.
    
* `action_policy = all`: Deletions and renames happen across all drives.
    
* `search_policy = ff`: Finds the first matching file when reading.
    

Save and exit.  
You’ve just created your own multi-cloud virtual drive.

---

## Step 6: Test Your Union

Check the total combined size:

```bash
rclone size union1: --human-readable
```

You should now see the total storage space from all your drives combined.  
Each cloud’s quota still applies individually, but now they act as one seamless unit.

---

## Step 7: Using the Union Like Real Storage

You can now use this combined storage locally or expose it to applications.

### Option A — Mount as a Local Folder

```bash
rclone mount union1: ~/UnifiedCloud --vfs-cache-mode full
```

This makes the union appear as a normal folder where you can drag and drop files.

### Option B — Serve as an S3 Bucket

For apps like **Nextcloud**, **Immich**, or **Jellyfin**, you can serve the union as an S3-compatible endpoint:

```bash
rclone serve s3 union1: --addr :9000 --auth-key ACCESS_KEY,SECRET_KEY --vfs-cache-mode full
```

Now your apps can access it like a single cloud bucket.

---

## Step 8: Expanding Later

You can always add new remotes to the same union as your space fills up.

Either run:

```bash
rclone config
```

and edit the union remote,  
or open the configuration file directly (`rclone.conf`) and update the line:

```bash
upstreams = gdrive1: gdrive2: onedrive1: newdropbox:
```

Save it, and your new drive is part of the pool.

---

## Step 9: Important Notes

* The union only works **while rclone is running** — it’s not hosted in the cloud itself.
    
* If you’re using it on a laptop, the system must stay **powered on and online** for access.
    
* For 24/7 use (e.g., a media or photo server), host it on a **VPS** or **home server**.
    
* Each cloud still enforces its own **15 GB or provider limit**, but the union makes managing them effortless.
    

---

## Step 10: The Bigger Picture

Remember *Interstellar* — when Cooper talks about looking up at the stars?  
Now you’ll look up at your connected drives, realizing you’ve just created a virtual galaxy of storage.

It’s not about cheating the system — it’s about using technology smartly.  
rclone union is open-source magic that makes cloud limits feel irrelevant.

---

## Conclusion

You’ve just learned how to:

* Connect multiple cloud accounts from different providers
    
* Combine them into one virtual storage drive
    
* Expand your space anytime by simply adding more remotes
    

Whether you’re a developer, photographer, or digital hoarder, this setup gives you **scalable, free storage** powered by your own creativity.

The next time someone says, “I ran out of space,”  
you can just smile and say:  
**“There’s always room in the union.”**

---

## TL;DR

1. Install rclone
    
2. Connect multiple cloud accounts as remotes
    
3. Create a union remote combining them
    
4. Mount or serve the union as one big storage
    
5. Add new remotes anytime for more space