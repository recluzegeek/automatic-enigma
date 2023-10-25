# audio & videos

## Downloading audios from youtube

`yt-dlp {youtube_link} -x --audio-format mp3`

## Trimming Audios using ffmpeg

- From any point till end `ffmpeg -i {input_file.mp3} --ss {start:ing:time} -acodec copy {output_file.mp3}`
  
- Trimming between two points `ffmpeg -i {input_file.mp3} --ss {start:ing:time} --to {start:ing:time} -acodec copy {output_file.mp3}`
