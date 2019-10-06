# jake-miner
Jake's AMD ETH miner

### Parts
```
Frame - https://amzn.to/2p6L35n

GPU (Not exactly the same because the XFX RX 580 are no longer available) - https://amzn.to/2mLqMl7

Motherboard - https://amzn.to/2mLtyqx

RAM - https://amzn.to/2onGS4K

CPU - https://amzn.to/2nEyb6i

Power Supply - https://amzn.to/2mI0D6G

PCI-E Risers - https://amzn.to/2nsb0w1
```

### Manual Build Steps - VARIATION #1 <--About 10 mega hashes per card
```
# Get Ubuntu 16.04.3
# No on 3rd party and No on updates!
# REF: http://old-releases.ubuntu.com/releases/16.04.3/
# ISO: http://old-releases.ubuntu.com/releases/16.04.3/ubuntu-16.04.3-desktop-amd64.iso

# FROM THE MINING RIG:
# Install ssh server in Ubuntu 18
sudo apt install tasksel
sudo tasksel install -y openssh-server
service ssh status

# Get IP Address of the mining rig:
hostname -I


# FROM A REMOTE COMPUTER:
# SSH to the mining rig:
ssh jacob@192.168.1.46

# See current kernel version
sudo uname -r
cat /etc/os-release
lsb_release -a

#  Create kit dir:
sudo mkdir -p /opt/kit
sudo chmod -R a+rw /opt/kit
cd /opt/kit

# Get AMD 17.40
# REF: https://www.amd.com/en/support/previous-drivers/graphics/radeon-500-series/radeon-rx-500-series/radeon-rx-580
wget --referer support.amd.com https://drivers.amd.com/drivers/linux/ubuntu/amdgpu-pro-17.40-492261.tar.xz
tar xf amdgpu-pro-17.40-492261.tar.xz
cd amdgpu-pro-17.40-492261

# Do we have the cherry picked files needed?
ls -alF amdgpu-pro-core*
ls -alF libopencl1-amdgpu-pro*
ls -alF clinfo-amdgpu-pro*
ls -alF opencl-amdgpu-pro-icd*
ls -alF amdgpu-pro-dkms*
ls -alF libdrm2-amdgpu-pro*
ls -alF ids-amdgpu-pro*
ls -alF libdrm-amdgpu-pro-amdgpu1*

# OpenCV cherry pick install!
# REF: https://linuxconfig.org/install-opencl-for-the-amdgpu-open-source-drivers-on-debian-and-ubuntu
sudo apt install -y build-essential dkms
sudo dpkg -i amdgpu-pro-core_17.40-492261_all.deb
sudo dpkg -i libopencl1-amdgpu-pro_17.40-492261_amd64.deb
sudo dpkg -i clinfo-amdgpu-pro_17.40-492261_amd64.deb
sudo dpkg -i opencl-amdgpu-pro-icd_17.40-492261_amd64.deb
sudo dpkg -i amdgpu-pro-dkms_17.40-492261_all.deb
sudo dpkg -i libdrm2-amdgpu-pro_2.4.82-492261_amd64.deb
sudo dpkg -i ids-amdgpu-pro_1.0.0-492261_all.deb
sudo dpkg -i libdrm-amdgpu-pro-amdgpu1_2.4.82-492261_amd64.deb


# Install OpenCL
sudo apt install -y clinfo
clinfo


# Source build: ethminer
# REF: https://github.com/ethereum-mining/ethminer/blob/master/docs/BUILD.md
sudo apt install -y cmake git libdbus-1-dev mesa-common-dev

# Create a workspace for the kit:
sudo mkdir -p /opt/kit
sudo chmod a+rw -R /opt/kit
cd /opt/kit

# Clone down the source code:
git clone https://github.com/ethereum-mining/ethminer
cd ethminer
git submodule update --init --recursive
mkdir build
cd build

# cmake
cmake .. -DETHASHCUDA=OFF -DETHASHCL=ON -DETHSTRATUM=ON

# Build
cmake --build .

# Install
sudo make install

# List
ethminer -G --list-devices
```

### Manual Build Steps - VARIATION #2 <--About 25 mega hashes per card
```
# Get Ubuntu 16.04.3
# No on 3rd party and No on updates!
# REF: http://old-releases.ubuntu.com/releases/16.04.3/
# ISO: http://old-releases.ubuntu.com/releases/16.04.3/ubuntu-16.04.3-desktop-amd64.iso

# FROM THE MINING RIG:
# Install ssh server in Ubuntu 18
sudo apt install tasksel
sudo tasksel install -y openssh-server
service ssh status

# Get IP Address of the mining rig:
hostname -I


# FROM A REMOTE COMPUTER:
# SSH to the mining rig:
ssh jacob@192.168.1.46

# See current kernel version
sudo uname -r

#  Create kit dir:
sudo mkdir -p /opt/kit
sudo chmod -R a+rw /opt/kit
cd /opt/kit

# Get AMD 17.40
# REF: https://www.amd.com/en/support/previous-drivers/graphics/radeon-500-series/radeon-rx-500-series/radeon-rx-580
wget --referer support.amd.com https://drivers.amd.com/drivers/linux/ubuntu/amdgpu-pro-17.40-492261.tar.xz
tar xf amdgpu-pro-17.40-492261.tar.xz
cd amdgpu-pro-17.40-492261

# Install normally
./amdgpu-pro-install -y
# Do NOT reboot and do the "fix" now!
./amdgpu-pro-install --px
# Choose Y <--For YES (as needed) to uninstall/re-install


# Install OpenCL
sudo apt install -y clinfo
clinfo


# Source build: ethminer
# REF: https://github.com/ethereum-mining/ethminer/blob/master/docs/BUILD.md
sudo apt install -y cmake git libdbus-1-dev mesa-common-dev

# Create a workspace for the kit:
sudo mkdir -p /opt/kit
sudo chmod a+rw -R /opt/kit
cd /opt/kit

# Clone down the source code:
git clone https://github.com/ethereum-mining/ethminer
cd ethminer
git submodule update --init --recursive
mkdir build
cd build

# cmake
cmake .. -DETHASHCUDA=OFF -DETHASHCL=ON -DETHSTRATUM=ON

# Build
cmake --build .

# Install
sudo make install

# List
ethminer -G --list-devices
```

