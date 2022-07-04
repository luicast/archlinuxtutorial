
loadkeys la-latin1
timedatectl set-ntp true

cfdisk
sda1 512MB EFI
sda2 6GB   SWAP
sda3 *     EXT4 boot

lsblk
mkfs.fat -F32 /dev/sda1
mkswap /dev/sda2
swapon /dev/sda2
mkfs.btrfs /dev/sda3

mount /dev/sda3 /mnt

ls 
ls /
mkdir /mnt/boot
mkdir /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
nano /etc/pacman.d/mirrorlist

pacstrap /mnt base base-devel linux linux-firmware nano
genfstab -U  /mnt >> /mnt/etc/fstab
nano /mnt/etc/fstab

arch-chroot /mnt

ln -sf /usr/share/zoneinfo/Canada/Eastern /etc/localtime
hwclock --systohc
nano /etc/locale.gen          - - - - - - - - - en_US.UTF-8 UTF-8
                                       
echo LANG=en_US.UTF-8 > /etc/locale.conf
echo KEYMAP=la-latin1 > /etc/vconsole.conf
echo NatyZaty > /etc/hostname


nano /etc/hosts

127.0.0.1            localhost
::1                  localhost
127.0.0.1            Archlinux.localdomain    Archlinux



pacman -S networkmanager
systemctl enable NetworkManager

passwd 
pacman -S grub efibootmgr
grub--install --target=x86_64-efi --efi-directory=boot/efi
grub-mkconfig -o /boot/grub/grub.cfg
unmount -R /mnt                                        
exit
reboot
