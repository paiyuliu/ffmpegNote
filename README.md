# ffmpegNote
紀錄ffmpeg用法的repo

# ffmpeg course example

## firstly, set up the ffmpeg path please

```powershell
$env:Path += ";D:\u09889\Download\ffmpeg-2024-09-09-git-9556379943-full_build\ffmpeg-2024-09-09-git-9556379943-full_build\bin"
$env:Path += ";D:\u09889\Download\ffmpeg-master-latest-win64-gpl\bin"
```

## ffprob

```powershell

ffprobe -v error seagull.mp4 -show_format -show_streams -print_format json
# show stream only
ffprobe -v error seagull.mp4 -show_format -show_streams  -select_streams v -show_entries stream=codec_name
```

## ffplay

```powershell
ffplay -v error bullfinch.mp4

ffplay -v error bullfinch.mp4 -x 600 -y 600 -noborder
ffplay -v error bullfinch.mp4 -y 600 -noborder
ffplay -v error bullfinch.mp4 -y 600 -noborder -top 0 -left 0
ffplay -v error bullfinch.mp4 -y 600 -top 10 -left 10
ffplay -v error bullfinch.mp4 -y 600 -top 10 -left 10 -fs -an
ffplay -v error bullfinch.mp4 -y 600 -top 10 -left 10 -vn
ffplay -v error bullfinch.mp4 -y 600 -top 10 -left 10 -showmode waves
ffplay -v error bullfinch.mp4 -y 600 -top 10 -left 10 -loop 0
```

## when ffplay , We can click space to pause/resume, click "M" to mute/unmute.
## click "F" to full screen or double click to full screen
## click "0"or "*" to volume up, click "9"or "/" to volume down
## click "W" to cycle modes
## click "S" frame step
## click "←" back 10s clikc "→" forward 10s
## click "↓" back 1 min, click "↑" forward 1 min
## Esc quit

```powershell
ffplay -v error birds-forest.ogg -showmodes waves
ffplay -v error kingfisher.jpg
ffplay url(http url media)
```

```powershell
ffmpeg -f mov -i file:input.mov  ... /tmp/output.mp4

ffmpeg -i sub/dir/input.mp4 ../relative/path/output.mp4

ffmpeg -i input.mp4 -f mxf output-without-extension

ffmpeg -i https://some-server/media.mp4 ... disk-file.mp4

ffmpeg -i rtp://localhost:1234 ... ftp:/abc.com/123/out.mxf


```

```
ffmpeg -v error -y -i multitrack.mp4 -to 1 multitrack-ls.mp4

ffprobe multitrack-ls.mp4 -v error -show_format -show_streams -print_format json

ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0 multitrack-ls.mp4
ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:v multitrack-ls.mp4
ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:0 multitrack-ls.mp4
ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:1 multitrack-ls.mp4
ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:a multitrack-ls.mp4
ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:a:0 multitrack-ls.mp4
ffmpeg -v error -y -i multitrack.mp4 -i second-input.mp4 -to 1 -map 1:v:0 -map 0:a:1 multitrack-ls.mp4
```

### ffmpeg screen capture


```powershell
rem windows 
ffmpeg -f gdigrab -i desktop ...
ffmpeg -f gdigrab -framerate 30 -offset_x 100 -offset_y 100 -video_size 640x480 -show_region 1 -i desktop output.mp4
ffmpeg -f dshow -i audio="Realtek(R) Audio" output.wav
ffmpeg -f dshow -i audio="Microphone(Realtek(R) Audio)" output.wav

ffmpeg -f gdigrab -framerate 30 -offset_x 100 -offset_y 100 -video_size 640x480 -show_region 1 -i desktop -f dshow -i audio="Microphone (Realtek High Definition Audio)" output.mp4
ffmpeg -list_devices true -f dshow -i dummy



:: macos
ffmpeg -f avfoundation -i "1" ...

:: linux
ffmpeg -f x11grab -i $DISPLAY

```

### webcam capture

```
webcam capture

windows ffmpeg -f dshow -i video="USB2.0 HD UVC WebCam" ...

macOS ffmpeg -f avfoundation -i "default" ...

Linux ffmpeg -f v412 -i /dev/video0 ...
```

