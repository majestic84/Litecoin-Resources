# LTC-Lightning-Network-lnd-Guide

This is a step-by-step main net tutorial on how to setup a Lightning Network node for Litecoin on Windows. This guide was written by [Michael Rodriguez](https://www.reddit.com/user/majestic84) of [Zulu Republic](https://www.zulurepublic.io).

### Step 1. Install Litecoin Core

Go to https://litecoin.org/#download and download the Windows Litecoin installer for [32 bit](https://download.litecoin.org/litecoin-0.16.3/win/litecoin-0.16.3-win32-setup.exe) or [64 bit](https://download.litecoin.org/litecoin-0.16.3/win/litecoin-0.16.3-win64-setup.exe), depending on your system.

Run the installer and install to the default location.

Create a text file named "litecoin.conf" in %AppData%\Litecoin with the following content (be sure to replace the rpcuser and rpcpassword with your own values):

```
rpcuser=<username>
rpcpassword=<secure_password>
txindex=1
server=1
daemon=1
zmqpubrawblock=tcp://127.0.0.1:28332
zmqpubrawtx=tcp://127.0.0.1:28333
Open Litecoin Core from the Start Menu.
```

Litecoin Core will then begin to download the blockchain, which can take a few hours to a few days. You can continue with the next few steps while the node syncs.

### Step 2. Install LND

Go to https://github.com/lightningnetwork/lnd/releases and download LND for Windows ([32 bit](https://github.com/lightningnetwork/lnd/releases/download/v0.5.1-beta/lnd-windows-386-v0.5.1-beta.zip) or [64 bit](https://github.com/lightningnetwork/lnd/releases/download/v0.5.1-beta/lnd-windows-amd64-v0.5.1-beta.zip)).

Browse to %LocalAppData% and create a directory named "Lnd" and within it create a directory named "bin" (%LocalAppData%\Lnd\bin)

Copy the two files lncli.exe and lnd.exe from the zip file to it.

Create a text file named "lnd.conf" in %LocalAppData%\Lnd with the following content (be sure to replace alias, rpcuser, and rpcpass with your own values):

```
debuglevel=info
alias=<your_node_name>
maxpendingchannels=10
datadir=$LOCALAPPDATA/Lnd/data
rpclisten=127.0.0.1:10009
restlisten=127.0.0.1:10010
litecoin.mainnet=1
litecoin.active=1
litecoin.node=litecoind
litecoind.rpchost=localhost
litecoind.rpcuser=<username_from_litecoin.conf>
litecoind.rpcpass=<password_from_litecoin.conf>
litecoind.zmqpubrawblock=tcp://127.0.0.1:28332
litecoind.zmqpubrawtx=tcp://127.0.0.1:28333
```

### Step 3. Open ports

Open Windows Firewall then click "Advanced settings" and select "Inbound rules".

On the right-hand side click "New Rule". Select "Port" and click "Next". Enter 9333 in the "Specific local ports" section, then hit "Next" three times. Give your rule a name and hit "Finish".

Repeat the above steps for ports 9735, 10009, and 10010.

### Step 4. Start LND

Open a command prompt and enter:

    %LocalAppData%\Lnd\bin\lnd.exe

Open a second command prompt and enter:

	%LocalAppData%\Lnd\bin\lncli.exe create
	
Then enter a password of your choice and confirm it.

Type "n" when prompted "Do you have an existing cipher seed mnemonic you want to use?" and press Enter.

Press Enter again when prompted to "Input your passphrase if you wish to encrypt it".

Close all command prompts.

Open a new command prompt and enter:

    %LocalAppData%\Lnd\bin\lnd.exe
    
Open a second command prompt and enter:

    "%LocalAppData%\Lnd\bin\lncli.exe" unlock
    
Enter the wallet password you provided earlier to unlock the wallet.

Now allow some time for lnd to catch up to the current blockheight. You can keep track of its progress in the first command prompt (the one running lnd.exe).

### Step 5. Profit!

Now that you have lnd up and running, you can continue ecurrencyhodler’s guide for instructions on using it: https://gist.github.com/ecurrencyhodler/f6da7f26110c875e7fa41a91c66b72a1#create-and-fund-a-segwit-address