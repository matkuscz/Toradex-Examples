# Toradex-Examples
Pokusy o rozchozeni Toradexu


Boot z SD Karty:
- zastavit uboot
- strcit do Toradexu bud SD kartu nebo fleshku s operacnim systemem / Toradex Easy Installerem
- ```run distro_bootcmd```
- to si projede USBecka a karty, co najde prvni zacne bootovat
- bacha muze to podelat bootcmd, cili tady je puvodni:

```
bootcmd=run sdboot; echo; echo sdboot failed; run emmcboot; echo; echo emmcboot failed; run distro_bootcmd; usb start; setenv stdout se
rial,vga; setenv stdin serial,usbkbd

```
- celej originalni environment

```
arch=arm
baudrate=115200
board=apalis_imx6
board_name=apalis_imx6
board_rev=011c
boot_a_script=load ${devtype} ${devnum}:${distro_bootpart} ${scriptaddr} ${prefix}${script}; source ${scriptaddr}
boot_extlinux=sysboot ${devtype} ${devnum}:${distro_bootpart} any ${scriptaddr} ${prefix}extlinux/extlinux.conf
boot_file=zImage
boot_net_usb_start=usb start
boot_prefixes=/ /boot/
boot_script_dhcp=boot.scr.uimg
boot_scripts=boot.scr.uimg boot.scr
boot_targets=mmc1 mmc2 mmc0 usb0 dhcp 
bootcmd=run sdboot; echo; echo sdboot failed; run emmcboot; echo; echo emmcboot failed; run distro_bootcmd; usb start; setenv stdout se
rial,vga; setenv stdin serial,usbkbd
bootcmd_dhcp=if dhcp ${scriptaddr} ${boot_script_dhcp}; then source ${scriptaddr}; fi;
bootcmd_mmc0=setenv devnum 0; run mmc_boot
bootcmd_mmc1=setenv devnum 1; run mmc_boot
bootcmd_mmc2=setenv devnum 2; run mmc_boot
bootcmd_usb0=setenv devnum 0; run usb_boot
bootdelay=1
bootm_size=0x20000000
console=ttymxc0
cpu=armv7
defargs=vmalloc=400M user_debug=30
dfu_alt_info=u-boot.imx raw 0x2 0x3ff mmcpart 0;boot part 0 1;rootfs part 0 2;zImage fat 0 1;imx6q-colibri-eval-v3.dtb fat 0 1;imx6q-co
libri-cam-eval-v3.dtb fat 0 1
distro_bootcmd=for target in ${boot_targets}; do run bootcmd_${target}; done
emmcargs=ip=off root=/dev/mmcblk0p2 ro rootfstype=ext4 rootwait
emmcboot=run setup; setenv bootargs ${defargs} ${emmcargs} ${setupargs} ${vidargs}; echo Booting from internal eMMC chip...; run emmcdt
bload; load mmc 0:1 ${kernel_addr_r} ${boot_file} && run fdt_fixup && bootz ${kernel_addr_r} ${dtbparam}
emmcdtbload=setenv dtbparam; load mmc 0:1 ${fdt_addr_r} ${fdt_file} && setenv dtbparam " - ${fdt_addr_r}" && true
ethact=FEC
ethaddr=00:14:2d:a1:ba:62
ethprime=FEC
fdt_addr_r=0x12100000
fdt_file=imx6q-apalis-ixora-v1.1.dtb
fdt_fixup=;
initrd_high=0xffffffff
ipaddr=192.168.10.2
kernel_addr_r=0x11000000
loadaddr=0x12000000
mmc_boot=if mmc dev ${devnum}; then setenv devtype mmc; run scan_dev_for_boot_part; fi
netmask=255.255.255.0
nfsargs=ip=:::::eth0:on root=/dev/nfs rw
nfsboot=run setup; setenv bootargs ${defargs} ${nfsargs} ${setupargs} ${vidargs}; echo Booting via DHCP/TFTP/NFS...; run nfsdtbload; dh
cp ${kernel_addr_r} && run fdt_fixup && bootz ${kernel_addr_r} ${dtbparam}
nfsdtbload=setenv dtbparam; tftp ${fdt_addr_r} ${fdt_file} && setenv dtbparam " - ${fdt_addr_r}" && true
pxefile_addr_r=0x17100000
ramdisk_addr_r=0x12200000
sata_boot=if sata dev ${devnum}; then setenv devtype sata; run scan_dev_for_boot_part; fi
scan_dev_for_boot=echo Scanning ${devtype} ${devnum}:${distro_bootpart}...; for prefix in ${boot_prefixes}; do run scan_dev_for_extlinu
x; run scan_dev_for_scripts; done;
scan_dev_for_boot_part=part list ${devtype} ${devnum} -bootable devplist; env exists devplist || setenv devplist 1; for distro_bootpart
 in ${devplist}; do if fstype ${devtype} ${devnum}:${distro_bootpart} bootfstype; then run scan_dev_for_boot; fi; done
scan_dev_for_extlinux=if test -e ${devtype} ${devnum}:${distro_bootpart} ${prefix}extlinux/extlinux.conf; then echo Found ${prefix}extl
inux/extlinux.conf; run boot_extlinux; echo SCRIPT FAILED: continuing...; fi
scan_dev_for_scripts=for script in ${boot_scripts}; do if test -e ${devtype} ${devnum}:${distro_bootpart} ${prefix}${script}; then echo
 Found U-Boot script ${prefix}${script}; run boot_a_script; echo SCRIPT FAILED: continuing...; fi; done
scriptaddr=0x17000000
sdboot=run setup; run sdsetup; setenv bootargs ${defargs} ${sdargs} ${setupargs} ${vidargs}; echo Booting from MMC/SD card in 8-bit slo
t...; run sddtbload; load mmc ${sddrive}:1 ${kernel_addr_r} ${boot_file} && run fdt_fixup && bootz ${kernel_addr_r} ${dtbparam}
sddrive=1
sddtbload=setenv dtbparam; load mmc ${sddrive}:1 ${fdt_addr_r} ${fdt_file} && setenv dtbparam " - ${fdt_addr_r}" && true
sdsetup=setenv sdargs ip=off root=/dev/mmcblk${sddrive}p2 ro rootfstype=ext4 rootwait
serial#=10599010
serverip=192.168.10.1
setethupdate=if env exists ethaddr; then; else setenv ethaddr 00:14:2d:00:00:00; fi; tftpboot ${loadaddr} flash_eth.img && source ${loa
daddr}
setsdupdate=setenv interface mmc; setenv drive 1; mmc rescan; load ${interface} ${drive}:1 ${loadaddr} flash_blk.img || setenv drive 2;
 mmc rescan; load ${interface} ${drive}:1 ${loadaddr} flash_blk.img && source ${loadaddr}
setup=run setupvideo; setenv setupargs fec_mac=${ethaddr} consoleblank=0 no_console_suspend=1 console=tty1 console=${console},${baudrat
e}n8
setupdate=run setsdupdate || run setusbupdate || run setethupdate
setupvideo=i2c dev 0; if i2c probe 4A; then setenv vidargs mxc_hdmi.only_cea=1 video=mxcfb0:dev=lcd,FusionF07A,if=RGB24 video=mxcfb1:of
f video=mxcfb2:off video=mxcfb3:off fbmem=32M; fi
setusbupdate=usb start && setenv interface usb; setenv drive 0; load ${interface} ${drive}:1 ${loadaddr} flash_blk.img && source ${load
addr}
soc=mx6
splashpos=m,m
usb_boot=usb start; if usb dev ${devnum}; then setenv devtype usb; run scan_dev_for_boot_part; fi
vendor=toradex
ver=U-Boot 2016.11-2.8.5+g02735f4004 (Aug 01 2019 - 11:44:38 +0000)
vidargs=mxc_hdmi.only_cea=1 video=mxcfb0:dev=hdmi,1280x720M@60,if=RGB24 video=mxcfb1:off video=mxcfb2:off video=mxcfb3:off fbmem=32M

Environment size: 5276/8188 bytes
```