### Microphone capture

```
windows ffmpeg -f dshow -i audio "Microsoft (Realtek)" ...

macos ffmpeg -f avfoundation -i "2" ...

linux ffmpeg -f alsa -i hw:1 ...
```

### ffmpeg filter

```powershell

ffmpeg -v error -y -i bullfinch.mp4 -vf "split[bg][ol];[bg]scale=width=1920:height=1080,format=gray[bg_out];[ol]scale=-1:480,hflip[ol_out];[bg_out][ol_out]overlay=x=w-w:y=(H-h)/2" ol.mp4
ffmpeg -v error -y -i bullfinch.mp4 -vf "split[bg][ol];[bg]scale=width=1920:height=1080,format=gray[bg_out];[ol]scale=-1:480,hflip[ol_out];[bg_out][ol_out]overlay=x=w-w:y=(H-h)/2" ol2.mp4

ffmpeg -v error -y -i bullfinch.mp4 -i ffmpeg-logo.png -filter_complex "[1:v]scale=-1:200[small_logo];[0:v][small_logo]overlay=x=W-w-50:y=H-h-50,split=2[sd_in][hd_in];[sd_in]scale=-2:480[sd];[hd_in]scale=-2:1080[hd];[0:a]pan=stereo|FL=c0+c2|FR=c1+c3[stereo_mix]" -map [sd] sd.mp4 -map [hd] hd.mp4 -map [stereo_mix] stereo_mix.mp3
ffmpeg -v error -y -i bullfinch.mp4 -i ffmpeg-logo.png -filter_complex "[1:v]scale=-1:200[small_logo];[0:v][small_logo]overlay=x=W-w-50:y=H-h-50,split=2[sd_in][hd_in];[sd_in]scale=-2:480[sd];[hd_in]scale=-2:1080[hd];[0:a]pan=stereo|FL=c0+c2|FR=c1+c3[stereo_mix]" -map [sd] sd.mp4 -map [hd] hd.mp4 -map [stereo_mix] stereo_mix.mp3
```

## encodec

```powershell
ffprobe -v error bullfinch.mov -select_streams v -show_entries stream=codec_name -print_format default=noprint_wrappers=1
ffmpeg -encoders
ffmpeg -v error -y -i bullfinch.mov transcoded.mxf
ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 transcoded.mxf
ffmpeg -v error -y -i bullfinch.mov -vcodec libvpx-vp9 transcoded.mp4
ffmpeg -v error -y -i bullfinch.mov -vcodec libvpx-vp9 -acodec libmp3lame transcoded.mp4

ffmpeg -encoders | grep "mp3"
ffmpeg -encoders | Select-String -Pattern "mp3"
```

## h264/avc

### choose a crf bitrate

### crf 越高，影片畫質越不好

```
ffprobe -v error bullfinch.mov -select_streams v -show_entries stream=codec_name,bit_rate -print_format default=noprint_wrappers=1
ffplay -v error -an transcoded.mp4
ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -crf 10 transcoded.mp4
ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -crf 10 transcoded.mp4
ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -b:v 2M transcoded.mp4
ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -b:v 2M -pass 1 -f null /dev/null
ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -b:v 2M -pass 2 transcoded.mp4
```

## speed

```powershell
Measure-Command { ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 transcoded.mp4 }

time ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 transcoded.mp4
time ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -preset ultrafast transcoded.mp4

### ffmpeg presets

```powershell
ffmpeg -h encoder=libx264
```
#### This command will display a list of available presets for the `libx264` encoder, which you can choose from. Common presets include:

    - ultrafast (最不會壓縮)
    - superfast
    - veryfast
    - faster
    - fast
    - medium (default)
    - slow
    - slower
    - veryslow
    - placebo (最會壓縮)


## manupi video


### trim

```
rem 
ffmpeg -y -v error -i nature.mp4 --ss 00:03:55.000 -to 240.0 squirre1.mp4

ffmpeg -y -v error -i nature.mp4 -ss 00:03:55.000 -to 00:04:00.000 squirre2.mp4

