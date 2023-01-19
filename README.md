<p align="center">
<img
    width="320"
    src="docs/img/mybonk_label.png"
    alt="MYBONK logo">
</p>
<br/>
<p align="center">
    <a href="https://cirrus-ci.com/github/mybonk/mybonk-core" target="_blank">
        <img src="https://api.cirrus-ci.com/github/mybonk/mybonk-core.svg?branch=master" alt="CirrusCI status">
    </a>
    <a href="https://github.com/mybonk/mybonk-core/releases/latest" target="_blank">
        <img src="https://img.shields.io/github/v/release/mybonk/mybonk-core" alt="GitHub tag (latest SemVer)">
    </a>
    <a href="https://github.com/mybonk/mybonk-core/commits/master" target="_blank">
        <img src="https://img.shields.io/github/commit-activity/y/mybonk/mybonk-core" alt="GitHub commit activity">
    </a>
    <a href="https://github.com/mybonk/mybonk-core/graphs/contributors" target="_blank">
        <img src="https://img.shields.io/github/contributors-anon/mybonk/mybonk-core" alt="GitHub contributors">
    </a>
    <a href="https://github.com/mybonk/mybonk-core/releases" target="_blank">
        <img src="https://img.shields.io/github/downloads/mybonk/mybonk-core/total" alt="GitHub downloads">
    </a>
</p>
<br/>
👆 this is work in progress, will get this fixed with commit of first release.


---
# Table of Contents

  - [Foreword](#foreword)
  - [Terminology](#terminology)
  - [Overview](#overview)
  - [Advice](#advice)
  - [1. Build the orchestration machine](#build-orchestration-machine)
    -  [1.1 Build the orchestration machine](#build-orchestration-machine)
    -  [1.2 Build the orchestration machine](#build-orchestration-machine)
      - [Option 1]
      - [Option 2]
  - [2. Build the MYBONK full node](#build-mybonk-full-node)
    -  [2.2 Build the orchestration machine](#build-orchestration-machine) 
    -  [2.2 Build the orchestration machine](#build-orchestration-machine) 



# Foreword
Welcome in our rabbit hole. 
You arrived here for a reason. Read this document from the beginning to the end once, then read it again before you get your hands dirty. 
  
We [collaboratively] take great pride and care editing this documentation so it remains clear and concise, often it references external links. Explore them when instructed to, this will make the process as straightforward and pleasant as can be.
  
You too can contribute to this documentation on GitHub.
  
Enjoy the ride, no stress, Check out the [FAQ](FAQ.md) and the [things that really make a difference](BABY-RABBIT-HOLES.md)  :hole: :rabbit2:

# Terminology
- **MYBONK core**: Tailor-made full-node software stack for MYBONK console (although it can run on pretty much any hardware if you are ready to tune and hack a little bit). MYBONK core is based on nix-bitcoin itself based on nixOS. [Software stack](MYBONK-STACK.md).
- **MYBONK console**: A [full-node bitcoin-only hardware platform](https://mybonk.co) designed with security, price, performance, durability, low-enery, supply chain resilience and generic parts in mind.
- **MYBONK user**: Merchant, family man, citadel, institution, bank .etc.. Just want it to work "plug and forget". Only uses web-based GUIs (not the command line). On MAINNET.
- **MYBONK operator**: A "MYBONK hacker" that got really hooked and decided to learn more, has some "skin in the game". On MAINNET.
- **MYBONK hacker**: Student, Maker, researcher. Just want to tear things apart. Love using only the terminal. On SIGNET.

# Overview

This small ecosystem consists of only two elements that we are going to build together:
  
- **One orchestration machine:**
  This machine is used to orchestrate your fleet of MYBONK consoles, it is essentially a Linux with a few additional software installed including the nix package manager.
- **One MYBONK console:**
  This machine runs the MYBONK stack. It is setup once and its configuration can be updated remotly using the orchestration machine.
  You could generate as many MYBONK consoles as you want as easily, it's the goal, but let's keep this simple here.
  
# Advice
- **Don't trust, verify**: Anything you download on the internet is at risk of being malicious software. Know your sources. Always run the GPG (signature) or SHA-256 (hash) verification (typically next to the download link of an image or package there is a sting of hexadecimal characters, it's no decoration).
- **nix vs. nixOS**: To start on the right foot it is very important to understand the concept that nix and nixOS two different things: nix is a [package manager](https://en.wikipedia.org/wiki/Package_manager) (something like npm, rpm and others) whereas nixOS is a [full-blow Linux distribution](https://en.wikipedia.org/wiki/NixOS) built on top of the nix package manager.
- **Read and explore**: The pros write and read documentation, not so much on YouTube. 

<a name="build-orchestration-machine"></a>
===
# 1. Build the orchestration machine
This machine is used to orchestrate your fleet of MYBONK consoles.
  
It does not have to run nixOS (only nix package manager), you could use your day to day laptop but Nix installs quite a few things deep in the system and I like to keep things separate. 
  
The steps hereafter are based on a Linux Debian installed in a VirtualBox virtual machine running on my Mac laptop.
### 1.1. Download and install VirtualBox
Follow the instructions on their website

[https://www.virtualbox.org](https://www.virtualbox.org/)

### 1.2. Build the OS
  Now that VirtualBox is installed you need to have an OS running on it (Linux Debian in our case).
  
  There are 2 ways to do this:
  #### Option 1: Use a default installation image from Debian (https://www.debian.org/distrib/)
  - With this method you go through the standard steps of installing the Debian OS in the VirtualBox just as if you were installing it on a new desktop.
  - Don't forget to take note of the the machine's IP address and login details you choose during the installation!
  - Detailed instructions here: https://techcolleague.com/how-to-install-debian-on-virtualbox/
  #### Option 2: Use a ready-made Virtual Box VDI (Virtual Disk Image)
  - More convenient than OPTION 1 as we use a pre-installed Debian System. 
  - Example: https://www.linuxvmimages.com/images/debian-11/ the login details are typically on the same page as the download link. 
  - Do not use such images in a production environment. 
  - It is possible you get 'Loggin incorrect' when trying to ssh in the box. This is a common issue when using a certain language's OS with another language's keyboard (e.x. QWERTY vs AZERTY) there are various easy ways to work around this that are out of the scope of this document. The simplest and effective is to find a way to login with the keyboard you have anyways until you figure out which key is which then once logged-in you can ajust the settings in "Region & Language" > "Input Source" using the mouse in Gnome or KDE.
### 1.3. Install the Nix packages
  There are 2 ways to do this:
  #### Option 1: Build Nix from the source
  - Follow the instructions at https://nixos.org/nix/manual/#ch-installing-source
  #### Option 2: Install using the build from Nix cache
  - This is quicker and more convenient for test environments.
  
  ssh into the machine:
  
      ```
      $ sh < (curl -L https://nixos.org/nix/install) --daemon
      ```
      
  Note: If you prefer to build the system from source instead of copying binaries from the Nix cache, add the following line to /etc/nix.conf

### 1.4. Prepare the image of nix-bitcoin


### 1.5. Tune the configuration of nix-bitcoin
  Being tested & documented.
  
  
<a name="build-mybonk-full-node"></a>
# 2. Build MYBONK
  ### 2.1. Overlay the configuration of nix-bitcoin
  Being tested & documented.

  ### 2.2. Overlay the configuration of nix-bitcoin
  Being tested & documented.

  
  
  
Sill a long way to go. ETA 10Jan2023

