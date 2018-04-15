# Truffle Tutorial 3: Debug

That's a run through of the [Debug Tutorial][debugt] on the Truffle main site.
[debugt]:http://truffleframework.com/tutorials/debugging-a-smart-contract

_If you go through the commits in this repo one by one you will see step by step how this
tutorial progresses, and you can follow the progress in this file below._


## Progress

### Running `truffle init`

    root@eth1:~/TUTORIALS/3_debug# truffle init
    Downloading...
    Unpacking...
    Setting up...
    Unbox successful. Sweet!

    Commands:

    Compile:        truffle compile
    Migrate:        truffle migrate
    Test contracts: truffle test


### Running `truffle compile`

    root@eth1:~/TUTORIALS/3_debug# truffle compile
    Compiling ./contracts/Migrations.sol...
    Writing artifacts to ./build/contracts

### Adding network code

Adding the network code for `ganache-cli` to `truffle.js`

    networks: {
      development: {
        host: "127.0.0.1",
        port: 8545,     // ganache-cli
        network_id: "*" // Match any network id
      }
    }

### Deploying to ganache-cli

Run `ganache-cli` in one terminal window

    root@eth1:~/TUTORIALS/3_debug# ganache-cli
    Ganache CLI v6.1.0 (ganache-core: 2.1.0)

    Available Accounts
    ==================
    (0) 0x019d976cf020a53eaf86af8bd633433026c07d1f
    (1) 0x55959f123dc04e75684ffbd7c142f1f088d9e9a2
    ...
    Private Keys
    ==================
    (0) b322dccf68b3756c701edd758606502aa904af373eea69a7208b424e79124075
    (1) b0f859cef76537726b21cab4e2896d5ca72353e6c2f20879c81a3d5ed1c194a0
    ...

    HD Wallet
    ==================
    Mnemonic:      anxiety congress swap party category feel stove spend mixture glad express leopard
    Base HD Path:  m/44'/60'/0'/0/{account_index}

    Listening on localhost:8545

Run `truffle deploy` in another terminal window

    root@eth1:~/TUTORIALS/3_debug# truffle deploy
    Using network 'development'.

    Network up to date.

The ganache window shows

    Listening on localhost:8545
    net_version
    eth_accounts

### Adding the SimpleStorage smart contract

Adding a new contract file `SimpleStorage.sol`

    pragma solidity ^0.4.17;

    contract SimpleStorage {
      uint myVariable;

      function set(uint x) public {
        myVariable = x;
      }

      function get() constant public returns (uint) {
        return myVariable;
      }
    }

and a new migration file `2_deploy_contracts.js`

    var SimpleStorage = artifacts.require("SimpleStorage");

    module.exports = function(deployer) {
      deployer.deploy(SimpleStorage);
    };


### Running `truffle compile`

    root@eth1:~/TUTORIALS/3_debug# truffle compile
    Compiling ./contracts/SimpleStorage.sol...
    Writing artifacts to ./build/contracts


### Running `truffle deploy`

    root@eth1:~/TUTORIALS/3_debug# truffle deploy
    Using network 'development'.

    Running migration: 1_initial_migration.js
      Deploying Migrations...
      ... 0x9b53b8064c1d7e5494a4fc30a7ef0bfd940adaf22fcb54bc2893644ccc074381
      Migrations: 0x60a5851d25076c3d0fd064302d92937cbb33851a
    Saving successful migration to network...
      ... 0x0421336c69e018dc382929a6ae7ebb48ea12ebdc4fb072555a9ca6c2fdc6d961
    Saving artifacts...
    Running migration: 2_deploy_contracts.js
      Deploying SimpleStorage...
      ... 0x8b59475fccf535055d764bb26f081ba6823a4e67b758c6a18d440f5d2ec72313
      SimpleStorage: 0x81dd6c970d6e368ce06835bce8fd4011fdd4d289
    Saving successful migration to network...
      ... 0xb84e7a12b8a46773f8a0c3e264e38c7660e07bd50048f6257acc8a37a29b20ab
    Saving artifacts...
