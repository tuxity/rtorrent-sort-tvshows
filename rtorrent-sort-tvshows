#!/usr/bin/php
<?php


$config = array (
  "tvshows_destdir" => "/home/rtorrent/downloads/tvshows-smb",  /* where de shows will be sort */
  "tvshows_ext" => array ("avi", "mkv", "mp4"),                 /* extensions concerning video only */
  "tvshows_regex" => array (                                    /* regex to match, FIFO */
    "/^(.+)[ ._-][Ss]([0-9]+)[Ee]([0-9]+)[^\\/]*$/i",
    "/^(.+)[ ._-][Ss]?([0-9]+)[Ee]?([0-9]{2,}+)[^\\/]*$/i"
  ),
  "tvshows_debug" => FALSE,
  "tvshows_log" => "/home/rtorrent/rtorrent-sort-tvshows.log",          /* where de log file will be written */
);


function putInLog($str)
{
    global $config;

    if ($config["tvshows_debug"] === TRUE)
        file_put_contents($config["tvshows_log"], date("Y-m-d H:i:s")." : ".$str."\n", FILE_APPEND);
}

function createSymlink($path, $action)
{
    global $config;

    $pathInfo = pathinfo($path);
    $file = $pathInfo['basename'];
    $ext = $pathInfo['extension'];

    if (!isset($ext) || (isset($ext) && !in_array($ext, $config["tvshows_ext"])))
    {
        putInLog("No extension found or not an allowed extension");
        return;
    }

    $res = array();
    foreach ($config["tvshows_regex"] as $regex) {
        if (preg_match($regex, $file, $res))
            break;
        else
            putInLog("Regex error on this name: ".$file);
    }

    $name = ucwords(strtolower(preg_replace("/[^a-zA-Z0-9]/", " ", $res[1])));
    $season = ($res[2][0] == 0) ? str_replace("0", "", $res[2]) : $res[2];

    $symlinkDir = $config["tvshows_destdir"]."/".$name."/Season ".$season;

    if ($action == "unlink_show")
    {
        putInLog("Remove link ".$symlinkDir."/".$file);
        unlink($symlinkDir."/".$file);
    }
    else if ($action == "link_show")
    {
        if (!file_exists($symlinkDir))
        {
            putInLog("Create directory ".$symlinkDir);
            mkdir($symlinkDir, 0777, true);
        }

        $symlinkFile = $symlinkDir."/".$file;
        if (!file_exists($symlinkFile))
        {
            putInLog("Create link ".$symlinkFile);
            link($path, $symlinkFile);
        }
    }
}

function scanDirectory($path, $action)
{
    foreach (scandir($path) as $file) {

        if ($file == "." || $file == ".." || $file[0] == ".")
            continue;

        if (is_dir($path."/".$file))
            scanDirectory($path."/".$file, $action);
        else
            createSymlink($path."/".$file, $action);
    }
}

$action = $argv[1];
$filePath = $argv[2];

if (strpos($filePath, "/tvshows/") == false)
{
  putInLog("Not a TV Show");
  return;
}

if (is_dir($filePath))
  scanDirectory($filePath, $action);
else
  createSymlink($filePath, $action);

?>

