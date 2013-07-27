This script will create a directory structure for a TV Show, to match the [directory naming convention of XBMC](http://wiki.xbmc.org/index.php?title=Video_library/Naming_files/TV_shows#Directory_structure_and_file_names) and get the scraper TVDB (enabled by default) working.

## Installation
###In .rtorrent.rc
    system.method.set_key = event.download.finished,update_symlink,"execute=/home/rtorrent/rtorrent-sort-tvshows.php,link_show,$d.get_base_path="
    system.method.set_key = event.download.erased,update_symlink,"execute=/home/rtorrent/rtorrent-sort-tvshows.php,unlink_show,$d.get_base_path="

(You need to change the path of the execute if needed)

###In ~/rtorrent-sort-tvshows.php
    The script below 
    Don't forget to chmod +x him, and change conf variable if needed 

