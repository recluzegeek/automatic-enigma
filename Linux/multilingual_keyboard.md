# Multilingual Keyboard Input

Used `ibus` on Arch Sway, but honestly felt wasted the whole day.

- `ibus` worked in just `firefox` and `vscode`, while remain ineffective on the system.
- Started crawling the web and finally found the right tool for the job.

## `fcitx5`

- From the Arch Documentation, its a successor of `fcitx` and supports multilingual keyboards.
- Install it by `sudo pacman -S fcitx5-m17n fcitx5-im`
- Edit the `/etc/environment` and add the following lines.

    ```shell
    GTK_IM_MODULE=fcitx
    QT_IM_MODULE=fcitx
    XMODIFIERS=@im=fcitx
    ```

- Launch `fcitx-daemon` as a background process on startup by editing the `~/.config/sway/config.d/autostart_applications` and add the following lines`exec --no-startup-id fcitx5 -d`

- Restart the system and configure the applet from the `waybar`

[Click to watch Youtube Saviour](https://www.youtube.com/watch?v=GejGVDfuJQo)