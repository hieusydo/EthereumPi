# Ethereum Mining on Raspberry Pi 1

## Installation
1. Set up Raspbian on Raspberry Pi
2. Download Ethereum Geth dependencies:
  ```sh
  $ sudo apt-get update
  $ sudo apt-get upgrade -y
  $ sudo apt-get install libgmp3-dev -y
  ```
3. Install Go for RPi. Put your files in `home/pi/bin/`. Can grab a prebuilt tarball from [GoLang-ARMbuilds](https://github.com/hypriot/golang-armbuilds/releases) that is compatible with your system. For my RPi 1, it's armv6. Can do `cat /proc/cpuinfo` to check. 
  ```sh
  $ tar -xzvf <tarball-file>
  $ export PATH=$PATH:/home/pi/bin/go/bin
  $ export GOROOT=/home/pi/bin/go
  ```
4. Install Geth
  ```sh
  $ git clone -b release/<version-you-want> https://github.com/ethereum/go-ethereum.git
  $ cd go-ethereum/
  $ make geth
  $ sudo cp build/bin/geth /usr/local/bin/
  ```
5. Set up and run Ethereum Geth
  ```sh
  $ cd ~/
  $ geth --datadir /home/pi/ethData account new
  $ geth account new
  $ geth --datadir /home/pi/ethData --ipcdisable --autodag console # If using external storage
  $ geth --autodag console # Not using external storage
  ```

## Known Errors
- `running gcc failed: fork/exec /usr/bin/gcc: cannot allocate memory`:
  Create [swapon memory](http://redcorner.io/blog/building-ethereum-light-client-on-chip/) from external thumbdrive
  ```sh
  sudo mkswap /dev/sda1
  sudo swapon /dev/sda1
  ```

## Other RPi and Linux Tricks
- [Prevent Your Raspberry Pi From Sleeping](https://www.bitpi.co/2015/02/14/prevent-raspberry-pi-from-sleeping/)  
  ```sh
  > sudo nano /etc/lightdm/lightdm.conf
  ```
  Add this command below the [SeatDefaults] header
  ```sh
  xserver-command=X -s 0 -dpms
  ```  
- See [free space](https://www.tecmint.com/how-to-check-disk-space-in-linux/): `df [-hT]`
- Unmount thumbdrive (before doing swapon): `umount [options] filesystem`

## Resources
- [Official Go Ethereum Github](https://github.com/ethereum/go-ethereum)
- [EtherScan](https://etherscan.io/)
- [RaspNode](http://raspnode.com/diyEthereumGeth.html)
- [GoLang-ARMbuilds](https://github.com/hypriot/golang-armbuilds/releases)