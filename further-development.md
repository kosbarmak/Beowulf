# Further Development 
(Including limitations)

1. We are interested in having the headnode be able to wake up on lan and wake up the rest of the nodes. At the current moment, the head node is capable of waking up when suspended, but not when shut down. We have yet to try and see if a PC can be awoken if provided a USB signal. We have a Raspberry Pi in the office that we believe we can always keep running for this purpose. This might also help with waking up each worker node as the network cards do not support wake up on lan. It is for this reason that we're interested in getting a usb hub to connect to each worker node.
2. There are three PC's in the cluster that do not have a GPU. Two of the worker nodes need a GPU, specifically node00 and node04. While this does not prevent a node from being a part of the cluster (slurm or mpi), it has a noticable effect on computation time. For example, running the test `srun -N16 -n16 /bin/hostname` will always result in node00 and node04 being last. If and when any GPU's can be spared from outside the cluster, we humbly and kindly request that they would be given to node00 and node04. We did end up taking a GPU from node13 (grandpa), but there seems to no noticable effect on its performance in this regard.
- This slowdown in the headless PC's are odd, so we seek to eventually set up the nodes to use a lightweight server version of Ubuntu.

3. There is a limited number of power cables in the room with the cluster, and all of the cables (save two or three) have been devoted to the PC's. We would like more power cables to support more monitors.

5. We would like to assign a static dns entry point for the cluster. This however requires reaching out to ITS and asking for a static IP and a dns entry point. 

- Before requesting a public DNS/static IP entry from ITS, the cluster should meet appropriate security requirements and be reviewed with faculty/ITS. 
    
5. Upon verifying security, we would like to set up an account management system for the cluster (similar to that of the Cal Poly unix server), such that a person can `ssh` into the cluster if they have a Cal Poly account. From there, they can simply copy over via `scp` their project files and request a slurm job. 

