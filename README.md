Audio Fix for Xiaomi RedmiBook 16 on Arch Linux
Description

This guide provides detailed steps to fix audio issues on the Xiaomi RedmiBook 16 running Arch Linux. It includes updating system packages, configuring GRUB, loading necessary kernel modules, and enabling required services.
Prerequisites

    Arch Linux installed on Xiaomi RedmiBook 16
    Basic knowledge of Linux command line

Steps
1. Update System

Update all system packages to the latest versions.

bash

sudo pacman -Syu

2. Install/Reinstall Required Packages

Install or reinstall necessary packages for audio.

bash

sudo pacman -S linux-firmware alsa-utils sof-firmware pipewire pipewire-alsa pipewire-pulse pipewire-jack wireplumber pavucontrol

3. Load Kernel Modules

Load the required kernel modules.

bash

sudo modprobe snd-hda-intel
sudo modprobe snd-sof-pci-intel-mtl

4. Configure GRUB

Edit the GRUB configuration file to disable dmic_detect for snd_hda_intel.

bash

sudo nano /etc/default/grub

Change the line:

bash

GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet"

to:

bash

GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet snd_hda_intel.dmic_detect=0"

Update the GRUB configuration:

bash

sudo grub-mkconfig -o /boot/grub/grub.cfg

5. Enable and Start Services

Enable and start the necessary services.

bash

systemctl --user enable wireplumber
systemctl --user start wireplumber
systemctl --user restart pipewire pipewire-pulse wireplumber

6. Restore ALSA Settings

Restore ALSA settings.

bash

sudo alsactl restore

7. Reboot System

Reboot the system to apply all changes.

bash

sudo reboot

Post-Reboot Checks

    Verify audio functionality with alsamixer and pavucontrol.
    Confirm services are running:

    bash

    systemctl --user status pipewire pipewire-pulse wireplumber

8. Verify Audio Functionality

Check if audio is working:

bash

aplay /usr/share/sounds/alsa/Front_Center.wav

9. Troubleshooting

If issues persist, refer to kernel logs for more information:

bash

sudo dmesg | grep -i audio


References

This guide was developed based on discussions and solutions found in the following GitHub issue:

    Intel SOF Project - GitHub Issue #4880 - Special thanks to the contributors for their valuable insights and solutions.
    https://github.com/thesofproject/linux/issues/4880
    

License

MIT License
