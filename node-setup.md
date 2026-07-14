# Node Setup

This document will detail how to set up a node for the cluster. The end parts may be incomplete with a lack of knowing the proper script names.

## 0. Find a Working PC

(For this part, the network router switch we have still has enough ports, the wall enough outlets, and the table enough space)

Before anything else, a working PC should be found, which correctly loads BIOS. Below are some issues that may arise and some fixes:  
1. If it power cycles, then verify all parts are plugged in and working, notably the ram. 
2. If it doesn't display BIOS, then verify that it is plugged into a working monitor via HDMI.
3. If it doesn't have a GPU, then you may use a GPU from a (powered off and unplugged) working node, but first verify that it's not the head node and that the network card is not in an invalid PCIe position (use a non-silver slot). After that, you may return the GPU, but mark the one without a GPU as headless. 
4. After placing the GPU into the PCIe slot, connect the bundled cables from the power block to the GPU
5. If it doesn't have a storage and won't make it past BIOS, then there is a spare HDD in a drawer that can be plugged in while it doesn't have power.

After that, you should be able to move on to the next step, which is installing Ubuntu. Typically, this means that the PC will normally boot to Windows. However, you should first power off the PC and plug in a valid network card. This network card should be installed in the lowest PCIe slot available to allow the PC to enable the PC the option to be headless.

## 1. Install Ubuntu

For the sake of ease, please use a Ubuntu Server flash USB that you plug in and then set as first in priority in the boot menu. It also simplifies matters if you plug in the PC to the switch through the network card installed in the previous step. 

Once you get to the network management settings, look for the connection that does not say disconnected. You can then configure the IPv4 settings and set the following:
    1. Lan:     192.168.88.0/24
    2. IP:      192.168.88.<NUM>        (Num is the current node number + 11)
    3. Gateway: 192.168.88.10
    4. DNS: 192.168.88.10 8.8.8.8

In this page, it should then say that the PC is connected to the internet, though it should require that the head node and switch both be powered on. This should then allow you to move on to the next step, where it'll look for packages. It may fail multiple times or not at all, so keep retrying to mirror the packages over. At this point, all that is left is installing openSSH and setting the username, password, and hostname. The first two should be the same as the head node's name, and the hostname should be set to be node<NUM>, using the same number from earlier.

### Issues with a corrupted file system

Unfortunately, more often than not, the PC may get stuck in wiping the file system to prepare for the new OS. From the looks of it, it looks like a stuck installer, but it is hanging if it takes more than a minute on a single step. As a result, it is up to the user to wipe the file system. This can be done by pulling up the terminal using CTRL+ALT+F2. At which point, the user can then enter the following commands, following the disk named from the first command (In example, nvme0n1 was used):
- 1. `lsblk -f`
- 2. `sudo wipefs -a /dev/nvme0n1`
- 3. `sudo sgdisk --zap-all /dev/nvme0n1`
- 4. `sudo partprobe`

After which, the user can then reboot the system (which will require removing the flash usb only to reinsert it). Repeating the steps will allow installing Ubuntu cleanly.

## 2. Allow SSH and Reconfigure

The only thing left to be done directly on the pc is to run 
`
sudo ufw allow ssh 
`

This is a very simple step, but the user should now verify (after running ssh on the given IP) that the head node can access the PC via ssh. At which point, the user can then place the PC where it needs to be. This may require plugging in a monitor to see if the PC is stuck at BIOS recovery due to a weak CMOS battery or unstable BIOS settings. It's also important to run `ssh-copy-id 192.168.88.YY` from the head node.

If the PC is accessible via ssh through the head node, then all that remains is to configure the given static IP within the Mikrotik by ssh-ing into it and running. 

```
ssh mikrotik
ip dns static add name=[nodeXX] address=[192.168.88.YY]
```
To see existing naming scheme:
```
ip dns static print
```

## 3. Install SLURM and MPI

At this point, the head node should be able to access the new node through ssh, which should be physically placed and connected where it needs to be. At which point, the user can then run the following commands on the head node. It is advised to do this with every node powered on and connected since the config needs to be shared across all PC's. The following commands were designed to set up multiple PC's at once, so you can configure it as follows.

```
~/SlurmInstall/addNodes [nodeXX, nodeYY, ...]
~/bin/install-mpi-on-config-hosts --ask-password
```

If the installation went correctly, then you can run `sinfo` and see the new PC included in the cluster.