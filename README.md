# Beowulf Cluster

This is the basic documentation for the Ubuntu-based compute cluster based in the Computer Science Lab at California Polynechnic State University, San Luis Obispo, called Beowulf. It features a total of 16 PC's for a total of 240 CPU cores processing power all linked together in a private network.


Constructed by Chris Amey and Alan Kusparmakov.

Special thanks to Ceya Badyal, Dr. Christian Eckhardt, Dr. Aaron Keen, and Dr. John Bellardo.

## Basic Usage

SSH into the head node first. You must receive login credentials from a cluster administrator. 

From the head node, connect to worker nodes:

```bash
ssh node00
ssh node01
```

Run a simple Slurm check:

```bash
sinfo
srun hostname
```

Run a command using multiple CPU tasks:

```bash
srun -N 2 -n 4 hostname
```

Access the MikroTik from inside the cluster LAN:

```bash
ssh admin@192.168.88.1

or

ssh mikrotik
```


## Key Tools

- SSH — node access
- Slurm — job scheduling
- OpenMPI — MPI workloads
- MikroTik RouterOS — LAN/DNS/switch management

## Hardware
The following hardware was used to establish the local area network. 

We were provided a total of 16 PC's (17 if you count the raspberry pi), most of which are homogeneous in hardware. All of them are installed with Ubuntu and run it on boot.

The following hardware pieces were used to establish the local area network. 

1. Network Switch (changed to MikroTik CRS326-24S+2Q+RM):

Per Node: 
1. Intel X520-DA2 10Gb Network Card 
2. 10Gtek SFP+ Direct Attach Copper (DAC) Cables


## More Documentation

- `lan-setup.md` — network layout, node addresses, DNS, MikroTik access
- `node-setup.md` — adding/installing a worker node, IPv4 settings, SSH, MikroTik DNS entry
- `further-development.md` — limitations, requested hardware, future cluster work

## Further Reading
**Beowulf Cluster Computing with Linux/Windows, by Thomas Sterling**

Although somewhat dated, the introductory chapters of this book were a great entry points to learn about both the design and history of beowulf clusters. 

**OpenMPI**

This is an open-source implementation of MPI (Message Passing Interface), which allows for different cores in a multiprocessing program to send data to one another.

https://docs.open-mpi.org/en/v5.0.x/


**Slurm**

This is an open-source workload manager that can work together with MPI, as well as on its own for simpler tasks. It allocates resources, can do diagnostics and send jobs across the cluster. It is preferred for when the cluster access is given to multiple clients. Slurm is used in many supercomputers, such as the one in San Diego State University. 

https://slurm.schedmd.com/documentation.html
