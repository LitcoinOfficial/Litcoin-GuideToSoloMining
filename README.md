# Litcoin Solo Mining

## Getting Started

Start with a fresh install of Ubuntu and run updates:

`$ sudo apt-get update`

Then install the dependencies:

```
$ sudo apt-get install build-essential pkg-config libgtest-dev libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python zlib1g-dev wget bsdmainutils automake
```

Then fetch the Litcoin repository with git and run `fetch-params.sh` like so:

```
$ git clone https://github.com/LitcoinOfficial/Litcoin
$ cd Litcoin/
$ git checkout origin
$ ./zcutil/fetch-params.sh
```

This will fetch the public beta proving and verifying keys and place them into ~/.zcash-params/. These keys are, together, nearly 1.5GB in size, so it may take some time to download them.

gcc/g++ 4.9 or later is required. Litcoin has been successfully built using gcc/g++ versions 4.9 to 7.x inclusive. Use g++ --version to check which version you have.

On Ubuntu Trusty, if your version is too old then you can install gcc/g++ 4.9 as follows:

`
$ sudo add-apt-repository ppa:ubuntu-toolchain-r/test
$ sudo apt-get update
$ sudo apt-get install g++-4.9
`

## Compiling

Ensure you have successfully installed all system package dependencies as described above. Then run the build:

`$ ./zcutil/build.sh -j$(nproc)`

This should compile our dependencies and build zcashd. (Note: if you don't have nproc, then substitute the number of cores on your system. If the build runs out of memory, try again without the -j argument, i.e. just ./zcutil/build.sh. )

## Testing

The tests take a while to run but it is encouraged to run them. For the first set of tests run:

`$ ./qa/zcash/full-test-suite.sh`

If you notice any failed tests, please inform the zcash devs.

To run the next set of tests run:

`$ ./qa/pull-tester/rpc-tests.sh`

Again, if you get any failures, please inform the zcash devs.

## Solo Mining Configuration

We need to set up a config file, so first we make a directory for it:

`$ mkdir -p ~/.litcoin`

Next we need to create a config file and give it some configurations. First we create and edit the file:

`$ nano ~/.litcoin/litcoin.conf`

Enter the following in the file:

```
addnode=mainnet.litcoin
rpcuser=rpcusername
rpcpassword=rpcpassword
gen=1
genproclimit=4
equihashsolver=tromp
```
Naturally, you'll want to use a random password. You'll also want to set the `genproclimit` to the number of cores on which you want to mine.
Then save the file (within nano you do this via `CTRL+X` then `Y`).

## Starting the Miner

Now that your config file is set up, you start the zcash server via:

`$ ./src/zcashd -daemon`

Give your server a few seconds to finish loading. You can check to see if it's working via:

`$ ./src/zcash-cli getinfo`

Give your node time to catch up with the blockchain. You can see how many blocks your node has received/checked by running the following command and looking at the line labelled *blocks*:

That's it! You're mining zcash!

## Useful Commands

Now that your miner is running, there are some commands you'll want to familiarize yourself with.

#### Stop/Start Mining

If you want to stop mining use the command:
`$ ./src/zcash-cli setgenerate false`

You can start up again via the command:
`$ ./src/zcash-cli setgenerate true`

It can take a few seconds for your node to startup/stop after issuing these commands. Be patient.

#### See Your Mined Coins

To check if you've mined coins first run the command:

`$ ./src/zcash-cli listtransactions`

This will output a list of *all* transactions associated with your wallet, along with their details. Any transaction in that list that has `"generated" : true` is *coinbase transaction*. Those are coins that you've mined!

Note: It takes some time (100 blocks) for the coinbase transactions to "settle" and show up in your wallet balance. Any coinbase transaction that has `"category" : "immature"` is one that you've successfully mined but hasn't settled yet. 

Be patient, and after 100 blocks this will switch to `"generated" : true` -- at which time those mined coins will show up in your wallet balance and will be spendable.

#### Checking Your Wallet Balance

You can see your balance by running the following command and looking for `"balance": xxxxx`:

`$ ./src/zcash-cli getinfo`

Note that your mined coins won't show up in your balance until they are 100 blocks deep.


