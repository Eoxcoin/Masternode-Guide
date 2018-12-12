# Masternode-Guide
A complete Masternode Guide for EOX Masternode setup on Ubuntu VPS 18.04

Use the following instructions to setup a masternode on Ubuntu Server 18.04.

Make sure that you have the following requirements.

- Required amount of coins to setup the masternode. 
- A wallet to store your coins. 
- A server or VPS.

The instructions are split in three sections.


Setup the control wallet (1/2)
Open your wallet and wait until the wallet has downloaded the complete blockchain.

Go to “Tools”. 
Click “Debug console”. 
This is the console where you will execute all commands.

Create a masternode private key.

"masternode genkey"

Example output :-

"75eqvNfaEfkd3YTwQ3hMwyxL2BgNSrqHDgWc6jbUh4Gdtnro2Wo"

Show your collateral address.

getaccountaddress "MN1"

Example output

"Nad4xtgdwf7c5y45ruy5MWtVY43zYMCvva"

Keep note of the masternode private key and the collateral address in note pad.


Setup the VPS
Install Ubuntu Server 18.04 on a VPS. (We prefer you to use Vultr VPS)

Update your Ubuntu machine. (Copy and paste this code in the root)

sudo apt-get update
sudo apt-get upgrade

Install the required dependencies. (Copy and paste this code in the root)

sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils python3 libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-test-dev libboost-thread-dev libboost-all-dev libboost-program-options-dev
sudo apt-get install libminiupnpc-dev libzmq3-dev libprotobuf-dev protobuf-compiler unzip

Install Berkeley DB from source code. (Copy and paste this code in the root)

wget https://download.oracle.com/berkeley-db/db-4.8.30.zip
unzip db-4.8.30.zip
cd db-4.8.30
cd build_unix/
../dist/configure


Download the daemon from Eox Coin

wget https://github.com/Eoxcoin/Wallets/releases/download/1.0/eox-daemon-linux.tar.gz" -O eoxd-daemon-linux.tar.gz

Extract the tar file.

tar -xzvf eoxd-daemon-linux.tar.gz

Install the daemon.

chmod +x eoxd
sudo mv eoxd /usr/bin/

Create the config file.

mkdir $HOME/.eox
nano $HOME/.eox/eox.conf

Paste the following lines in eox.conf.

#----
rpcuser=rpc_eox
rpcpassword=kuw05sqio7bcm8z96o7redv17xws1lw6xpd1qf33
rpcallowip=127.0.0.1
#----
listen=1
server=1
daemon=1
maxconnections=64
#----
masternode=1
masternodeprivkey= REPLACE_WITH_YOUR_MASTERNODE_PRIVATE_KEY
externalip=REPLACE_WITH_YOUR_EXTERNAL_IP_OF_YOUR_VULTR_VPS
#----

Replace the text “REPLACE_WITH_MASTERNODE_PRIVATE_KEY” with the “masternode private key” that you created using the command “masternode genkey”. 

E.G. masternodeprivkey=75eqvNfaEfkd3YTwQ3hMwyxL2BgNSrqHDgWc6jbUh4Gdtnro2Wo

Replace the text “REPLACE_WITH_EXTERNAL_IP_OF_VPS” with the external IP address of your VPS. 

E.G. externalip=136.144.171.201

Start your node with the following command.

eoxd

Setup the control wallet (2/2)
Transfer the required amount of coins to the “collateral address” that you created using the command “getaccountaddress 0”.

Wait until the transaction has the required masternode confirmations.

Go to Tools. 
Click Debug console.

Enter the following command.

masternode outputs

Example output

{ "06e38868bb8f9958e34d5155437d009b72dff33fc28874c87fd42e51c0f74fdb" : "0", } 

Go to “Tools”. 
Click “Open Masternode Configuration File”.

Modify the following line and paste it into notepad:-

MN1 136.144.171.201:9999 75eqvNfaEfkd3YTwQ3hMwyxL2BgNSrqHDgWc6jbUh4Gdtnro2Wo 06e38868bb8f9958e34d5155437d009b72dff33fc28874c87fd42e51c0f74fdb 0

MN1 - Alias for your masternode.

136.144.171.201 - External IP of your VPS.

9999 - Masternode port.

75eqvNfaEfkd3YTwQ3hMwyxL2BgNSrqHDgWc6jbUh4Gdtnro2Wo - Masternode private key from the command “masternode genkey”.

06e38868bb8f9958e34d5155437d009b72dff33fc28874c87fd42e51c0f74fdb - Transaction hash from the command “masternode outputs”.

0 - Single digit from the command “masternode outputs”.

Save the file and close notepad.

Shutdown your wallet and re-open your wallet.

Go to “Settings”. 
Click “Unlock Wallet”.

Enter your wallet passphrase and unlock your wallet.

Go to “Tools”. 
Click “Debug console”.

Start your masternode using the command.

masternode start-alias MN1

It will take +/- 30 minutes to activate your masternode.
