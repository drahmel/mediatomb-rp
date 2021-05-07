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

`./configure`

CONFIGURATION SUMMARY ----

sqlite3               : yes
mysql                 : missing
libjs                 : missing

libmagic              : missing
    apt-get install -y libmagic-dev
inotify               : yes
    libexif               : missing
    apt-get install -y libexif-dev
id3lib                : disabled

taglib                : missing
    apt-get install -y libtag-extras-dev

libmp4v2              : missing

ffmpeg                : missing
ffmpegthumbnailer     : missing
    apt install ffmpegthumbnailer libffmpegthumbnailer-dev libffmpegthumbnailer4v5 libavutil-dev libavformat-dev

lastfmlib             : missing
    apt-get install -y liblastfm-dev
external transcoding  : yes

curl                  : missing -- apt install libcurl3
YouTube               : missing
    libcurl4-gnutls-dev
libextractor          : disabled
db-autocreate         : yes
libmp4v2              : missing
    libmp4v2-dev
mysql                 : missing
    apt-get install libmariadbclient-dev


-------------------- Not working
id3lib                : disabled
    apt-get install -y libid3-3.8.3-dev libid3-tools
libjs                 : missing
     apt install libmozjs-24-dev
     apt install -y libmozjs-24-0 libmozjs-24-bin
     update-alternatives --install /usr/bin/js js /usr/bin/js24 10
lastfmlib             : missing
libextractor          : disabled


## First make error

../src/autoscan_inotify.cc:229:48:   required from here
../src/hash/dbo_hash.h:165:28: error: ‘search’ was not declared in this scope, and no declarations were found by argument-dependent lookup at the point of instantiation [-fpermissive]
         bool found = search(key, &slot);

Forced my way past error with permissive mode:

`make CFLAGS='-fpermissive' CXXFLAGS='-fpermissive'`

Compile completed, but don't know if it works.


## First run

`make install`

Mediatomb successfully executed!

Ran from the command line with simple:

`mediatomb`

It then reported:

MediaTomb UPnP Server version 0.12.1 - http://mediatomb.cc/

===============================================================================
Copyright 2005-2010 Gena Batsyan, Sergey Bostandzhyan, Leonhard Wimmer.
MediaTomb is free software, covered by the GNU General Public License version 2

2021-05-06 14:38:08    INFO: MediaTomb configuration was created in: /root/.mediatomb/config.xml
2021-05-06 14:38:08    INFO: Loading configuration from: /root/.mediatomb/config.xml
2021-05-06 14:38:08    INFO: UUID generated: de6b5004-f6d5-4ab0-aa41-ab4d99ef4c48
2021-05-06 14:38:08    INFO: Checking configuration...
2021-05-06 14:38:08    INFO: Setting filesystem import charset to UTF-8
2021-05-06 14:38:08    INFO: Setting metadata import charset to UTF-8
2021-05-06 14:38:08    INFO: Setting playlist charset to UTF-8
2021-05-06 14:38:08    INFO: Configuration check succeeded.
2021-05-06 14:38:08 WARNING: Sqlite3 database seems to be corrupt or doesn't exist yet.
2021-05-06 14:38:08    INFO: no sqlite3 backup is available or backup is corrupt. automatically creating database...
2021-05-06 14:38:08    INFO: database created successfully.
2021-05-06 14:38:08    INFO: Initialized port: 49152
2021-05-06 14:38:08    INFO: Server bound to: 192.168.50.142
2021-05-06 14:38:09    INFO: MediaTomb Web UI can be reached by following this link:
2021-05-06 14:38:09    INFO: http://192.168.1.142:49152/

Accessed the Web GUI successfully.

Streamed successfully from Android BubblePNP

TODO: Fix compile errors and make sure optional modules compile


## Second compile

Errors when two plugins enabled, so run configure with them disabled:

./configure --disable-ffmpeg --disable-libmp4v2

Compile also stopped by problem in line 662 of tombupnp/upnp/src/genlib/net/http/webserver.c:

g++ -I../src -I../tombupnp/ixml/inc -I../tombupnp/threadutil/inc -I../tombupnp/upnp/inc -I..  -I/usr/include/mysql   -I/usr/include/taglib       -pthread        -fpermissive  -lrt  -lmagic -o mediatomb mediatomb-main.o libmediatomb.a ../tombupnp/build/libtombupnp.a              -lsqlite3 -L/usr/lib/arm-linux-gnueabihf  -lmariadbclient -lpthread -lz -lm -ldl -L/usr/lib/arm-linux-gnueabihf -ltag   -lmagic  -lexif -lz -lrt -pthread    -lexpat     -lcurl -lcurl
../tombupnp/build/libtombupnp.a(libtombupnp_a-webserver.o): In function `get_file_info':
webserver.c:(.text+0x634): undefined reference to `get_content_type'
collect2: error: ld returned 1 exit status



Commented out and compiled:

    // TODO: Fix compile error
    //rc = get_content_type( filename, &info->content_type );




