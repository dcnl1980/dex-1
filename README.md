# Setup 

* Fork and clone the repository

* [Install NVM](https://github.com/creationix/nvm)

* Install and enable Xcode Command Line Tools

```
xcode-select --install
sudo xcode-select --switch /Library/Developer/CommandLineTools
```

* Switch to Node version specified in .nvmrc

```
nvm install
```

* Terminal Tab 1 - Install Truffle
  * Reference: http://truffleframework.com/

```
npm install -g truffle
```

* Terminal Tab 2 - Install Test Framework with Ethereum TestRPC

```
npm install -g ganache-cli
```

* Terminal Tab 2 - Start Ethereum Blockchain Protocol Node Simulation

```
ganache-cli \
  --port="8500" \
  --mnemonic "copy obey episode awake damp vacant protect hold wish primary travel shy" \
  --verbose \
  --networkId=3 \
  --gasLimit=7984452 \
  --gasPrice=2000000000;
```

* Optionally [Install Geth](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum) and run the Testnet using Geth in the project directory:

  * Show installation directory of Geth and Go, and show Go path [Reference](https://github.com/ethereum/go-ethereum/wiki/Developers%27-Guide)

```
which geth
which go
echo $GOPATH
geth version
```

  * Show where Geth Chain directories are stored:

```
find ~ -type d -name 'chaindata'
```

  * [Install or Upgrade existing version of Geth](https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Mac) (if not installed using Homebrew)

```
brew tap ethereum/ethereum
brew install ethereum
brew upgrade ethereum
```

* Terminal Tab 1 - Compile and Deploy the FixedSupplyToken Contract

```
truffle migrate --network development
```

* Terminal Tab 1 - Run Sample Unit Tests on the Truffle Contract. Truffle Re-Deploys the Contracts

```
truffle test
```

* Terminal Tab 1 - Run Dapp front-end in "Development" using Ganache CLI

  * Open Chrome browser
  * Enable Metamask from Chrome Extensions
  * Click the Metamask icon. Click "Restore from seed phrase".
  * Enter Wallet Seed of "copy obey episode awake damp vacant protect hold wish primary travel shy" to restore vault (matches the seed generated by Ganache CLI).
  * Enter Password "testtest".
  * Select Custom RPC from the Metamast drop-down and enter http://localhost:8500 (instead of the default port of 8545).
  * Select http://localhost:8500 from the Metamask drop-down to connect to Ganache CLI.
  * Run the Dapp front-end with:

```
yarn run start
```

  * Check there are no errors in the Chrome Inspector Console or in the Bash Terminal

* TODO - Terminal Tab 1 - Build Dapp for "Production"

```
yarn run build
```

# Debugging

* Debug the Solidity Smart Contract in Remix IDE
* Verify the Solidity Smart Contract compiles by pasting it in MIST using https://github.com/ltfschoen/geth-node
* Verify the Solidity Smart Contract compiles by deploying it to Ethereum TestRPC using Truffle

# TODO

* [ ] - Incorporate Automated Market Maker (AMM) similar to that described in 0x Whitepaper

# Initial Setup - Truffle, TestRPC, Unit Tests (FixedSupplyContract)

* Install Truffle
  * Reference: http://truffleframework.com/

```
npm install -g truffle
```

* Setup Truffle to Manage Contracts (i.e. MetaCoin sample), Migrations and Unit Tests

```
truffle init

truffle migrate --network development
```

* Initialise with Front-End
  * Truffle Default
    * https://github.com/trufflesuite/truffle-init-default
  * Truffle Webpack
    * https://github.com/trufflesuite/truffle-init-webpack
    * https://github.com/truffle-box/webpack-box
  * Truffle React
    * http://truffleframework.com/boxes/
  * Truffle Drizzle
    * http://truffleframework.com/docs/drizzle/getting-started

    * Created a new `truffle unbox drizzle` project and copied the relevant files to the existing project and fixed errors

* Truffle Configuration File Examples
  * http://truffleframework.com/docs/advanced/configuration
  * Note: Use `from` to specify the From Address for Truffle Contract Deployment.
  * Use the Mnemonic to Restart TestRPC with the same Accounts.
  * Note: Use `provider` to specify a Web3 Provider
  * Note: Truffle Build `truffle build` script for say Webpack is usually in package.json

* Truffle with Test Framework using Ethereum TestRPC (avoid delays in mining transactions with Geth Testnet)
  * Ethereum TestRPC - In-Memory Blockchain Simulation

```
npm install -g ganache-cli
```

  * Start Ethereum Blockchain Protocol Node Simulation with 10x Accounts on http://localhost:8500
    * Creates 10x Private Keys and provides a Mnemonic. Assigns to each associated Address 100 Ether.
    * Note: Private Keys may be imported into Metamask Client 
    * Note: Mnemonic may be used subsequently with Ethereum TestRPC to re-create the Accounts with `

```
ganache-cli
```

  * Restart TestRPC with Same Accounts (i.e. `ganache-cli --mnemonic "copy obey episode awake damp vacant protect hold wish primary travel shy"`)

```
ganache-cli --port 8500 --mnemonic <INSERT_MNEMONIC> 
```

* Add FixedSupplyToken to Truffle Contracts folder
* Remove MetaCoin from Truffle Contracts folder
* Update 2nd Migration file to deploy FixedSupplyToken

* Compile and Deploy the FixedSupplyToken Contract

```
truffle migrate --network development
```

* Run Sample Unit Tests on the Truffle MetaCoin Contract. Truffle Re-Deploys the MetaCoin Contracts

```
truffle test
```

# Troubleshooting

* Try restarting Ganache TestRPC if you encounter error `sender doesn't have enough funds to send tx. The upfront cost is: x and the sender's account only has: y`

* Fix error `Error: Error: Exceeds block gas limit` that may occur when sending Gas Limit say of `50000000` when truffle.js has `gas` property set as `gas: 4712388,`, by changing to a smaller value: `myExchangeInstance.buyToken("FIXED", web3.toWei(4, "finney"), 5, {from: accounts[0], gas: 4000000});`

* Fix `Error: VM Exception while processing transaction: out of gas`. In the `buyToken` function it always occurs after a certain line of code. Simply increase the Gas Limit to the Mainnet's limit (currently shown as `7984452` at https://ethstats.net/) in both Ganache CLI Flags and in truffle.js

* Chrome Inspector Console error when try connect to http://localhost:8500 Custom RPC in Metamask when truffle.s and local Ganache CLI is running without a network id set

```
Injected web3 detected.
drizzle.js Error retrieving network ID:
drizzle.js TypeError: Cannot read property 'address' of undefined
    at http://localhost:3000/static/js/bundle.js:69225:103
From previous event:
    at new DrizzleContract (http://localhost:3000/static/js/bundle.js:69224:27)
    at Drizzle.getContracts
```

  * Tried `console.log(web3)` and `web3.currentProvider` successfully but still returns error.

  * Note that it momentarily worked but was showing the following error

```
Error: The current provider doesn't support subscriptions: MetamaskInpageProvider
```

  * **UNRESOLVED ISSUE**