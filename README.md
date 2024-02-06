# Jump-start for Tezos hackathons

Here's some information to help new developers get started hacking with Tezos.
This is not a comprehensive reference for all of Tezos; it's just an overview.
For links to complete reference information, see [References](#references).

## Overview: What is Tezos?

Tezos is an open-source, decentralized blockchain created in 2014 by Arthur and Kathleen Breitman.




## What can I do with Tezos?

You can do lots of things with Tezos, but here are some ideas:

- **Manage tokens**: Tezos manages digital assets called _tokens_.
Tokens can represent anything that you want them to represent and behave in any way that you program them to behave.

- **Run programs**: Tezos is a platform for deploying programs called _smart contracts_, which behave like APIs.
You can deploy smart contracts to Tezos and not worry about where they are running, and users can call them from many different clients.

- **Accept payments**: Using Tezos to manage online payments is much easier than accepting credit cards because Tezos and wallet applications handle all of the security for you.

- **Run games**: The Tezos SDK for Unity lets you use Tezos as a backend for games, keeping track of players' inventories and allowing them to work with their items in and out of your games.
You can also use Tezos to handle logic and security for games.
For an example, see the tutorial [Create a mobile game](https://docs.tezos.com/tutorials/mobile).

## What are tokens and NFTs?

The main cryptocurrency of Tezos is called _tez_, which is sometimes referred to as ꜩ or as XTZ.
Tezos users use tez to pay transaction fees and as a method of payment and exchange.

Tezos also

## Diagram

The following diagram shows the major components of Tezos:

- **Nodes**: The backbone of Tezos is a network of computers that run the Tezos protocol.
Anyone can run a node and no one is directly in charge of the nodes, which makes the system resilient and decentralized.

  The nodes run the Tezos protocol that any user can call or deploy programs called smart contracts to, which makes the system behave like a globally-distributed operating system.
  Nodes have an [RPC interface](https://docs.tezos.com/architecture/rpc) that allows clients to call them, though not all nodes make their RPC interface public.

  The nodes work closely with **bakers** and **accusers**, which package the transactions that users and other clients send into blocks to go on the blockchain.

- **Blockchain**: The blockchain itself is a record of transactions and other operations that happen on Tezos.
Aside from metadata, each block contains transactions from users and other clients.
The main types of transactions that hackathon users are interested in are:

  - Transferring of tez from one account to another
  - Creating other kinds of tokens
  - Transferring of other kinds of tokens from one account to another
  - Deploying smart contracts
  - Calling smart contracts

  The diagram shows **smart contracts** as part of the blockchain because their code and storage is stored in the blocks.

- **Indexers**: Indexers are applications that retrieve blockchain data, process it, and store it in a way that makes it easier to search and use.

- **Clients**: Many different clients can access Tezos for information, to initiate token transfers, and to call smart contracts.

![Overview diagram of Tezos](./images/architecture-overview.png)

## Decentralized applications (dApps)

Applications that use Tezos are called Decentralized applications (dApps) because they run not on only one computer, but on the network of Tezos nodes all around the world.
This makes them resilient, so you don't have to worry about when, where, or whether your programs will run.

dApps usually have two parts:

- **Frontend**: An ordinary web, desktop, or mobile app; this is referred to as the _off-chain_ part because it runs on ordinary web or application hosting, not on Tezos itself
- **Backend**: One or more [smart contracts](./smart-contracts); they are referred to as the _on-chain_ part because they run on Tezos itself

dApps can also have middleware in the form of an [indexer](https://docs.tezos.com/developing/information/indexers) to interpret the backend information and provide it in a more convenient format for the frontend component.

Here's a diagram of how these parts work together:

![Overview diagram of Web and mobile apps calling smart contracts and getting information from indexers](./images/dapp-overview.png)

## How do I access Tezos?

The most popular ways to access Tezos are:

- [The Octez client](#the-octez-client)
- [The Taquito SDK for JavaScript and TypeScript](#the-taquito-sdk-for-javascript-and-typescript)
- [The Tezos SDK for Unity](#the-tezos-sdk-for-unity)

### The Octez client

The Octez client is a command-line client for Tezos.
You can use it either through installing it locally or by using the [tezos/tezos](https://hub.docker.com/r/tezos/tezos) Docker image.
To install it locally, see [Installing the Octez client](https://docs.tezos.com/developing/octez-client/installing).
For setup and configuration information, see [Setting up the client](https://tezos.gitlab.io/user/setup-client.html) in the Octez documentation.

Here are some common commands for the octez-client:

- `octez-client man [search term]`: Search for a command
- `octez-client gen keys my_account`: Create an account with the alias "my_account" in the client's local wallet
- `octez-client get balance for my_account`: Get the tez balance of the account with the alias "my_account"
- `octez-client show address my_account`: Print the address for the alias "my_account"
- `octez-client list known addresses`: Print information about the accounts in the client's local wallet
- `octez-client --wait none transfer 1 from my_account to other_account --burn-cap 0.1`: Transfer 1 tez from "my_account" to "other_account" and accept a fee as high as 0.1 tez

Deploying a smart contract is called "origination" and it requires the source code of the contract in Michelson and the initial value of the contract storage.
For example, this code originates a contract from the source code file `my_new_contract_source.tz` and initializes its storage to "hello":

```bash
octez-client originate contract my_new_contract \
  transferring 0 from my_account \
  running my_new_contract_source.tz \
  --init 'hello' --burn-cap 0.5 --force
```

The `my_new_contract` in the first line is the local alias for the contract, so you can call it using that alias instead of the full address.
For example, if the contract has an entrypoint named "doSomething" that accepts an integer, you can call it with a command that looks like this:

```bash
octez-client --wait none transfer 0 from my_account \
  to my_new_contract --entrypoint "doSomething" \
  --arg '122' --burn-cap 0.1
```

### The Taquito SDK for JavaScript and TypeScript

The Taquito SDK for JavaScript and TypeScript lets you connect to user wallets and send transactions to Tezos.

For an introduction to Taquito, see https://docs.tezos.com/dApps/taquito.

For complete Taquito documentation, see https://tezostaquito.io.

For tutorials that use Taquito, see:

- https://docs.tezos.com/tutorials/build-your-first-app
- https://docs.tezos.com/tutorials/create-an-nft/nft-web-app

For starter Taquito applications, see https://github.com/trilitech/tutorial-applications

### The Tezos SDK for Unity

You can install the SDK for Unity to call Tezos from Unity games, including locally-installed games, mobile apps on Android and iOS, and WebGL-based games on web pages.
The SDK allows you to work with Tezos via C# code that backs objects in the Unity editor.

The SDK has a quickstart here: https://docs.tezos.com/unity/quickstart

## Using test networks and faucets

To avoid working with real cryptocurrency on Tezos Mainnet, you can use the test network named Ghostnet.
Its faucet provides free tez tokens so you can deploy contracts and work with them at no cost.

For information about Ghostnet, see https://teztnets.com/ghostnet-about.

The Ghostnet faucet is at https://faucet.ghostnet.teztnets.com.

To use test networks, you have to point your client or SDK to an RPC endpoint for that network instead of for Tezos Mainnet.
RPC nodes are like API servers that you can send Tezos transactions to.
The [Ghostnet page](https://teztnets.com/ghostnet-about) has a list of public RPC endpoints that you can use.

- The online IDEs for LIGO and SmartPy have settings for Ghostnet.
- To set the Octez client to use Ghostnet, pass the RPC endpoint that you want to use, as in this example: `octez-client -E https://rpc.ghostnet.teztnets.com config init`
- To set Taquito to use Ghostnet, create an instance of Taquito with the RPC endpoint, as in this example:

   ```javascript
   import { TezosToolkit } from "@taquito/taquito";
   const rpcUrl = "https://ghostnet.ecadinfra.com";
   const Tezos = new TezosToolkit(rpcUrl);
   ```
- The Unity SDK uses Ghostnet by default.

## Funding a wallet

To interact with most dApps, you need a wallet, which stores your private key and allows you to approve Tezos transactions from dApps.
Wallets are available as browser plugins, mobile apps, and social apps.
For information about getting a wallet, switching it to Ghostnet, and funding it from the faucet, see https://docs.tezos.com/developing/wallet-setup.

## Writing smart contracts

Tezos smart contacts are pieces of code stored on the blockchain.
After they are deployed, they are immutable and cannot be shut down or prevented from running.

A smart contract is composed of three elements:

- Its balance: a contract is a kind of account, and can receive and send tez
- Its [storage](https://docs.tezos.com/smart-contracts/storage): data that is dedicated to and can be read and written by the contract
- Its code: one or more [entrypoints](https://docs.tezos.com/smart-contracts/entrypoints), which are a kind of function that can be called either from outside the chain or from other contracts

You can think of a smart contract as a single independent application with its own storage and an API that consumers can call.
However, smart contracts have some limitations.

All a contract can do can be summarized as:

- Performing computations
- Reading or updating the value of its own storage
- Generating a list of operations, such as calls to other contracts (see [Operations](https://docs.tezos.com/smart-contracts/logic/operations))

Smart contracts can't do these things:

- Access programs outside the blockchain, including calling external APIs
- Access other contracts' storage
- Change their code
- Catch and respond to errors (see [Handling errors](https://docs.tezos.com/smart-contracts/logic/errors))

### Smart contract languages

You can write Tezos smart contracts in any of these languages:

- SmartPy, which has a syntax similar to Python
- LIGO, which has versions with syntaxes similar to JavaScript/TypeScript and OCaml
- Archetype, which is a high-level language developed specifically for Tezos

Each of these languages are eventually compiled to Michelson, the base language for Tezos smart contracts.
Michelson is a stack-based language and is hard to read, so most people use one of the high-level languages instead.

For a tutorial that covers writing a basic smart contract in each of these languages, see https://docs.tezos.com/tutorials/smart-contract.

#### SmartPy

[SmartPy](https://smartpy.io/) is a version of Python that you can use to write smart contracts.
You can use it to write, test, and deploy smart contracts in two basic ways:

- You can use the online IDE at https://smartpy.io/ide
- You can write contracts in your own IDE and then use the SmartPy command-line tool to test and compile them; see https://smartpy.io/manual/introduction/installation

Here's a simple smart contract in SmartPy.
It stores a string and allows callers to change that string.
It provides an entrypoint named `replace` that replaces the string with the string that the caller sends, and an entrypoint named `append` that adds the string to the string that the caller sends.
Indentation is significant in Python, so be careful to keep it the same if you copy this code:

```python
import smartpy as sp

@sp.module
def main():
    class StoreGreeting(sp.Contract):
        def __init__(self, greeting): # Note the indentation
            self.data.greeting = greeting

        @sp.entrypoint # Note the indentation
        def replace(self, params):
            self.data.greeting = params.text

        @sp.entrypoint # Note the indentation
        def append(self, params):
            self.data.greeting += params.text

@sp.add_test(name = "StoreGreeting")
def test():
    scenario = sp.test_scenario(main)
    scenario.h1("StoreGreeting")

    contract = main.StoreGreeting("Hello")
    scenario += contract

    scenario.verify(contract.data.greeting == "Hello")

    contract.replace(text = "Hi")
    contract.append(text = ", there!")
    scenario.verify(contract.data.greeting == "Hi, there!")
```

You can test, compile, and deploy this contract from the online IDE or use these commands to test, compile, and deploy it locally:

```bash
./smartpy test store_greeting.py store_greeting/

cd store_greeting/StoreGreeting

octez-client originate contract storeGreeting \
  transferring 0 from my_account \
  running step_002_cont_0_contract.tz \
  --init '"Hello"' --burn-cap 0.1
```

Now you can call the contract by running this command:

```bash
octez-client --wait none transfer 0 from my_account \
  to storeGreeting --entrypoint 'replace' \
  --arg '"Hi there!"' --burn-cap 0.1
```

#### LIGO

[LIGO](https://ligolang.org/) is a language that you can use to write smart contracts.
It has a version called CameLIGO that works like OCaml and a version called JsLIGO that works like TypeScript.
You can use it to write, test, and deploy smart contracts in two basic ways:

- You can use the online IDE at https://ide.ligolang.org/
- You can write contracts in your own IDE and then use the LIGO command-line tool to test and compile them; see https://ligolang.org/docs/intro/installation?lang=jsligo

Here's a simple smart contract in JsLIGO.
It stores a number and allows callers to change it.
It provides three entrypoints:

- The `increment` endpoint accepts an integer as a parameter and adds that integer to the value in storage
- The `decrement` endpoint accepts an integer as a parameter and subtracts that integer to the value in storage
- The `reset` endpoint takes no parameters and resets the value in storage to 0

Each entrypoint returns a list of other Tezos transactions to run (in these cases, an empty list) and the updated value of the contract's storage.

```ts
namespace Counter {
  type storage = int;
  type returnValue = [list<operation>, storage];

  // Increment entrypoint
  @entry
  const increment = (delta : int, store : storage) : returnValue =>
    [list([]), store + delta];

  // Decrement entrypoint
  @entry
  const decrement = (delta : int, store : storage) : returnValue =>
    [list([]), store - delta];

  // Reset entrypoint
  @entry
  const reset = (_p : unit, _s : storage) : returnValue =>
    [list([]), 0];
}
```

You can test, compile, and deploy this contract from the online IDE or use these commands to test, compile, and deploy it locally:

```bash
ligo run dry-run counter.jsligo -m Counter "Increment(32)" "10"

ligo compile contract counter.jsligo -m Counter -o increment.tz

octez-client originate contract counter \
    transferring 0 from my_account \
    running counter.tz \
    --init 10 --burn-cap 0.1 --force
```

Now you can call the contract by running this command:

```bash
octez-client --wait none transfer 0 from my_account \
  to counter --entrypoint 'increment' \
  --arg '5' --burn-cap 0.1
```


#### Archetype

TODO

## Getting information about Tezos

- RPCs
- Block explorers
- Indexers






## Getting help

- https://discord.gg/yXaPy6s5Nr
- https://www.reddit.com/r/tezos
- https://tezos.stackexchange.com

For more community resources, see https://tezos.com/community.

## References

- Developer information: https://docs.tezos.com
- Documentation for the Octez suite, mostly about managing nodes and bakers: https://tezos.gitlab.io/
- https://opentezos.com
