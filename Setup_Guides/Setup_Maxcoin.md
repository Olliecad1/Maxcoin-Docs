#### Turning on the Pi
Take the micro SD out of the adapter and over to the Raspberry Pi 3 and connect it up to monitor and screen.  
On first boot it will load up the raspi-config console. If this is not your first time then load it using the commmand:
```
sudo raspi-config
```
Select:  
1. Expand File System - click OK
2. Enable SSH
3. Change hostname to eg "maxnode" (without quotes)  
4. Reboot either via the config 

If you want you can reboot manually like so:
```
sudo reboot
```

#### Installing Updates

```
sudo apt-get update
```  
```
sudo apt-get upgrade -y
```  
The -y flag tells the OS to answer "yes" to any prompts warning you of extra disk space required, we use this for convenience but it is not essential.  

#### Install the dependencies
```
sudo apt-get install -y git-core build-essential libssl-dev libboost-all-dev libdb-dev libdb++-dev libminiupnpc-dev libqrencode-dev libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools qt-sdk libssl1.0-dev libssl1.0.2
```

#### Increase Swap File size for installation
Make sure you have at least 2Gb free on the card before starting and increase your swap file like so:
```
sudo nano /etc/dphys-swapfile
```  
Then enter  
```
CONF_SWAPSIZE=1024
```  
Then hit control+X and click Y to save and Enter. Then type the command:  
```
sudo dphys-swapfile setup
```  
```
sudo dphys-swapfile swapon
``` 

#### Create a folder where we will install the files
```
mkdir ~/bin
```  
```
cd ~/bin
```

#### Install the Berkeley DB 4.8
```
wget http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz
```  
```
tar -xzvf db-4.8.30.NC.tar.gz
```  
```
cd db-4.8.30.NC/build_unix/
```  

#### Configuring the system and installing Berkeley DB  
*Note the -j4 flag installs using all four cores on the Raspberry Pi*  
```
../dist/configure --enable-cxx
```  
```
make -j4
```  
```
sudo make install
```  

#### Installing Maxcoin

```
cd ~/bin
```   
```
git clone https://github.com/Max-Coin/MaxCoin.git
```  
```
cd MaxCoin/src
```  

#### Remove Cryptopp 
```
rm -rf cryptopp
```

#### Install the dependencies(It may be already installed)
```
sudo apt-get install unzip
```

#### Creating the Cryptopp and rebuilding
```
mkdir cryptopp
```
```
cd cryptopp
```
```
wget http://www.cryptopp.com/cryptopp562.zip
```
```
unzip -a cryptopp562.zip
```
#### Open GNUmakefile 

```
sudo nano GNUmakefile
```
#### Add -Wno-narrowing to the end of the first line of GNUmakefile 

```
-Wno-narrowing
```

#### Compile the cryptopp 
```
make static dynamic test
```
```
sudo make install
```

#### Edit makefile.unix for the Raspberry Pi

```
cd ..
```
```
nano makefile.unix
```

#### Go down to line 21 and change Boost Library Path to 
```
BOOST_LIB_PATH = /usr/lib/arm-linux-gnueabihf/
```

#### Go down to line 64 and change Lib Miniupnpc Path to 
```
/usr/lib/arm-linux-gnueabihf/libminiupnpc.a
```

#### Compile Maxcoind 
```
make -f makefile.unix
```







