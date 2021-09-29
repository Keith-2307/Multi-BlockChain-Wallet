# Multi-BlockChain-Wallet
Assignment 19
![Picture1](https://user-images.githubusercontent.com/83662813/135195510-a4093426-2ac7-46d9-981d-e05572a16915.jpg)  

## Background ##

My new startup is focusing on building a portfolio management system that supports not only traditional assets like gold, silver, stocks, etc, but crypto-assets as well! The problem is, there are so many coins out there! It's a good thing I understand how HD wallets work, since I will need to build out a system that can create them.  

I am in a race to get to the market. There aren't as many tools available in Python for this sort of thing, yet. Thankfully, I've found a command line tool, hd-wallet-derive that supports not only BIP32, BIP39, and BIP44, but also supports non-standard derivation paths for the most popular wallets out there today! However, I need to integrate the script into my backend with my dear old friend, Python.  

Once I've integrated this "universal" wallet, I can begin to manage billions of addresses across 300+ coins, giving me a serious edge against the competition.  

In this assignment, however, I will only need to get 2 coins working: Ethereum and Bitcoin Testnet. Ethereum keys are the same format on any network, so the Ethereum keys should work with my custom networks or testnets.  

As an exercise, I invite you to follow along, using the instructions below to install the dependencies and wallets.

## Dependencies ##

The following dependencies are required for this assignment and were likely already installed as part of your preparation for in-class activities.  

**Important**: If you have not already installed the dependencies listed below, you may do so by following the instructions found in the following guides:
- [HD Wallet Derive Installation Guide](https://utoronto.bootcampcontent.com/utoronto-bootcamp/utor-tor-fin-pt-05-2021-u-c/-/blob/master/19-Blockchain-Python/Unit%2019%20Homework/Resources/HD_Wallet_Derive_Install_Guide.md)
- [Blockchain TX Installation Guide.](https://utoronto.bootcampcontent.com/utoronto-bootcamp/utor-tor-fin-pt-05-2021-u-c/-/blob/master/19-Blockchain-Python/Unit%2019%20Homework/Resources/Blockchain_TX_Install_Guide.md)

**Dependencies List:**
•  PHP must be installed on your operating system.

•  You will need to clone the [hd-wallet-derive tool](https://github.com/dan-da/hd-wallet-derive).

•  [bit](https://ofek.github.io/bit/) Python Bitcoin library.

•  [web3.py](https://github.com/ethereum/web3.py) Python Ethereum library.

**Instructions:**
1.	Project setup

- Create a project directory called wallet and cd into it.

 - Clone the hd-wallet-derive tool into this folder and install it using the [HD Wallet Derive Installation Guide](https://utoronto.bootcampcontent.com/utoronto-bootcamp/utor-tor-fin-pt-05-2021-u-c/-/blob/master/19-Blockchain-Python/Unit%2019%20Homework/Resources/HD_Wallet_Derive_Install_Guide.md)

- Create a symlink called derive for the hd-wallet-derive/hd-wallet-derive.php script. This will clean up the command needed to run the script in our code, as we can call ./derive instead of ./hd-wallet-derive/hd-wallet-derive.php:

-	Make sure you are in the top level project directory - in this case the directory named wallet.

- Mac Users: Run the following command: ln -s hd-wallet-derive/hd-wallet-derive.php derive.

- Windows Users: Creating symlinks is not supported by default on Windows, only reading them, so Windows users must perform the following steps:
o	Open up Git-Bash as an administrator (right-click on Git-Bash in the start menu).
o	Within bash, run the command export MSYS=winsymlinks:nativestrict.
o	Run the following command: ln -s hd-wallet-derive/hd-wallet-derive.php derive.

-	Test that you can run the ./derive script properly, by running the following command.

./derive --key=xprv9zbB6Xchu2zRkf6jSEnH9vuy7tpBuq2njDRr9efSGBXSYr1QtN8QHRur28QLQvKRqFThCxopdS1UD61a5q6jGyuJPGLDV9XfYHQto72DAE8 --cols=path,address --coin=ZEC --numderive=3 -g

- The output should match what you see below:

o	+------+-------------------------------------+

o	| path | address   

o	+------+-------------------------------------+

o	| m/0  | t1V1Qp41kbHn159hvVXZL5M1MmVDRe6EdpA 

o	| m/1  | t1Tw6iqFY1g9dKeAqPDAncaUjha8cn9SZqX

o	| m/2  | t1VGTPzBSSYd27GF8p9rGKGdFuWekKRhug4

o	+------+-------------------------------------+

-  Create a file called wallet.py -- this will be your universal wallet script. You can use [this starter code](https://utoronto.bootcampcontent.com/utoronto-bootcamp/utor-tor-fin-pt-05-2021-u-c/-/blob/master/19-Blockchain-Python/Unit%2019%20Homework/Starter-Code/wallet.py) as a starting point.

- Your directory tree should look something like this:
<img width="390" alt="Picture2" src="https://user-images.githubusercontent.com/83662813/135197595-0c55b013-f750-4d0a-a262-e762422b30f9.png">

2. Setup constants

•	In a separate file, constants.py, set the following constants:
  o	BTC = 'btc'
  
  o	ETH = 'eth'
  
  o	BTCTEST = 'btc-test'
  
•	In wallet.py, import all constants: from constants import *

•	Use these anytime you reference these strings, both in function calls, and in setting object keys.

3. Generate a Mnemonic

•	Generate a new 12 word mnemonic using hd-wallet-derive or by using [this tool](https://iancoleman.io/bip39/).
•	Set this mnemonic as an environment variable by storing it a an .env file and importing it into your wallet.py.

4. Derive the wallet keys

•	Create a function called derive_wallets that does the following:
  o	Use the subprocess library to create a shell command that calls the ./derive script from Python. Make sure to properly wait for the process. **Windows Users** may need to prepend the php command in front of ./derive like so: php derive.
  o	The following flags must be passed into the shell command as variables:
  - Mnemonic (--mnemonic) must be set from an environment variable, or default to a test  mnemonic
  - Coin (--coin)
  - Numderive (--numderive) to set number of child keys generated
  - Format (--format=json) to parse the output into a JSON object using json.loads(output) 

•	Create a dictionary object called coins that uses the derive_wallets function to derive ETH and BTCTEST wallets.

•	When done properly, the final object should look something like this (there are only 3 children each in this image):

**BTC-TEST and ETHEREUM (ETH)**
![Picture3](https://user-images.githubusercontent.com/83662813/135198164-99d72064-2791-439e-81c2-464320dcee92.gif)

•	You should now be able to select child accounts (and thus, private keys) by accessing items in the coins dictionary like so: coins[COINTYPE][INDEX]['privkey'].

5. Linking the transaction signing libraries

•	Use bit and web3.py to leverage the keys stored in the coins object by creating three more functions:
      o	priv_key_to_account:
      
 •	This function will convert the privkey string in a child key to an account object that bit or web3.py can use to transact.     
This function needs the following parameters:

  •	coin -- the coin type (defined in constants.py).
  •	priv_key -- the privkey string will be passed through here.

You will need to check the coin type, then return one of the following functions based on the library:

          •	For ETH, return Account.privateKeyToAccount(priv_key) 

This function returns an account object from the private key string. You can read more about this object [here](https://web3js.readthedocs.io/en/v1.2.0/web3-eth-accounts.html#privatekeytoaccount).

•	For BTCTEST, return PrivateKeyTestnet(priv_key) 

This is a function from the bit libarary that converts the private key string into a WIF (Wallet Import Format) object. WIF is a special format bitcoin uses to designate the types of keys it generates.

You can read more about this function [here](https://ofek.dev/bit/dev/api.html).

create_tx:
  •	This function will create the raw, unsigned transaction that contains all metadata needed to transact.
  
  •	This function needs the following parameters:
  
      o	coin -- the coin type (defined in constants.py).
      
      o	account -- the account object from priv_key_to_account.
      
      o	to -- the recipient address.
      
      o	amount -- the amount of the coin to send.
      
•	You will need to check the coin type, then return one of the following functions based on the library:

      o	For ETH, return an object containing to, from, value, gas, gasPrice, nonce, and chainID. Make sure to calculate all of these values properly using web3.py!
      
For BTCTEST, return PrivateKeyTestnet.prepare_transaction(account.address, [(to, amount, BTC)])


send_tx:
  •	This function will call create_tx, sign the transaction, then send it to the designated network.
  
  •	This function needs the following parameters:
  
      o	coin -- the coin type (defined in constants.py).
      
      o	account -- the account object from priv_key_to_account.

      o	to -- the recipient address.
      
      o	amount -- the amount of the coin to send.
      
•	You may notice these are the exact same parameters as create_tx. send_tx will call create_tx, so it needs all of this information available.

•	You will need to check the coin, then create a raw_tx object by calling create_tx. Then, you will need to sign the raw_tx using bit or web3.py (hint: the account objects have a sign transaction function within).

•	Once you've signed the transaction, you will need to send it to the designated blockchain network.

  o	For ETH, return w3.eth.sendRawTransaction(signed.rawTransaction) 
  
  o	For BTCTEST, return NetworkAPI.broadcast_tx_testnet(signed)
  
6. Send some transactions!

•	Now, you should be able to fund these wallets using testnet faucets.

•	Open up a new terminal window inside of wallet.

•	Then run the command python to open the Python shell.

•	Within the Python shell, run the command from wallet import *. This will allow you to access the functions in wallet.py interactively.

•	You'll need to set the account with priv_key_to_account and use send_tx to send transactions.
![Picture4](https://user-images.githubusercontent.com/83662813/135199080-359ecbd8-78f2-4bec-8b66-f1e118651e01.png)

![Picture5](https://user-images.githubusercontent.com/83662813/135199175-6d0522b9-27f5-49d6-9048-c08e8ffc5990.png)

**Bitcoin Testnet transaction**

•	Fund a BTCTEST address using [this testnet faucet.](https://testnet-faucet.mempool.co/)
![Picture6](https://user-images.githubusercontent.com/83662813/135199359-7dc20620-31ca-4a63-b41f-210539f73dfb.png)

•	Use a [block explorer](https://tbtc.bitaps.com/) to watch transactions on the address.

![Picture7](https://user-images.githubusercontent.com/83662813/135199520-31d89027-74a5-4e15-ad28-45ecfe4e8416.gif)

•	Send a transaction to another testnet address (either one of your own, or the faucet's).
![Picture8](https://user-images.githubusercontent.com/83662813/135199639-83f0c1bd-e92c-41cf-9a1a-4ce7e114f53e.png)

•	Screenshot the confirmation of the transaction like so:

**Local PoA Ethereum transaction**

•	Add one of the ETH addresses to the pre-allocated accounts in your networkname.json.

•	Delete the geth folder in each node, then re-initialize using geth --datadir nodeX init networkname.json. This will create a new chain, and will pre-fund the new account.

•	[Add the following middleware](https://web3py.readthedocs.io/en/stable/middleware.html#geth-style-proof-of-authority) to web3.py to support the PoA algorithm:

![Picture9](https://user-images.githubusercontent.com/83662813/135199899-30f4bd5d-faf1-486f-8f6e-1f3d046215e9.png)
![Picture10](https://user-images.githubusercontent.com/83662813/135199965-54bb4bbb-9c09-4224-aef8-a4dcaa8fa465.png)

•	The following slide indicates the transaction sent within the python code.
![Picture11](https://user-images.githubusercontent.com/83662813/135200056-ddd34332-34c4-4dbc-9844-22386b7c97d9.png)

•	[Add the following middleware](https://web3py.readthedocs.io/en/stable/middleware.html#geth-style-proof-of-authority) to web3.py to support the PoA algorithm:

    from web3.middleware import geth_poa_middleware

    w3.middleware_onion.inject(geth_poa_middleware, layer=0)

•  Due to a bug in web3.py, you will need to send a transaction or two with MyCrypto first, since the w3.eth.generateGasPrice() function does not work with an empty chain. You can use one of the ETH address privkey, or one of the node keystore files.

•  Send a transaction from the pre-funded address within the wallet to another, then copy the txid into MyCrypto's TX Status, and screenshot the successful transaction like so:

![Picture12](https://user-images.githubusercontent.com/83662813/135200283-ca275a6a-d590-403e-aa82-1ec64b8291f2.png)

The above screen is still pending due to my system’s functioning capabilities.  The memory and CPU are operating at almost 100% capacity. As a result, there is significant time required for the transaction to be completed and in many cases, the nodes time out providing errors.

![Picture13](https://user-images.githubusercontent.com/83662813/135200363-38de0018-7f2b-4c15-a974-c517f0355177.png)

The following shows the activity on the CPU when running the nodes and MyCrypto Wallet.

![Picture14](https://user-images.githubusercontent.com/83662813/135200454-e1d6b2d2-4335-4597-9357-f3aed6d3cbfd.png)









