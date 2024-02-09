# OBS Studio Virtual Camera Guidelines

## Installation

1. Install OBS Studio using `pacman` package manager:

   ```bash
   sudo pacman -S obs-studio
   ```

2. If the Virtual Camera button is not showing up in the menu below "Start Recording," reinstall the `v4l2loopback-dkms` package.

   ```bash
   sudo pacman -S v4l2loopback-dkms
   ```

## Troubleshooting Virtual Camera Error

If you encounter an error message when trying to start the Virtual Camera output, follow these steps:

1. **Reboot System**: Reboot your system after reinstalling the `v4l2loopback-dkms` package.

2. **Enable Virtual Camera**: After rebooting, open OBS Studio and click on the Virtual Camera button.

3. **Error Message**: If you receive an error message stating "Starting the output failed. Please check the log for details," follow the next step.

4. **Run Command**: Open a terminal and run the following command:

   ```bash
   sudo modprobe v4l2loopback exclusive_caps=1 card_label='OBS Virtual Camera'
   ```

   This command loads the `v4l2loopback` kernel module with specific parameters required for OBS Studio's Virtual Camera.
