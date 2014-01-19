This script will create a directory structure for TV Shows to match the [directory naming convention of XBMC](http://wiki.xbmc.org/index.php?title=Video_library/Naming_files/TV_shows#Directory_structure_and_file_names) and get the scraper TVDB (enabled by default) working.

## Installation

1. Clone the repository (For example in /home/rtorrent)
2. Edit .rtorrent.rc and add these lines at the end :

        system.method.set_key = event.download.finished,rtorrent-sort-tvshows,"execute=/home/rtorrent/rtorrent-sort-tvshows.php,link_show,$d.get_base_path="
        system.method.set_key = event.download.erased,rtorrent-sort-tvshows,"execute=/home/rtorrent/rtorrent-sort-tvshows.php,unlink_show,$d.get_base_path="

3. Copy torrent-sort-tvshow.conf.dist to torrent-sort-tvshow.conf
4. Edit $config variable in rtorrent-sort-tvshow.conf file to set correct paths
5. Make the script executable :

        chmod +x rtorrent-sort-tvshow.php

6. Restart rtorrent
