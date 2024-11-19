# ffmpeg note

## Organized ffmpeg course markdown file

This markdown file provides an excellent overview of ffmpeg commands with clear explanations and examples. Here's a slightly re-arranged version for better readability:

**# ffmpeg Course Examples**

**## Setting Up ffmpeg Path**

These commands add the ffmpeg installation directory to your system's path environment variable. Make sure to replace the paths with the actual locations on your system.

```powershell
$env:Path += ";D:\u09889\Download\ffmpeg-2024-09-09-git-9556379943-full_build\ffmpeg-2024-09-09-git-9556379943-full_build\bin"
$env:Path += ";D:\u09889\Download\ffmpeg-master-latest-win64-gpl\bin"
```

**## ffprobe**

ffprobe is a tool for analyzing media files. Here are some examples:

* **Get basic information:**

```powershell
ffprobe -v error seagull.mp4 -show_format -show_streams -print_format json
```

* **Show only video stream codec:**

```powershell
ffprobe -v error seagull.mp4 -show_format -show_streams -select_streams v -show_entries stream=codec_name
```

**## ffplay**

ffplay is a simple media player for various formats. Here are some options you can use:

* Play a video:

```powershell
ffplay -v error bullfinch.mp4
```

* Set window size and remove borders:

```powershell
ffplay -v error bullfinch.mp4 -x 600 -y 600 -noborder
```

* Control playback position:

```powershell
* Space - Pause/Resume
* M - Mute/Unmute
* F - Fullscreen
* Double-click - Fullscreen
* 0 or * - Volume up
* 9 or / - Volume down
* W - Cycle playback modes
* S - Step forward one frame
* ← - Back 10 seconds
* → - Forward 10 seconds
* ↓ - Back 1 minute
* ↑ - Forward 1 minute
* Esc - Quit
```

* Play other media types and URLs:

```powershell
ffplay -v error birds-forest.ogg -showmodes waves
ffplay -v error kingfisher.jpg
ffplay url(http url media)
```

**## Basic File Operations**

ffmpeg can be used to convert, copy, and manipulate media files. Here are some examples:

* **Convert file format:**

```powershell
ffmpeg -f mov -i file:input.mov ... /tmp/output.mp4
```

* **Specify relative or absolute output path:**

```powershell
ffmpeg -i sub/dir/input.mp4 ../relative/path/output.mp4

# Without extension for format detection
ffmpeg -i input.mp4 -f mxf output-without-extension
```

* **Process online media:**

```powershell
ffmpeg -i https://some-server/media.mp4 ... disk-file.mp4

# RTP and remote file transfer protocol (MFTP)
ffmpeg -i rtp://localhost:1234 ... ftp:/abc.com/123/out.mxf
```

**## Selecting Streams**

You can choose specific streams (audio/video) within a multi-track file for processing. The `-map` option is used for this purpose.

```powershell
ffmpeg -v error -y -i multitrack.mp4 -to 1 multitrack-ls.mp4  # Select first second of the entire file

# Select video stream only
ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:v multitrack-ls.mp4

# Select specific video or audio stream by index
ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:0 multitrack-ls.mp4  # First video stream
ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:1 multitrack-ls.mp4  # First audio stream
ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:a multitrack-ls.mp4  # All audio streams

# Select specific audio stream within a track (if multiple exist)
ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0
```
