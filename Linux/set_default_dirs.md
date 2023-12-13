# Default Directories in Linux

1. Open or create the `user-dirs.dirs` file:

   ```bash
   nano ~/.config/user-dirs.dirs
   ```

2. Edit the file with your preferred paths:

   ```bash
   XDG_DOWNLOAD_DIR="$HOME/my_downloads"
   XDG_DOCUMENTS_DIR="$HOME/my_documents"
   XDG_MUSIC_DIR="$HOME/my_music"
   XDG_VIDEOS_DIR="$HOME/my_videos"
   ```

3. Save and exit.

4. Apply changes:

   ```bash
   xdg-user-dirs-update
   ```
