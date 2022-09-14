# arch-install-docs
### In my installation of Arch, with Kernel 5.15.67-1-lts, I disable my NVIDIA RTX 3080 drivers and force Arch to use only my Radeon RX 5500, for VM passthrough purposes.



## My X11 Config
- /etc/X11/xorg.conf.d/90-monitor.conf | Specify monitor in X11.
```
    Section "Monitor"
        Identifier "<default monitor>"
        DisplaySize 310 131 # In millimeters
    EndSection
    
    # To calculate physical screen width & height.
    # I'm using a 2560x1080 monitor, replace that with whatever you have.
    # Using the pythagorean theorem:
    #    echo 'scale=5;sqrt(2560^2+1080^2)' | bc (install bc through pacman), should output 2778.)
    #    echo 'scale=5;sqrt(13.3/2778)*2560*25.4)' | bc (should output 4495.)
    #    echo 'scale=5;sqrt(13.3/2778)*1080*25.4)' | bc (should output 1896.)
```

- /etc/X11/xorg.conf.d/20-radeon.conf | Force Arch to use amdgpu drivers only.
```
    Section "Device"
        Identifier "Device0"
        Driver "amdgpu"
        BusID "PCI:5:0:0"
    EndSection
    
    # Determine BusID:
    #   lspci -nn
        
    #   BusID for my Radeon RX 5500 is 05:00.0.
```

## Terminal Stuff
```
pacman -S virt-manager qemu libvirt edk2-ovmf dnsmasq iptables-nft

systemctl enable libvirtd.service
systemctl start libvirtd.service # one time only

virsh net-autostart default
virsh net-start default # one time only

```

