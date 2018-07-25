#Guide to Solo Mining

###Getting Started
Fresh install of Ubuntu and run updates: 

```$ sudo apt-get update```

Then install the dependencies:

<code>$ sudo apt-get install build-essential pkg-config libgtest-dev libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python zlib1g-dev wget bsdmainutils automake</code>

Then fetch the Litcoin repository with git and run fetch-params.sh like so:

```
$ git clone https://github.com/LitcoinOfficial/Litcoin
$ cd Litcoin/
$ git checkout origin
$ ./zcutil/fetch-params.sh```


This will fetch the public beta proving and verifying keys and place them into ~/.litcoin-params/. These keys are, together, nearly 1.5GB in size, so it may take some time to download them.

###Compiling

####Check GCC version
gcc/g++ 4.9 or later is required. Litcoin has been successfully built using gcc/g++ versions 4.9 to 7.x inclusive. Use g++ --version to check which version you have.

On Ubuntu Trusty, if your version is too old then you can install gcc/g++ 4.9 as follows:
Ensure you have successfully installed all system package dependencies as described above. Then run the build:
```
$ sudo add-apt-repository ppa:ubuntu-toolchain-r/test
$ sudo apt-get update
$ sudo apt-get install g++-4.9```


Ensure you have successfully installed all system package dependencies as described above. Then run the build:

```$ ./zcutil/build.sh -j$(nproc)```

This should compile the dependencies and build zcashd.
