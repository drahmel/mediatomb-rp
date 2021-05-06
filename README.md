# mediatomb-rp
Mediatomb DLNA server running on Raspberry Pi

This is my attempt to get MT running on the Raspeberry Pi. I've been running MT for years on Arch Linux for my video and audio streaming needs, but my box finally died and I'd like to run it on a more current platform. MediaTomb still seems better than the alternatives that I've found on RP since I don't need Plex.


It doesn't work yet. I'll clean up the readme as I get things working.



# Steps for debugging

These are the steps I'm following to try to get things working.

```
automake
./configure
```

# Installs

apt install sqlite3
apt install libsqlite3-dev
apt install libsqlite3
apt install expat
apt install ffmpeg

## First successful configure run

CONFIGURATION SUMMARY ----

sqlite3               : yes
mysql                 : missing
libjs                 : missing
libmagic              : missing
inotify               : yes
libexif               : missing
id3lib                : disabled
taglib                : missing
libmp4v2              : missing
ffmpeg                : missing
ffmpegthumbnailer     : missing
lastfmlib             : missing
external transcoding  : yes
curl                  : missing
YouTube               : missing
libextractor          : disabled
db-autocreate         : yes

## First make error

../src/autoscan_inotify.cc:229:48:   required from here
../src/hash/dbo_hash.h:165:28: error: ‘search’ was not declared in this scope, and no declarations were found by argument-dependent lookup at the point of instantiation [-fpermissive]
         bool found = search(key, &slot);