ffmpeg -y -v error -i nature.mp4 --ss 00:03:55.000 -t 5 squirre3.mp4
```

### merge

#### list.txt

```
file 'bullfinch.mp4'
file 'seagull.mp4'
file 'squirrel.mp4'
```

```
ffmpeg -y -v error -f concat -i list.txt merged.mp4
```

### generating thumbnails

```
ffmpeg -v error -i bullfinch.mp4 -vframes 1 bullfinch-poster-frame.jpg
ffmpeg -v error -i bullfinch.mp4 -vframes 1 -vf scale=320:180 bullfinch-thumbnail.jpg
ffmpeg -v error -i bullfinch.mp4 -ss 00:05.000 -vframes 1 -vf scale=320:180 bullfinch-thumbnail5S.jpg
ffmpeg -v error -i bullfinch.mp4 -vf fps=1,scale=320:180 bullfinch-thumbnail-%02d.jpg
ffprobe bullfinch-poster-frame.jpg -v error -select_streams v -show_entries stream=width,height

```

## scaling

```
ffplay -v error cow_4k.mp4 -an
ffprobe cow_4k.mp4 -v error -select_streams v -show_entries stream=width,height
ffmpeg -v error -y -i cow_4k.mp4 -vf scale=1280:720 cow_720p.mp4
ffmpeg -v error -y -i cow_4k.mp4 -vf scale=640:480 cow_480p.mp4
rem aspect_preserved
ffmpeg -v error -y -i cow_4k.mp4 -vf scale=-1:480 cow_480p.mp4
ffmpeg -v error -y -i cow_4k.mp4 -vf scale=-2:480 cow_480p.mp4
ffmpeg -v error -y -i cow_4k.mp4 -vf scale=640:480:force_original_aspect_ratio=decrease cow_480_aspect_forced.mp4

ffmpeg -v error -y -i cow_4k.mp4 -vf "scale=640:480: force_original_aspect_ratio=decrease,pad=640:480:(ow-iw)/2:(oh-ih)/2" cow_480_aspect_forced_and_padded.mp4
```

## Overlay

```
ffmpeg -v error -y -i bullfinch.mp4 -i ffmpeg-logo.png -filter_complex "overlay" out.mp4 && ffplay out.mp4 -v error -an -top 10 -y 875

ffplay out.mp4 -v error -an -top 10 -y 875

ffmpeg -v error -y -i bullfinch.mp4 -i ffmpeg-logo.png -filter_complex "overlay=x=main_w-overlay_w-50:y=50" out.mp4

ffmpeg -v error -y -i bullfinch.mp4 -i ffmpeg-logo.png -filter_complex "[1:v]colorchannelmixer=aa=0.4[transparent_logo];[0:v][transparent_logo]overlay=x=main_w-overlay_w-50:y=50" out.mp4

ffmpeg -v error -y -i bullfinch.mp4 -i ffmpeg-logo.png -filter_complex "[1:v]scale=-1:100[smaller_logo];[0:v][smaller_logo]overlay=x=main_w-overlay_w-50:y=50" out.mp4


ffmpeg -v error -y -i bullfinch.mp4 -i ffmpeg-logo.png -i tux.png -filter_complex "[1:v]scale=-1:100[smaller_logo];[0:v][smaller_logo]overlay=x=main_w-overlay_w-50:y=50[after_one_logo];[after_one_logo][2:v]overlay=x=main_w-overlay_w-50:y=H-h-50" out.mp4

ffmpeg -v error -y -i bullfinch.mp4 -i ffmpeg-logo.png -i squirrel.mp4 -filter_complex "[1:v]scale=-1:100[smaller_logo];[0:v][smaller_logo]overlay=x=main_w-overlay_w-50:y=50[after_one_logo];[2:v]scale=-1:400[smaller_squirrel];[after_one_logo][smaller_squirrel]overlay=W-w-50:H-h-50" out.mp4
```

## drawtext

```
ffplay bullfinch.mp4 -v error -an -top 240 -y 835 -vf "drawtext=fontfile='c\:/Windows/Fonts/courbd.ttf':text=Birds:fontsize=48:x=100:y=100"
```
