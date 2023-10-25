# Audio Thumbnails Using CLI

## Scenario

I've had a large audio collection but never thought of using them as I would normally use Youtube or SoundCloud. But now I've decided to organize my Audio Collection.

- To manage the audio metadata, use `easytag` by installing from your respective package manager. On `pacman` use this command, `sudo pacman -Sy easytag`
- I can edit the file metadata easily using `easytag` and now I want to make audio thumbnail using some CLI tool.
  - I hate blank screen or default album cover art and I thought of adding song lyrics to the audio thumbnail.
  - Scrapped the song lyrics to `lyric.txt` and after getting the desired thumbnail, use again `easytag` to manipulate the audio thumbnail.
- After searching and asking, stumbled upon `convert` - a member tool of `magick` suite of tools.
  
  ```shell
  convert -size 1920x1080 -background white -fill black -font Jameel Noori Nastaleeq Regular/Jameel Noori Nastaleeq Regular.ttf -pointsize 25 -gravity center caption:@lyrics.txt canvas.jpg
  ```

Here's what each part of the command does:

- `convert`: This is the command to invoke ImageMagick's `convert` utility, which is used for image manipulation and conversion.

- `-size 1920x1080`: This sets the size of the canvas to 1920 pixels in width and 1080 pixels in height.

- `-background white`: This specifies that the background color of the canvas should be white.

- `-fill black`: This sets the fill color for drawing (text) to black.

- `-font Jameel Noori Nastaleeq Regular/Jameel Noori Nastaleeq Regular.ttf`: This specifies the font file to be used for the text. It appears to be a specific font file located in the directory "Jameel Noori Nastaleeq Regular."

- `-pointsize 25`: This sets the font size to 25 points.

- `-gravity center`: This specifies that the text caption should be placed at the center of the canvas.

- `caption:@lyrics.txt`: This is the text to be drawn on the canvas. The text is read from the file "lyrics.txt."

- `canvas.jpg`: This is the output file where the final image will be saved as a JPEG file with the name "canvas.jpg."

The command essentially takes the text from "lyrics.txt" and creates an image with the specified dimensions, background color, font, font size, and text color, positioning the text at the center of the canvas, and saves the result as "canvas.jpg."

## Organizing Audio Quran

- As part of my audio collection, is Holy Quran and wasn't organized. Surah numbers were the file names and I don't want to edit the metadata of 114 files + generating cover art for each of the file.
  - I decided to create bash script that'll add the metadata such as `file name`, `title`, `album`, `genere`, `artist`, `comment` and most important of all `cover art`.
  - Here's an overview of my directory

  ```bash
  /mnt/win/Music/Al Sudais
  ├── 001.mp3
  ├── 002.mp3
  ├── 003.mp3
  ├── 004.mp3
  ├── 005.mp3
  ├── 006.mp3
  ├── 007.mp3
  ├── 008.mp3
  ├── 009.mp3
  ├── 010.mp3
  ├── 011.mp3
  ├── 012.mp3
  ├── 013.mp3
  ├── 014.mp3
  ├── 015.mp3
  ├── 016.mp3
  ├── 017.mp3
  ├── 018.mp3
  ├── 019.mp3
  ├── 020.mp3
  .
  .
  .
  |── 114.mp3
  ```
  
