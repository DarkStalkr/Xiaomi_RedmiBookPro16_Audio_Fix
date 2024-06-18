Audio Fix for Xiaomi RedmiBook 16 on Arch Linux
Description

This guide provides detailed steps to fix audio issues on the Xiaomi RedmiBook 16 running Arch Linux. It includes updating system packages, configuring GRUB, loading necessary kernel modules, and enabling required services.
Prerequisites

    Arch Linux installed on Xiaomi RedmiBook 16
    Basic knowledge of Linux command line

Steps

    Update System:

    bash

sudo pacman -Syu

Install/Reinstall Required Packages:

bash

sudo pacman -S linux-firmware alsa-utils sof-firmware pipewire pipewire-alsa pipewire-pulse pipewire-jack wireplumber pavucontrol

Load Kernel Modules:

bash

sudo modprobe snd-hda-intel
sudo modprobe snd-sof-pci-intel-mtl

Configure GRUB:

    Edit /etc/default/grub to include:

    bash

GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet snd_hda_intel.dmic_detect=0"

Update GRUB:

bash

    sudo grub-mkconfig -o /boot/grub/grub.cfg

Enable and Start Services:

bash

systemctl --user enable wireplumber
systemctl --user start wireplumber
systemctl --user restart pipewire pipewire-pulse wireplumber

Restore ALSA Settings:

bash

sudo alsactl restore

Reboot System:

bash

    sudo reboot

Post-Reboot Checks

    Verify audio functionality with alsamixer and pavucontrol.
    Confirm services are running:

    bash

    systemctl --user status pipewire pipewire-pulse wireplumber

Troubleshooting

    If issues persist, refer to kernel logs:

    bash

    sudo dmesg | grep -i audio

References

    Intel SOF Project - GitHub Issue #4880

License

MIT License
