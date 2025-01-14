# Audio Fix for Xiaomi RedmiBook 16 on Arch Linux

## Description
This guide provides detailed steps to fix audio issues on the Xiaomi RedmiBook 16 running Arch Linux. It includes updating system packages, configuring GRUB, loading necessary kernel modules, and enabling required services.

## Prerequisites
- Xiaomi RedmiBook 16 Redmibook Pro
- Intel Corporation Meteor Lake-P HD Audio Controller (rev 20)
- Arch Linux (should work in other distros)



## Steps

0. Check the presence of your built in speakers

       lspci | grep -i audio

2. Update System

Update all system packages to the latest versions.


    sudo apt update && sudo apt upgrade   
    sudo pacman -Syu

2. Install/Reinstall Required Packages

Install or reinstall necessary packages for audio.



    sudo pacman -S linux-firmware alsa-utils sof-firmware pipewire pipewire-alsa pipewire-pulse pipewire-jack wireplumber pavucontrol

3. Load Kernel Modules

Load the required kernel modules.


    sudo modprobe snd-hda-intel
    sudo modprobe snd-sof-pci-intel-mtl

4. Configure GRUB

Edit the GRUB configuration file to disable dmic_detect for snd_hda_intel.


    sudo nano /etc/default/grub

Add the following parameters:

    GRUB_CMDLINE_LINUX_DEFAULT="snd_hda_intel.dmic_detect=0"

Update the GRUB configuration:



    sudo grub-mkconfig -o /boot/grub/grub.cfg

5. Enable and Start Services

Enable and start the necessary services.


    
    systemctl --user enable wireplumber
    systemctl --user start wireplumber
    systemctl --user restart pipewire pipewire-pulse wireplumber

6. Restore ALSA Settings

Restore ALSA settings.



    sudo alsactl restore

7. Reboot System

Reboot the system to apply all changes.



    sudo reboot

Post-Reboot Checks

    Verify audio functionality with alsamixer and pavucontrol.
    Confirm services are running:

    

    systemctl --user status pipewire pipewire-pulse wireplumber

8. Verify Audio Functionality

Check if audio is working:



    aplay /usr/share/sounds/alsa/Front_Center.wav

9. Troubleshooting

If issues persist, refer to kernel logs for more information:



    sudo dmesg | grep -i audio


References

This guide was developed based on discussions and solutions found in the following GitHub issue:

    Intel SOF Project - GitHub Issue #4880 - Special thanks to the contributors for their valuable insights and solutions.
    https://github.com/thesofproject/linux/issues/4880
    

License

MIT License
