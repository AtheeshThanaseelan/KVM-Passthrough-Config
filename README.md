# KVM-Passthrough-Config
Configuration files for KVM passthrough on Ubuntu 22.04.3 LTS

Platform: HP EliteDesk 800 G1
Passthrough GPU: Nvidia GT 620 OEM (Fermi)
CPU: i7 4770

## Domain XML File
- This file was used to pass the GPU into a Windows 10 Guest. The i440FX chipset was used, as the Q35 chipset caused a host lockup when rebooting the guest. (virt-manager 4.0.0 seems to have a bug where switching to the i440FX chipset doesn't remove the PCI-e components. Change the pcie controller to pci, and delete the pcie slots.

- The SPICE components had to be removed, as the card would not start with those components. This may be board specific, as passing a different board's ROM allowed the GPU to boot with the SPICE components, albeit with graphical corruption. 

- A patched VBIOS file (patched using https://github.com/Matoking/NVIDIA-vBIOS-VFIO-Patcher/) was passed to the card. The vbios file had to be placed in /usr/share/vgabios to bypass AppArmor file access restrictions. This may or may not be mandatory depending on the GPU board.

## Boot Parameters
"intel_iommu=on iommu=pt vfio-pci.ids=10de:1049,10de:0e08"

These parameters are specific to Intel CPUs, as well as the PCI IDs of the graphics card and its HDMI controller.



## Guides Used:
https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF
https://askubuntu.com/questions/1406888/ubuntu-22-04-gpu-passthrough-qemu
