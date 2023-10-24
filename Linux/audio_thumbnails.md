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