### Manual Build Steps - VARIATION #3 <--Trying to get ~30 mega hashes per card <--WIP
```
# Get Ubuntu 16.04.3
# No on 3rd party and No on updates!
# REF: http://old-releases.ubuntu.com/releases/16.04.3/
# ISO: http://old-releases.ubuntu.com/releases/16.04.3/ubuntu-16.04.3-desktop-amd64.iso

# FROM THE MINING RIG:
# Install ssh server in Ubuntu 18
sudo apt install tasksel
sudo tasksel install -y openssh-server
service ssh status

# Get IP Address of the mining rig:
hostname -I


# FROM A REMOTE COMPUTER:
# SSH to the mining rig:
ssh jacob@192.168.1.46

# See current kernel version
sudo uname -r
cat /etc/os-release
lsb_release -a

# List possible kernel choices already downloaded:
dpkg -l | awk  '{print $2" "$3}' | grep ^linux-image


# Install old kernel 4.10x on Ubuntu 16x
# Note: You may be able to skip and just edit grub (below) if the older kernel is already installed (but not active)
# REF: https://askubuntu.com/questions/700214/how-do-i-install-an-old-kernel
# All: http://old-releases.ubuntu.com/ubuntu/pool/main/l/linux/
# Note: https://help.ubuntu.com/community/RemoveOldKernels
sudo mkdir -p /opt/kit/oldkern
sudo chmod a+rw -R /opt/kit/oldkern
cd /opt/kit/oldkern
# Headers
wget http://old-releases.ubuntu.com/ubuntu/pool/main/l/linux/linux-headers-4.10.0-19-generic_4.10.0-19.21_amd64.deb
wget http://old-releases.ubuntu.com/ubuntu/pool/main/l/linux/linux-headers-4.10.0-19_4.10.0-19.21_all.deb
# Kernel
wget http://old-releases.ubuntu.com/ubuntu/pool/main/l/linux/linux-image-4.10.0-19-generic_4.10.0-19.21_amd64.deb
# Install old kernel
ls -alFh
sudo dpkg -i *.deb

# Edit the grub menu:
sudo vi /etc/default/grub
GRUB_DEFAULT=saved
GRUB_SAVEDEFAULT=true
GRUB_HIDDEN_TIMEOUT=10
#GRUB_TIMEOUT_STYLE=hidden
GRUB_TIMEOUT=5
GRUB_HIDDEN_TIMEOUT_QUIET=false
:wq!

# Update grub
sudo update-grub

# Reboot
sudo reboot

# On the next boot, hold LEFT SHIFT and choose advanced from the grub boot menu
# Then select kernel 4.15 and boot
# Here after, it should remember the last kernel choice that was previously chosen on each reboot
# Press ESCAPE to get the menu!

# Get IP Address of the mining rig:
hostname -I


# FROM A REMOTE COMPUTER:
# SSH to the mining rig:
ssh jacob@192.168.1.46

# See current kernel version
sudo uname -r

#  Create kit dir:
sudo mkdir -p /opt/kit
sudo chmod -R a+rw /opt/kit
cd /opt/kit

# Get AMD 17.40
# REF: https://www.amd.com/en/support/previous-drivers/graphics/radeon-500-series/radeon-rx-500-series/radeon-rx-580
wget --referer support.amd.com https://drivers.amd.com/drivers/linux/ubuntu/amdgpu-pro-17.40-492261.tar.xz
tar xf amdgpu-pro-17.40-492261.tar.xz
cd amdgpu-pro-17.40-492261


# I was able to get the AMD drivers working on Ubuntu 16.04.03 LTS using 17.40 AMD Drivers
# Install normally
./amdgpu-pro-install -y
# Do NOT reboot and do the "fix" now!
./amdgpu-pro-install --px
# Choose Y <--For YES (as needed) to uninstall/re-install
# Try a -y to avoid being prompted??
#./amdgpu-pro-install --px -y

# List installed AMD GPU
dpkg -l | awk  '{print $2" "$3}' | grep ^amd

# Reboot
sudo reboot


# FROM A REMOTE COMPUTER:
# SSH to the mining rig:
ssh jacob@192.168.1.46

# See current kernel version
sudo uname -r
# Review current AMD GPU drivers installed
dpkg -l | awk  '{print $2" "$3}' | grep ^amd


# Install OpenCL
sudo apt install -y clinfo
clinfo


# Source build: ethminer
# REF: https://github.com/ethereum-mining/ethminer/blob/master/docs/BUILD.md
sudo apt install -y cmake git libdbus-1-dev mesa-common-dev

# Create a workspace for the kit:
sudo mkdir -p /opt/kit
sudo chmod a+rw -R /opt/kit
cd /opt/kit

# Clone down the source code:
git clone https://github.com/ethereum-mining/ethminer
cd ethminer
git submodule update --init --recursive
mkdir build
cd build

# cmake
cmake .. -DETHASHCUDA=OFF -DETHASHCL=ON -DETHSTRATUM=ON

# Build
cmake --build .

# Install
sudo make install

# List
ethminer -G --list-devices
```
