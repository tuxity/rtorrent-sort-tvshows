#!/usr/bin/env php
<?php

require_once __DIR__ . "/rtorrent-sort-tvshows.conf"

function putInLog($str)
{
    global $config;

    if ($config['tvshows.debug'] === TRUE)
        file_put_contents($config['tvshows.log'], date("Y-m-d H:i:s") . " : {$str}\n", FILE_APPEND);
}

function createSymlink($path, $action)
{
    global $config;

    $pathInfo = pathinfo($path);
    $file = $pathInfo['basename'];
    $ext = $pathInfo['extension'];

    if (!isset($ext) || (isset($ext) && !in_array($ext, $config["tvshows.ext"])))
    {
        putInLog("No extension found or not an allowed extension");
        return;
    }

    $res = array();
    foreach ($config['tvshows.regex'] as $regex) {
        if (preg_match($regex, $file, $res))
            break;
        else
            putInLog("Regex error on this file: {$file}");
    }

    $name = ucwords(preg_replace("/[^a-zA-Z0-9]/", " ", $res[1]));
    $season = ($res[2][0] == 0) ? str_replace("0", "", $res[2]) : $res[2];

    $symlinkDir = "{$config['tvshows.dest']}/{$name}/Season {$season}";

    if ($action == "unlink_show")
    {
        putInLog("Remove link {$symlinkDir}/{$file}");
        unlink("{$symlinkDir}/{$file}");
    }
    else if ($action == "link_show")
    {
        if (!file_exists($symlinkDir))
        {
            putInLog("Create directory {$symlinkDir}");
            mkdir($symlinkDir, 0777, true);
        }

        $symlinkFile = "{$symlinkDir}/{$file}";
        if (!file_exists($symlinkFile))
        {
            putInLog("Create link {$symlinkFile}");
            symlink($path, $symlinkFile);
        }
    }
}

function scanDirectory($path, $action)
{
    foreach (scandir($path) as $file) {

        if ($file == "." || $file == ".." || $file[0] == ".")
            continue;

        if (is_dir("{$path}/{$file}"))
            scanDirectory("{$path}/{$file}", $action);
        else
            createSymlink("{$path}/{$file}", $action);
    }
}

$action = $argv[1];
$filePath = $argv[2];

if (strpos($filePath, $config['tvshows.src']) == false)
{
  putInLog("This file isn't a TV Show or the show is in the wrong directory");
  return;
}

if (is_dir($filePath))
  scanDirectory($filePath, $action);
else
  createSymlink($filePath, $action);

?>

