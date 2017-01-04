---
layout: post
title: "Fixing Docker: no space left on device"
date: 2017-01-04 07:02
comments: true
categories:
---

## The Problem
If you're runing docker on OSX more than just a little, you've probably run into an issue where you're building a container and it fails due to an error that looks something like the following...

```
Error response from daemon: mkdir /var/lib/docker/tmp/docker-builder983959066: no space left on device
```

From this point on, you won't be able to build new containers or images until...
1) you delete existing contaimers or images
or
2) completely delete the virtual disk image that contains all of your docker containers and images

## The Breakdown
Ok, so lets look at what this error is telling us. The context is important here.

On OSX, when building a docker container or image, the work is being done inside of a virtual machine. If you've looked at /var/lib/docker... on your local machine, you may have noticed that its either not there or it is but it isn't full. The reason for this is becuase the /var/lib/docker folder that the error is referring to lives inside of the virtual machine in which docker for mac or docker-machine is doing its building.

The virtual file system lives on your disk here: `/Users/philip/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/Docker.qcow2`

By default this virtual disk is 20G.

### Troubleshooting
To confirm that this is really your issue, here are the steps that I used. Your disk size and space used will be different as I ran these after resolving the issue. Either way, this will give you a good idea of how it all works.

#### Checking the Size of the Virtual Disk Image File
If you check the size of this file...

```
$ ls -altrh
total 47009328
-rw-r--r--   1 philip  staff    36B Nov 17 11:14 nic1.uuid
-rw-r--r--   1 philip  staff     0B Nov 17 11:14 lock
drwxr-xr-x  83 philip  staff   2.8K Jan  1 13:09 log/
lrwxr-xr-x   1 philip  staff    12B Jan  3 14:39 tty@ -> /dev/ttys004
-rw-r--r--   1 philip  staff     5B Jan  3 14:39 pid
-rw-r--r--   1 philip  staff    17B Jan  3 14:39 mac.0
-rw-r--r--   1 philip  staff     5B Jan  3 14:39 hypervisor.pid
drwxr-xr-x  21 philip  staff   714B Jan  3 14:39 ../
drwxr-xr-x  12 philip  staff   408B Jan  3 14:39 ./
-rw-r--r--   1 philip  staff   705B Jan  3 14:39 syslog
-rw-r--r--   1 philip  staff    64K Jan  3 20:12 console-ring
-rw-r--r--   1 philip  staff    22G Jan  4 07:11 Docker.qcow2
```

... you'll notice that it's 22G

#### Check Space Used From Within the VM
Then if you take a peek at the space used from within a container, it's slightly different but fairly close...
(pay attention to the root partition in this context)

```
docker run --rm --privileged debian:jessie df -h
Filesystem      Size  Used Avail Use% Mounted on
none             33G   20G   12G  63% /
tmpfs           999M     0  999M   0% /dev
tmpfs           999M     0  999M   0% /sys/fs/cgroup
/dev/vda1        33G   20G   12G  63% /etc/hosts
shm              64M     0   64M   0% /dev/shm
```

While I was having this issue, I was unable to run this command due to lack of space so you may not be able to do this until you fix the issue.

## The Fix
To fix this issue, we will need to do the following.

+ Stop Docker
+ Expand the disk image size
+ Launch a VM in which we run GParted against the Docker.qcow2 image
+ Expand the partition to use the additional space added to the disk image
+ Exit the VM and restart docker

### Stop Docker
Lets go ahead and stop docker so that the disk image is not being used while we resize. This may not be required but better safe than sorry...

### Install QEMU
To launch the virtual machine, you'll need QEMU or something that can boot from an ISO and mount a qcow2 image. For this example, I'm using QEMU.

```
brew install qemu
```

### Expand the disk image size
To expand the disk image, we'll use the qemu-img util packaged with Docker for MacOS. If you can't find this on your system, you should be able to get this from the qemu package.

```
$ /Applications/Docker.app/Contents/MacOS/qemu-img resize ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/Docker.qcow2 +5G
```

If you would like to expand it more or less, you can change the `+5G` on the end of the command as needed.

### Download the GParted Live Image
Visit [http://gparted.org/download.php](http://gparted.org/download.php) and download the gparted-live ISO for your architecture. In this case, I downloaded gparted-live-0.27.0-1-amd64.iso.

### Launch the VM Running GParted
Here we run qemu and launch a virtual machine adding our Docker.qcow2 disk image as a drive.

+ When prompted, you'll select the options for booting GParted Live.
+ Select don't touch keymap (unless you know what you're doing)
+ The next step should default to 33 (US English) so change it if needed, otherwise, hit enter
+ For mode, select start X & GParted automatically which should be default
+ Click on the GParted icon

While launching, I saw a warning stating `overlayfs: missing 'workdir'`. You can safely ignore this. Just be patient and let it finish booting.

It may take a bit for the machine to completely come up so give it some time...

```
$ qemu-system-x86_64 -drive file=Docker.qcow2  -m 512 -cdrom ~/Downloads/gparted-live-0.27.0-1-amd64.iso -boot d -device usb-mouse -usb
```

### Expand the Partition
In GParted...

+ select the partition (it should be the largest one and should match the size you've seen when inspecting the image)
+ right click the partition and select resize/move
+ resize it to use the full amount of space allocated for the disk by dragging the right size of the darkened box to the far right of the block
+ click resize
+ click apply
+ close GParted and exit the VM

### Start Docker
Start docker back up however you normally would start it.

## Confirm it Worked
At this point you can go back and run your commands to check space used from within the VM and confirm that the available size has increased as expected. If it did, you should be good to go!
