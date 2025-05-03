# Adult Media Apps

The apps you want to run so you can manage a library of adult media on your home server

Cause what good are all those videos if they are buried on your server and not easily viewable

## Applications

### Whisparr

The go-to for managing adult media. Make sure to use v3 (make sure that is the version your Docker is pulling ; see `whisparr.yml`).

Whisparr pulls its metadata for movies from TMDB but **only** searches for entries labeled as *both* "movie" and "adult"

This means that it cannot import entries for media that is not a movie, or that TMDB does not label as "adult"

### Radarr

We are loading a second Radarr instance for this docker compose stack to handle the cases where Whisparr is unable to import a movie from TMDB that lacks the lable "adult". There's a surprising number of movies that fall into this situation. Whether they get updated in TMDB to include the "adult" tag or not is up to TMDB.

### Shoko

Shoko specializes in anime library management. Since its using the aniDB (?), this also includes "adult animation" which are frequently in the form of multi-episode series. Thus few, if any, will be import-able from Radarr or Whisparr. Sonarr also cannot handle these because Sonarr instead uses the TVDB as its metadata source and TVDB does not allow listing "adult" content. So if you have a collection of "adult animation" series you will not get any help from Radarr, Sonarr, or Whispar in managing it. Shoko is the next best one on the list. (got suggestions for others? drop a comment in GitHub Issues here).

Unlike Radarr and Whisparr, Shoko does not have the ability to search your indexers and trackers to look for desired media and send it to your download client for automatic download and import. So you will have to go and manually obtain your media yourself. Then configure the import to Shoko. 

Like the other \*arr apps, Shoko supports renaming your media files to match consistent library file naming schemes. However unlike the \*arr apps, it does not currently have support for using the "hardlink" method to rename files (e.g. instead of moving the file to a new location with a new name, or renaming the file in place, you would want to create a hardlink to the file from a different location with a different name, so that your torrent seeds are not broken by moving or renaming the original files).

To deal with this you can simply just create a copy of the original directory that your media are located in and fill the new directory with all hardlinks of the original files. Then allow Shoko to import and rename the hardlinks, instead of your original files. This works perfectly well and can be accomplished easily like this;

```bash
# /mnt/disk1/torrents/seeding is the path to your torrent downloads that are seeding
# /mnt/disk1/Library is the location where you want Shoko to put the final renamed links to your files

cp -al /mnt/disk1/torrents/seeding /mnt/disk1/torrents/seeding-hardlinks

# now make sure /mnt/disk1 is mounted inside your Shoko docker container
# configure Shoko to import FROM /mnt/disk1/torrents/seeding-hardlinks
# and rename TO /mnt/disk1/Library
```

### Stash

Stash is a local alternative to Plex for viewing the media content on your server. 
