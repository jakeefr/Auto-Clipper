# Auto YouTube Playlist Downloader and Clipper

This tool automates the process of downloading all videos from a YouTube playlist, saving them as `.mp4`, organizing each into its own folder, and splitting them into 15-second video clips using FFmpeg. (Use any drive for installation; I use D:)

## 🔧 What It Uses

- [yt-dlp](https://github.com/yt-dlp/yt-dlp): For downloading YouTube videos.
- [FFmpeg](https://ffmpeg.org/): For converting and slicing videos into 15-second segments.
- PowerShell: For scripting and automation.

---

## 📥 Installation Instructions

1. **Download yt-dlp:**
   - Visit: https://github.com/yt-dlp/yt-dlp/releases
   - Download `yt-dlp.exe`
   - Place it in: `D:\ytdlp\yt-dlp.exe`

2. **Download FFmpeg:**
   - Visit: https://www.gyan.dev/ffmpeg/builds/
   - Download `ffmpeg-*-essentials_build.zip`
   - Extract the folder and place it in: `D:\ffmpeg\ffmpeg-*-essentials_build`
   - Confirm executable is at: `D:\ffmpeg\ffmpeg-*-essentials_build\bin\ffmpeg.exe`

3. **Create a working folder:**
   - Example: `D:\ytdlp`

---

## ▶️ How To Use

1. **Open PowerShell as Administrator**
2. **Run this script:**

```
$ytDlp = "D:\ytdlp\yt-dlp.exe"
$ffmpeg = "D:\ffmpeg\ffmpeg-7.1.1-essentials_build\bin\ffmpeg.exe"
$playlist = "https://www.youtube.com/playlist?list=xxxxxxxxxxxxxxxxxxxxxxxxx"
$downloadDir = "D:\ytdlp"

& $ytDlp --yes-playlist --format mp4 --output "$downloadDir\video %(playlist_index)03d - %(title).80s.%(ext)s" $playlist

Get-ChildItem -Path $downloadDir -Filter "video *.mp4" | ForEach-Object {
    $file = $_
    $outputFolder = Join-Path $downloadDir "autoclip\$($file.BaseName)"
    New-Item -ItemType Directory -Force -Path $outputFolder | Out-Null

    & $ffmpeg -i $file.FullName -c:v libx264 -c:a aac -map 0 -f segment -segment_time 15 -reset_timestamps 1 "$outputFolder\clip_%03d.mp4"
}

```

---

## ⚠️ Notes
- Do not rename `yt-dlp.exe` or `ffmpeg.exe`.
- Ensure your paths match exactly.
- This script assumes the output folder is `D:\ytdlp\autoclip\`.
- Videos are downloaded to `D:\ytdlp\video 00001.mp4`, `video 00002.mp4`, etc.

---

## ✅ Result

After running, you will have:
- All playlist videos downloaded in `.mp4` format.
- Each video split into 15-second `.mp4` clips.
- Each clip stored in its own folder: `video 00001\clip_000.mp4`, `video 00002\clip_000.mp4`, etc.

---

Created by jakeefr | PowerShell + yt-dlp + FFmpeg Automation