- I crawled the web and found out [**quran-json**](https://github.com/risan/quran-json). One of the end points I considered was `https://cdn.jsdelivr.net/npm/quran-json@3.1.2/dist/chapters/en/index.json`.
- Bumped the `https://cdn.jsdelivr.net/npm/quran-json@3.1.2/dist/chapters/en/index.json` to `https://hoppscotch.io/` and looked into the response. That's all I was looking for.
  
  ```json
  [
    {
      "id": 1,
      "name": "الفاتحة",
      "transliteration": "Al-Fatihah",
      "translation": "The Opener",
      "type": "meccan",
      "total_verses": 7,
      "link": "https://cdn.jsdelivr.net/npm/quran-json@3.1.2/dist/chapters/en/1.json"
    },
    {
      "id": 2,
      "name": "البقرة",
      "transliteration": "Al-Baqarah",
      "translation": "The Cow",
      "type": "medinan",
      "total_verses": 286,
      "link": "https://cdn.jsdelivr.net/npm/quran-json@3.1.2/dist/chapters/en/2.json"
    }
  ]
  ```

- I'd Surah numbers as file names so I can match the file number with the received `id` and then extract the other information and insert as metadata to the file.
- `jq` - a command line `json` processor is used to work with `json` format in bash

### Code Demo

- In this code, I'm reading the `json` file, extracting the surah number from the local file and comparing it with `id` received with the `json` data.

  ```bash
  #!/bin/bash

  # Path to the directory containing the audio files
  audio_dir="/mnt/win/Music/test/"

  # Read JSON data from the surah_details.json file
  json_data=$(cat surah_details.json)

  # List all MP3 files in the directory
  for audio_file in "$audio_dir"*.mp3; do
      # Extract the numeric part from the file name (only from the beginning)
      numeric_part=$(basename "$audio_file" | grep -o -E '^[0-9]+')

      # Find the corresponding JSON entry with the same "id"
      # use `man jq` for further consultation
      surah_details=$(echo "$json_data" | jq --arg numeric_part "$numeric_part" '.[] | select(.id == ($numeric_part | tonumber))')
  ```

- Inside the `for` loop

  ```bash
  # Check if Surah details were found and print them
  if [ -n "$surah_details" ]; then
      # Extract relevant fields from the JSON data
      id=$(echo "$surah_details" | jq -r '.id')
      name=$(echo "$surah_details" | jq -r '.name')
      transliteration=$(echo "$surah_details" | jq -r '.transliteration')
      translation=$(echo "$surah_details" | jq -r '.translation')
      type=$(echo "$surah_details" | jq -r '.type')
      total_verses=$(echo "$surah_details" | jq -r '.total_verses')
     # Create the formatted string
      file_name="${id} - ${transliteration} - Al Sudais.mp3"
      title="$id - $name - $translation -  Al Sudais"
      details="$type -  $total_verses Verses"
  echo "Formatted File Name: $file_name"
  echo "Title: $title"
  echo "Surah Details: $details"
  echo "------------------------------------------------------------"
  ```

### Generating Cover Art

- This block of code is generating cover art for each of the audio file. I'm using a background image on top of which I wanna add text specific for each audio file. So background image is kind of fill in blank for each local file where I can put file name, details and use it as thumbnail for the audio once its been created.

  ```bash
  convert bg.jpg \
  \( -background none -fill white -font "/usr/share/fonts/TTF/Jameel Noori Nastaleeq Kasheeda.ttf" -pointsize 200 -gravity center label:"﷽" \) -geometry +100-600  -composite  \
  \( -background none -fill "#90f83e" -font "/usr/share/fonts/noto/NotoKufiArabic-Regular.ttf" -pointsize 150 -gravity center label:"سورۃ $name   " \) -geometry +100-200  -composite  \
  \( -background none -fill "#89a4f1" -font "/usr/share/fonts/circular-font-family/ProductSans-BoldItalic.ttf" -pointsize 100 -gravity center label:"$id - $transliteration - $translation - Al Sudais" \)  -geometry +100+100 -composite \
  \( -background none -fill "#89a4f1" -font "/usr/share/fonts/circular-font-family/ProductSans-BoldItalic.ttf" -pointsize 70 -gravity center label:"$details" \)  -geometry +100+300 -composite \
  output_$id.jpg
  ```

1. `convert bg.jpg`:
   - This specifies the background image file, `bg.jpg`, which will serve as the base for the composition.

2. The command uses multiple subcommands, each enclosed in `\( ... \)`:
   - `\( -background none -fill white -font "/usr/share/fonts/TTF/Jameel Noori Nastaleeq Kasheeda.ttf" -pointsize 200 -gravity center label:"﷽" \)`:
     - This subcommand creates a white text element with the Arabic script "﷽" using the "Jameel Noori Nastaleeq Kasheeda" font. It positions this element at the center of the image with a vertical offset of 600 pixels.

   - `\( -background none -fill "#90f83e" -font "/usr/share/fonts/noto/NotoKufiArabic-Regular.ttf" -pointsize 150 -gravity center label:"سورۃ $name   " \)`:
     - This subcommand creates a text element with green color and the text "سورۃ" followed by the value of the `$name` variable. It uses the "NotoKufiArabic-Regular" font and positions it at the center of the image with a vertical offset of 200 pixels.

   - `\( -background none -fill "#89a4f1" -font "/usr/share/fonts/circular-font-family/ProductSans-BoldItalic.ttf" -pointsize 100 -gravity center label:"$id - $transliteration - $translation - Al Sudais" \)`:
     - This subcommand creates a text element with a blue color and displays the values of `$id`, `$transliteration`, `$translation`, and "Al Sudais." It uses the "ProductSans-BoldItalic" font and is positioned at the center of the image with a vertical offset of 100 pixels.

   - `\( -background none -fill "#89a4f1" -font "/usr/share/fonts/circular-font-family/ProductSans-BoldItalic.ttf" -pointsize 70 -gravity center label:"$details" \)`:
     - This subcommand creates a text element in blue that displays the value of `$details`. It uses the same font and positions it at the center of the image with a vertical offset of 300 pixels.

3. `output_$id.jpg`:
   - This specifies the output image file, which will be named based on the value of the `$id` variable.

- The result is an image (`output_$id.jpg`) with multiple text elements composed on the background image (`bg.jpg`) as described in the breakdown.

Here's the outcome of the above command.

![Background Image](imgs/mpv-shot0001.jpg)

and here's the default template.

![Template](imgs/bg.jpg)

### Embedding Audio Thumbnails

- One thing remained is to embed the img into the audio itself along with other extracted metadata from the `json`.

```bash
# It removes the current video / track img from the audio,
# otherwise image will be added but not be embedded or displayed
# So first we need to remove the old one and set the new one
# Extract audio without video and save in the NUT format
ffmpeg -i "$audio_file" -map 0 -map -0:v -c copy -f nut - |

# Read the audio from the previous pipe and apply metadata and thumbnail
ffmpeg -f nut -i - -i "$thumbnail" -map 0 -map 1 -c copy -c:v:1 jpg -disposition:v:1 attached_pic \

# Set audio metadata
-metadata title="$title" -metadata artist="Al Sudais" -metadata genre="Quran" -metadata comment="$details" -metadata album="Mushaf - Al Sudais" -y "$file_name"
```

- `-map 0 -map -0:v -c copy -f nut -`  is used to map and copy the audio stream (and exclude the video stream if present). The `-f nut -` option specifies the output format as NUT, and the hyphen `-` at the end represents standard output (stdout).

  - The pipe `|` is used to send the NUT format audio to the next ffmpeg command as input.

- `ffmpeg -f nut -i - -i "\$thumbnail" -map 0 -map 1 -c copy -c:v:1 jpg -disposition:v:1` reads the audio from stdin (specified as -i -) and takes a thumbnail image from the specified file \$thumbnail using -i "\$thumbnail".

- `-map 0 -map 1 -c copy -c:v:1 jpg -disposition:v:1` maps both the audio and the image. The audio is copied (-c copy), and a new video stream is created in JPG format (`-c:v:1 jpg`). The `disposition:v:1` attached_pic sets the attached picture flag for the video stream, indicating that it's a thumbnail image.

- `-metadata title="\$title" -metadata artist="Al Sudais" -metadata genre="Quran" -metadata comment="\$details" -metadata album="Mushaf - Al Sudais"` sets various metadata fields for the audio, including title, artist, genre, comment, and album.

- Running `ffprobe` on one of the files produces this output.

  ```bash
  └──| ffprobe 25\ -\ Al-Furqan\ -\ Al\ Sudais.mp3 
    ffprobe version n6.0 Copyright (c) 2007-2023 the FFmpeg developers
    built with gcc 13.2.1 (GCC) 20230801
    configuration: --prefix=/usr --disable-debug --disable-static --disable-stripping --enable-amf --enable-avisynth --enable-cuda-llvm --enable-lto --enable-fontconfig --enable-gmp --enable-gnutls --enable-gpl --enable-ladspa --enable-libaom --enable-libass --enable-libbluray --enable-libbs2b --enable-libdav1d --enable-libdrm --enable-libfreetype --enable-libfribidi --enable-libgsm --enable-libiec61883 --enable-libjack --enable-libjxl --enable-libmodplug --enable-libmp3lame --enable-libopencore_amrnb --enable-libopencore_amrwb --enable-libopenjpeg --enable-libopenmpt --enable-libopus --enable-libpulse --enable-librav1e --enable-librsvg --enable-libsoxr --enable-libspeex --enable-libsrt --enable-libssh --enable-libsvtav1 --enable-libtheora --enable-libv4l2 --enable-libvidstab --enable-libvmaf --enable-libvorbis --enable-libvpl --enable-libvpx --enable-libwebp --enable-libx264 --enable-libx265 --enable-libxcb --enable-libxml2 --enable-libxvid --enable-libzimg --enable-nvdec --enable-nvenc --enable-opencl --enable-opengl --enable-shared --enable-version3 --enable-vulkan
    libavutil      58.  2.100 / 58.  2.100
    libavcodec     60.  3.100 / 60.  3.100
    libavformat    60.  3.100 / 60.  3.100
    libavdevice    60.  1.100 / 60.  1.100
    libavfilter     9.  3.100 /  9.  3.100
    libswscale      7.  1.100 /  7.  1.100
    libswresample   4. 10.100 /  4. 10.100
    libpostproc    57.  1.100 / 57.  1.100
  Input #0, mp3, from '25 - Al-Furqan - Al Sudais.mp3':
    Metadata:
      album           : Mushaf - Al Sudais
      title           : 25 - الفرقان - The Criterion -  Al Sudais
      comment         : Meccan -  77 Verses
      genre           : Quran
      artist          : Al Sudais
      date            : 1436
    encoder         : Lavf60.3.100
    Duration: 00:14:37.64, start: 0.011995, bitrate: 351 kb/s
    Stream #0:0: Audio: mp3, 44100 Hz, stereo, fltp, 320 kb/s
      Metadata:
        encoder         : Lavf
    Stream #0:1: Video: mjpeg (Baseline), yuvj444p(pc, bt470bg/unknown/unknown), 4000x2251 [SAR 300:300 DAR 4000:2251], 90k tbr, 90k tbn (attached pic)
      Metadata:
        comment         : Other
  ```

### Final Result

- Changed the metadata of all 114 files, created cover art for each of these files, removed the old cover arts and embeded new ones.
- I learned along the way `jq` and `ffmpeg` and excessive usage of directory navigation commands. Also learnt about amazing tool `fcitx5` which is a multilingual keyboard otherwise it would've taken me ages to sort issues with `ibus`

```bash
.
├── 1 - Al-Fatihah - Al Sudais.mp3
├── 2 - Al-Baqarah - Al Sudais.mp3
├── 3 - Ali 'Imran - Al Sudais.mp3
├── 4 - An-Nisa - Al Sudais.mp3
├── 5 - Al-Ma'idah - Al Sudais.mp3
├── 6 - Al-An'am - Al Sudais.mp3
├── 7 - Al-A'raf - Al Sudais.mp3
├── 8 - Al-Anfal - Al Sudais.mp3
├── 9 - At-Tawbah - Al Sudais.mp3
├── 10 - Yunus - Al Sudais.mp3
├── 11 - Hud - Al Sudais.mp3
├── 12 - Yusuf - Al Sudais.mp3
├── 13 - Ar-Ra'd - Al Sudais.mp3
├── 14 - Ibrahim - Al Sudais.mp3
├── 15 - Al-Hijr - Al Sudais.mp3
├── 16 - An-Nahl - Al Sudais.mp3
├── 17 - Al-Isra - Al Sudais.mp3
├── 18 - Al-Kahf - Al Sudais.mp3
├── 19 - Maryam - Al Sudais.mp3
├── 20 - Taha - Al Sudais.mp3
├── 21 - Al-Anbya - Al Sudais.mp3
├── 22 - Al-Hajj - Al Sudais.mp3
├── 23 - Al-Mu'minun - Al Sudais.mp3
├── 24 - An-Nur - Al Sudais.mp3
├── 25 - Al-Furqan - Al Sudais.mp3
├── 26 - Ash-Shu'ara - Al Sudais.mp3
├── 27 - An-Naml - Al Sudais.mp3
├── 28 - Al-Qasas - Al Sudais.mp3
├── 29 - Al-'Ankabut - Al Sudais.mp3
├── 30 - Ar-Rum - Al Sudais.mp3
├── 31 - Luqman - Al Sudais.mp3
├── 32 - As-Sajdah - Al Sudais.mp3
├── 33 - Al-Ahzab - Al Sudais.mp3
├── 34 - Saba - Al Sudais.mp3
├── 35 - Fatir - Al Sudais.mp3
├── 36 - Ya-Sin - Al Sudais.mp3
├── 37 - As-Saffat - Al Sudais.mp3
├── 38 - Sad - Al Sudais.mp3
├── 39 - Az-Zumar - Al Sudais.mp3
├── 40 - Ghafir - Al Sudais.mp3
├── 41 - Fussilat - Al Sudais.mp3
├── 42 - Ash-Shuraa - Al Sudais.mp3
├── 43 - Az-Zukhruf - Al Sudais.mp3
├── 44 - Ad-Dukhan - Al Sudais.mp3
├── 45 - Al-Jathiyah - Al Sudais.mp3
├── 46 - Al-Ahqaf - Al Sudais.mp3
├── 47 - Muhammad - Al Sudais.mp3
├── 48 - Al-Fath - Al Sudais.mp3
├── 49 - Al-Hujurat - Al Sudais.mp3
├── 50 - Qaf - Al Sudais.mp3
├── 51 - Adh-Dhariyat - Al Sudais.mp3
├── 52 - At-Tur - Al Sudais.mp3
├── 53 - An-Najm - Al Sudais.mp3
├── 54 - Al-Qamar - Al Sudais.mp3
├── 55 - Ar-Rahman - Al Sudais.mp3
├── 56 - Al-Waqi'ah - Al Sudais.mp3
├── 57 - Al-Hadid - Al Sudais.mp3
├── 58 - Al-Mujadila - Al Sudais.mp3
├── 59 - Al-Hashr - Al Sudais.mp3
├── 60 - Al-Mumtahanah - Al Sudais.mp3
├── 61 - As-Saf - Al Sudais.mp3
├── 62 - Al-Jumu'ah - Al Sudais.mp3
├── 63 - Al-Munafiqun - Al Sudais.mp3
├── 64 - At-Taghabun - Al Sudais.mp3
├── 65 - At-Talaq - Al Sudais.mp3
├── 66 - At-Tahrim - Al Sudais.mp3
├── 67 - Al-Mulk - Al Sudais.mp3
├── 68 - Al-Qalam - Al Sudais.mp3
├── 69 - Al-Haqqah - Al Sudais.mp3
├── 70 - Al-Ma'arij - Al Sudais.mp3
├── 71 - Nuh - Al Sudais.mp3
├── 72 - Al-Jinn - Al Sudais.mp3
├── 73 - Al-Muzzammil - Al Sudais.mp3
├── 74 - Al-Muddaththir - Al Sudais.mp3
├── 75 - Al-Qiyamah - Al Sudais.mp3
├── 76 - Al-Insan - Al Sudais.mp3
├── 77 - Al-Mursalat - Al Sudais.mp3
├── 78 - An-Naba - Al Sudais.mp3
├── 79 - An-Nazi'at - Al Sudais.mp3
├── 80 - 'Abasa - Al Sudais.mp3
├── 81 - At-Takwir - Al Sudais.mp3
├── 82 - Al-Infitar - Al Sudais.mp3
├── 83 - Al-Mutaffifin - Al Sudais.mp3
├── 84 - Al-Inshiqaq - Al Sudais.mp3
├── 85 - Al-Buruj - Al Sudais.mp3
├── 86 - At-Tariq - Al Sudais.mp3
├── 87 - Al-A'la - Al Sudais.mp3
├── 88 - Al-Ghashiyah - Al Sudais.mp3
├── 89 - Al-Fajr - Al Sudais.mp3
├── 90 - Al-Balad - Al Sudais.mp3
├── 91 - Ash-Shams - Al Sudais.mp3
├── 92 - Al-Layl - Al Sudais.mp3
├── 93 - Ad-Duhaa - Al Sudais.mp3
├── 94 - Ash-Sharh - Al Sudais.mp3
├── 95 - At-Tin - Al Sudais.mp3
├── 96 - Al-'Alaq - Al Sudais.mp3
├── 97 - Al-Qadr - Al Sudais.mp3
├── 98 - Al-Bayyinah - Al Sudais.mp3
├── 99 - Az-Zalzalah - Al Sudais.mp3
├── 100 - Al-'Adiyat - Al Sudais.mp3
├── 101 - Al-Qari'ah - Al Sudais.mp3
├── 102 - At-Takathur - Al Sudais.mp3
├── 103 - Al-'Asr - Al Sudais.mp3
├── 104 - Al-Humazah - Al Sudais.mp3
├── 105 - Al-Fil - Al Sudais.mp3
├── 106 - Quraysh - Al Sudais.mp3
├── 107 - Al-Ma'un - Al Sudais.mp3
├── 108 - Al-Kawthar - Al Sudais.mp3
├── 109 - Al-Kafirun - Al Sudais.mp3
├── 110 - An-Nasr - Al Sudais.mp3
├── 111 - Al-Masad - Al Sudais.mp3
├── 112 - Al-Ikhlas - Al Sudais.mp3
├── 113 - Al-Falaq - Al Sudais.mp3
├── 114 - An-Nas - Al Sudais.mp3
├── nohup.out
├── out.txt
├── rename.sh
└── surah_details.json

1 directory, 118 files
```
