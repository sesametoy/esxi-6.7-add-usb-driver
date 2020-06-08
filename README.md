# esxi-6.7-add-usb-driver

Remotely access ESXI using SSH to access ESXI’s shell

Stop the USB arbitrator service

```
/etc/init.d/usbarbitrator stop
```

Plug in and look for your desired USB device

```
ls /dev/disks/
```

Write a GPT label to your USB device using it’s ID:
```
partedUtil mklabel /dev/disks/<deviceID> gpt
partedUtil getptbl /dev/disks/<deviceID>
```
Calculate (yes, math) the end sector for your new partition from the resulting numbers from the following command:

# 121597 * 255 * 63 -1 = 1953455804
```
partedUtil setptbl /dev/disks/<deviceID> gpt "1 2048 <endSector> AA31E02A400F11DB9590000C2911D1B8 0"
```

Format your new partition with VMFS6
```
vmkfstools -C vmfs6 -S <disk-name> /dev/disks/<deviceID>:1 
```

# Re-disable SSH access to your ESXI host
