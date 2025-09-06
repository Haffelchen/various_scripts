# Ffmpeg

> Convert mkv to gif
```bash
ffmpeg -i "input.mkv" -vf "fps=10,scale=320:-1:flags=lanczos" -c:v gif -f gif "output.gif"
```

```bash
ffmpeg -i "input.mkv" -vf "fps=30,scale=1920:-1:flags=lanczos" -c:v gif -f gif "output.gif"
```

---

> Convert mkv to gif and get rid of that pattern
```bash
ffmpeg -i "input.mkv" -vf palettegen palette.png
ffmpeg -i "input.mkv" -i palette.png -lavfi "fps=30,scale=1920:-1:flags=lanczos [x]; [x][1:v] paletteuse=dither=none" "output.gif"
```

---

> Convert mkv to mp4
```bash
ffmpeg -i "input.mkv" -codec copy "output.mp4"
```

---

> Detect silence
```bash
ffmpeg -i "Song.flac" -af silencedetect=noise=-30dB:d=5 -f null - 2>&1 | grep 'silence_'
```

---

> Check tags
```bash
ffprobe -v error -show_entries format_tags -of default=noprint_wrappers=1 "Song.flac"
```

---

> Extract aac from avi
```bash
ffmpeg -i input-video.avi -vn -acodec copy output-audio.aac
```

---

> Increase volume of mp3
```bash
ffmpeg -i input.mp3 -af volume=1.5 output.mp3
```

---

> Convert DTS to AAC
```bash
ffmpeg -i "Movie [DTS] [1080p].mp4" -c:v copy -c:a aac -b:a 256k "Movie [AAC] [1080p].mp4"
```

---

> Check for errors
```bash
ffprobe -v error -show_entries frame=pkt_pts_time -of csv=p=0 "input.mp4" > errors.log
ffmpeg -v error -i "output.mp4" -c copy -f null - 2> errors2.log
```

---

> Get audio channels + languages of a file
```bash
ffprobe -v error -select_streams a -show_entries stream=index,codec_type,channels,channel_layout,codec_name,codec_long_name:stream_tags=language -of default=noprint_wrappers=1 "input.mkv"
```

---

> Cut up to certain timestamp
```bash
ffmpeg -i "input.mkv" -to 01:37:49 -c copy -map 0 "output.mkv"
```

---

> Cut starting from specific timeframe
```bash
ffmpeg -i "input.mp4" -ss 00:05:28 -c copy -avoid_negative_ts 1 "output.mp4"
```

---

> Extract single frame within timeframe close to keyframe
```bash
ffmpeg -ss 00:02:07 -to 00:02:10 -i "input.mp4" -vf "select='eq(pict_type,PICT_TYPE_I)'" -vsync vfr thumbnail_%03d.png
```

---

> Extract single frame at specific timestamp
```bash
ffmpeg -ss 00:00:03 -i "input.mp4" -vf "select='eq(pict_type,PICT_TYPE_I)'" -frames:v 1 output.png
```

---
