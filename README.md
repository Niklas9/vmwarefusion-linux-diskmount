# vmwarefusion-linux-diskmount

1. Go into "Settings" for the VM in VMware Fusion -> "Sharing" -> "Add Device" and add a local folder that should be shared (with read/write or whatever permission you decide)

2. Install vmware-tools inside the VM

```
$ sudo apt-get install open-vm-tools
```

Reference: https://askubuntu.com/questions/762755/no-vmhgfs-file-system-installed-to-use-use-shared-folder

3. Mount the directory inside the VM,

```
sudo /usr/bin/vmhgfs-fuse .host:/shared-folder /mnt/shared-folder -o subtype=vmhgfs-fuse,allow_other
```

Reference: https://github.com/vmware/open-vm-tools/issues/199

4. Make sure it mounts automatically on boot by editing fstab. Remember to update user and group id from 1000 to whatever the user you want to use has (but on Debian systems this is the id for the first user created).

```
sudo -- sh -c 'echo ".host:/shared-folder /mnt/shared-folder fuse.vmhgfs-fuse uid=1000,gid=1000,defaults,allow_other 0 0" >> /etc/fstab'
```

Reminder to double check that you didn't screw up /etc/fstab before rebooting, since it might cause your VM to not boot

Simply run 

```
sudo mount -a
```

and make sure there's no errors.