Arch ISO: [archlinux-2026.10.01-x86_64.iso](https://mirrors.mit.edu/archlinux/iso/2026.10.01/archlinux-2026.10.01-x86_64.iso)

created a new VM and made the disk size 20GB and gave it the downloaded ISO as well as selected Other Linux 6.x kernel 64-bit
1. Error: First mistake was i forgot to give the VM enough memory, took a while to figure out why the kernel wasn't booting.
OBJECTIVE: Connect to network commands:
2. command: ip link set ens33 up
3. command: systemctl restart systemd-networkd
4. command: sleep 10
5. command: ip addr 
6. Error: originally had my network set to bridged and had to switch back to NAT 
OBJECTIVE: update system clock
7. command: timedatectl set-timezone America/Chicago
OBJECTIVE: Partition and format disks
8. command: lsblk
9. command: fdisk /dev/sda
10. commands: o, p, n , enter x3, a, p, w.
11. command: mkfs.ext4 /dev/sda1
12. mount /dev/sda1 /mnt
13. lsblk
14. error: accidentally created a gpt table first try
OBJECTIVE:  install essential packages
15. pacstrap -K /mnt base linux linux-firmware
Configure the system
16. genfstab -U /mnt >> /mnt/etc/fstab
17. arch-chroot /mnt
18. ln -sf /usr/share/zoneinfo/_Area_/_Location_ /etc/localtime
19. hwclock --systohc
20. locale-gen
21. nano /etc/locale.gen, delete the # before en.US.UTF-8 UTF-8
22. echo "VicsArchVM" > /etc/hostname
23. passwd - create new password
24. error: pacstrap installation never completed and i never ran arch-chroot /mnt to enter the installed system so i have to go back, the reason pacstrap never installed was because the connection was too slow (running on a hotspot not by choice.) having to disable the slow download timeout with command: sed -i '/^\[options\]/a DisableDownloadTimeout' /etc/pacman.conf
25. install successful
26. run all previous commands
27. grub-install --target=i386-pc /dev/sda
28. grub-mkconfig -o /boot/grub/grub.cfg
29. exit
30. umount -R /mnt
31. reboot
POST-INSTALLATION
INSTALL DE
32. pacman -Syu
33. pacman -S xorg-server lxqt sddm
34. systemctl enable sddm
35. systemctl set-default graphical.target
CREATE USERS
36. useradd -m -G wheel -s /bin/bash victor
37. passwd victor
38. useradd -m -G wheel -s /bin/bash codi
39. passwd codi
40. EDITOR=nano visudo
41. delete the "#" from %wheel ALL=(ALL:ALL) ALL
Confirm sudo 
42. su victor
43. sudo whoami
reboot and login as victor

download different shell
1. sudo pacman -S zsh
download ssh 
2. sudo pacman -S openssh
3. sudo systemctl enable --now sshd

Color Coding!!!
5. nano ~/.bashrc
6. write: PS1='\[\e[1;32m\]\u@\h\[\e[0m\] \[\e[1;34m\]\w\[\e[0m\] \$ '
7. alias ls='ls --color=auto'
8. alias grep='grep --color=auto'
9. alias diff='diff --color=auto'
10. alias ip='ip -color=auto'
11. source ~/.bashrc
12. sudo nano /etc/pacman.conf
13. delete the # on the color line
add aliases
14. nano ~/.bashrc
add following lines 
15. alias ll='ls -lah'
16. alias c='clear'
17. alias ping='ping -c 4'
18. alias install='sudo pacman -S'
Other installations
19. install firefox 
20. install open-vm-tools
