---
layout: post
title: Docker setup
comments: true
tags: [docker]
---

Installation Steps:

- Install HyperV (or another virtualization engine)

- [Ubuntu](www.ubuntu.com) 
  Download -> Server -> LTS

- [Docker](www.docker.com)
  [install on ubuntu](https://docs.docker.com/installation/ubuntulinux/)
  
 Setup HyperV VM
 Important! options: Generation 2 (2GB not dynamic RAM) 
 specify the ISO to install the VM
 
 VM configuration
 - Firmaware remove 'protected startup' -> prevent non Windows OS from starting
 
 connect and start the VM
 and start the setup
 
 Ubuntu install:
 selected 'no localization' (option C)
 
 Partitioning:
 important: SCSI1
 create a partition: 512 MB (the size must be 512 MB)
 'Use as': EFI boot
 
 setup a second partition: MAX size - 2MB
 'use as': btrfs
 
 create 3rd partition (SWAP area)
 named: swap
 size: 2MB
 'use as': SWAP AREA
 
 ------
 
 to install docker:
 sudo -i (get administration right)
 install docker following the instruction on the website
 
 ------
 usage
 
 docker search 'NOME PACCETTO'
 
 docker run -ti 'NOME PACCHETTO' (first time install, after this docker start)
 
 ps ax (lista processi) (process list)
 
 docker ps -a (list of docker names - containers - in the system)
 
 docker start containerName
 docker attach containerName 
 
 to remova container
 docker rm containerName
 
 to delete the image
 docker rmi imageNa

 
 
 
 
 
 ------
 
 Other tools:
 - Putty
 - WinSCP
 