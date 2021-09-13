# Xinu virtualbox on Windows

There is no Linux VM needed, this method compiles xinu on WSL, which is a new functionality of Windows. This will save a lot of runtime resources and disk space.

**Note: the `your Documents dir` can be changed to other, but keeping them integridy is necessary.**

# Step 1 : Xinu backend

1. Download and install latest Windows Version [Virtualbox](https://www.virtualbox.org/wiki/Downloads)

2. Download [Xinu Virtualbox pack](https://www.cs.purdue.edu/homes/comer/downloads/private/Xinu/xinu-vbox-appliances.tar.gz)

3. Extract the tarball, open Virtualbox and click `Tools->import`, then choose the `backend.ova` file you just extracted.

4. Setup settings

> Network->Adapter 1->Host-only Adapter, Promiscuous mode => Allow all

> Serial Ports->Host Pipe-> type `\\.\pipe\xinu` in Path

# Step 2 : Development and building software

1. follow [this](https://ubuntu.com/tutorials/ubuntu-on-windows) to enable WSL and install Ubuntu on Windows. *Note: This may take a while*

2. install and config Tftpd64 to pass grub file and boot ELF.

3. install PuTTY to connect serial pipe of Xinu.

# Step 3 : Xinu source code

I'll make a repo on github later.

1. Open Ubuntu, cd to `/mnt/c` and then cd to your Document dir

2. `git clone **not yet hosted repo**`

3. open `xinu_raw/compile/Makefile`, find the line`cp xinu.elf /mnt/c/Users/wanhz/Documents/xinu.boot`, change this path to your Document path (*You may only change wanhz to your Windows username*).

After modification, makefile will build and copy the boot file to your Documents dir.

4. You can compile and check now, if there is a new `xinu.boot` file in your dir, build enviroment is well configured now.

# Step 4 : Set up runtime and boot environment

1. copy `xinu/xinu.grub` to your Documents dir

2. open `Tftpd64`

> Setting->TFTP->Base Directory => Your Documents dir

> Setting->TFTP->check PXE comp

> Setting->TFTP->Bind address => 192.168.201.1

> Setting->DHCP->pool start => 192.168.201.2

> Setting->DHCP->Boot file => xinu.grub

> Setting->DHCP->DHCP Mask => 255.255.255.0

> Setting->DHCP->All Other Address => 192.168.201.1

> Setting->DHCP->Bind address => 192.168.201.1

Then leave setting and leave this software alone because it is listening like a server now.

3. Start Xinu `backend` in Virtualbox

It should boot properly(shows GRUB Welcome line)

4. open `PuTTY`, choose serial, set baud to 115200, open `\\.\pipe\xinu`.

Now the Xinu welcome text blocks should display on PuTTY window. If you are seeing this window, everything is cool now.
