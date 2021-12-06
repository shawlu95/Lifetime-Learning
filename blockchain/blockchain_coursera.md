# Blockchain Basics

### Defining Blockchain
* enable peer-2-peer transfer of asset without intermediary (central authority)
* poised to revolutionize:
  - supply chain good transfer
  - digital media transfer
  - remote service delivery
  - decentralized business logic
  - distributed intelligence (education credential)
  - distribute resource (power grid)
  - crowd funding/operation
  - identity management
  - government public records

### Bitcoin Whitepaper
* whitepaper: [Bitcoin: A Peer-to-Peer Electronic Cash System](https://bitcoin.org/bitcoin.pdf)
* establish peer-2-peer trust: validation, verify, confirm (consensus) -> immutable recording on distributed ledger
  - seller: transactions are computationally impractical to reverse
  - buyers: routine "escrow mechanism"
* credit card uses centralized authority: credit card agency, customer bank, credit card bank, exchange, merchant bank
  - disputes are unavoidable, incur mediation costs, which discourages small amount transaction
  - cannot make "non-reversible transaction" for non-reversible service
* rely on cryptographic proof instead of "trust"
  - compute hash of block header and nounce
  - if hash < F(difficulty): puzzle solved, broadcast block
  - otherwise keep changing nounce value
* the chain is secure as long as "honest nodes collectively control more CPU power"
* construction of a block transaction:
  - payer input: hash of previous block
  - payer output: payee public key
  - payee input: payer hash (built from payee public key)
  - payee output: sign the block using its private key, hash with next payee's public key

![alt-text](assets/blockchain.png)
* maintain an average speed of transaction per hour. proof-of-work difficulty increases if tph increases
* fault tolerance:
  - new block does not need to reach all nodes, longest chain is accepted as truth
  - if a node misses a block, it will re-request it when it receive next block
* there's new coin issued at constant rate as if mining gold mine
* when coin limit is reached, incentive for mining is transaction fees only
* if attacker amasses more CPU than the entire network, he finds it more profitable to play by the rules
  - with more CPU power, attacker can only modify block of his own corrupted blocks, to take his CPU & electricity money back
  - honest nodes will refuse to work on invalid blocks
* transactions are hashed in Merkle tree.
  - only root node contains the block's hash
  - old blocks can be compacted by removing branches (keeping root only). aka block headers
* reclaiming disk space:
> Once the latest transaction in a coin is buried under enough blocks, the spent transactions before it can be discarded to save disk space.

* privacy: public key anonymous
  - a **new** key pair should be used for each transaction

### Blockchain Structure
Block chains are doubly linked list.

UTXO
* unspent transaction output
* components:
  1. ID of the transaction that produced the UTXO
  2. index of the UTXO in the transaction output
  3. value

Transaction
* ID of current transaction
* references >= 1 input UTXO
* references >= 1 output UTXO
* total input & output amount

Transaction Zero
* first transaction of each block
  - is for paying the miner fees.
  - does not have any input UTXO.
  - is called the coinbase transaction

### Extension
* bitcoin supports "script for conditional transfer of value" -> Ethereum expands this into smart contract
* three types emerges:
  - coin only: BTC
  - currency + business: ETH
  - only business logic: Linux foundation's hyper ledger

## Ethereum
* smart contract allows embedding any business logic
  - must be deterministic
  - have their own address, from which to send and receive ether
  - can track caller, tell whether it's admin account
  - can read data from Etherum blockchain
* solidity is a high-level specialized language for smart contract compiled into binary
  - similar to javascript, loosely typed
  - index for multi-dimension array is in reversed order
  - online IDE: Remix
  - local IDE: [Truffle[(https://github.com/trufflesuite/truffle)]
    * [ganache-cli](https://github.com/trufflesuite/ganache-cli-archive)
    * kill process: `kill $(lsof -t -i:8545)`
  - The `msg` variable (together with `tx` and `block`) is a special global variable that contains properties which allow access to the blockchain
  - For solidty v0.5 the constant modifier for function was deprecated, either use pure or view.
  - visibility
    * external: only other contracts can call it
    * public: can be called everywhere
    * internal (default): current contract and subclass contracts can call
      - subclass a contract: `contract sub is super {...}`
    * private: only current contract
  - non-state changing modifier:
    - `view`: public vars are automatically view func
    - `pure`: only compute something, not return anything
  - memory:
    * `memory`: not persisted, string is array of bytes
    * `storage`: persisted
* block chain as a service
  - infura.io
  - alchemy.com
  - look up chain id at `chainlist.org network`
* every node (Ethereum Virtual Machines) should be able to execute contract code
  - only access to outside data is through **oracle** (there are company specialized in this)
  - keep subroutine, data structure simple (incur gas cost)
  - once deployed, code cannot be changed, but *data can be changed*
  - oracle uses decentralized network: `chain.data.link`
    * https://github.com/smartcontractkit/chainlink
* ABI: application binary interface
  - define how solidty and other languages can interact with contracts
* add a layer of logic to the trust infra supported by blockchain
* two types of account:
  - externally owned accounts (EOA): controlled by private key
  - contract accounts: activated only by EOA; represents a smart contract
* one ether = 10^18 wei
* start gas: max # of gas
* gas price: fee for computation
* memory-based proof-of-work
* meta mask is most popular wallet
* incentive:
  - every action requires gas
  - gas cost does not vary. fixed for every type of operation
  - if fees specified in transaction is not sufficient, transaction is rejected. Must be sufficient account balance
  - gas limit: the amount of gas point available
  - gas spent: actual amount spent on each block
  - winner miner: 3 ether base fees + transaction fees
    * other miners don't win: ommers, create ommer blocks (added as side block to the main chain), receive consolation fees

#### [brownie](https://github.com/eth-brownie/brownie): built on web 3. used by yearn, badger etc
- add my real account (from metamask) to brownie: `brownie accounts new {account_name}`
- `brownie accounts list`
- `brownie accounts delete Bailey`
- `brownie console`
* add network to brownie: `brownie networks add Ethereum ganache-local host=http://0.0.0.0:8545 chainid=1337`
* run test on local, not live chain ()
* fork: copy a live blockchain to local and work on it

```bash
brownie networks add development mainnet-fork-dev cmd=ganache-cli host=http://127.0.0.1 fork-'https://mainnet.infura.io/v3/$WEB3_INFURA_PROJECT_ID' accounts=10 mnemonic=brownie port=8545

brownie networks add development mainnet-fork-dev cmd=ganache-cli host=http://127.0.0.1 fork=https://eth-mainnet.alchemyapi.io/v2/OKS7Wnp_0U98Iz9sPbtFGGU6FzdqHGRc accounts=10 mnemonic=brownie port=8545
```

```bash
Launching 'ganache-cli --port 8545 --gasLimit 12000000 --accounts 10 --hardfork istanbul --mnemonic brownie'...
Brownie environment is ready.
>>> SimpleStorage
[]
>>> account=accounts[0]
>>> len(accounts)
10
>>> simple_storage = SimpleStorage.deploy({"from": account})
Transaction sent: 0xa25637614c1b33eab32dcea27441010406db2770379cc6dc020f802d813b7976
  Gas price: 0.0 gwei   Gas limit: 12000000   Nonce: 0
  SimpleStorage.constructor confirmed   Block: 1   Gas used: 420320 (3.50%)
  SimpleStorage deployed at: 0x3194cBDC3dbcd3E11a07892e7bA5c3394048Cc87

>>> simple_storage
<SimpleStorage Contract '0x3194cBDC3dbcd3E11a07892e7bA5c3394048Cc87'>
>>> len(SimpleStorage)
1
>>> simple_storage = SimpleStorage.deploy({"from": account})
Transaction sent: 0x313e9e5bce6f4d23ce795a7d0b46f4238ab5139436945e107cacb8c316310cc3
  Gas price: 0.0 gwei   Gas limit: 12000000   Nonce: 1
  SimpleStorage.constructor confirmed   Block: 2   Gas used: 420320 (3.50%)
  SimpleStorage deployed at: 0x602C71e4DAC47a042Ee7f46E0aee17F94A3bA0B6

>>> len(SimpleStorage)
2
>>> simple_storage.retrieve()
0
>>> simple_storage.store(15, {"from":account})
Transaction sent: 0x8e15adad8f35d811132d3034ee089c2f8b991df5be1125f80fe3e3df51baaec4
  Gas price: 0.0 gwei   Gas limit: 12000000   Nonce: 2
  SimpleStorage.store confirmed   Block: 3   Gas used: 41517 (0.35%)

<Transaction '0x8e15adad8f35d811132d3034ee089c2f8b991df5be1125f80fe3e3df51baaec4'>
>>> simple_storage.retrieve()
15
>>>
```

## Cryptography Basics
* for symmetric crypto, it's easy to derive private key from encrypted data
* Rivest Shamir Adelman (RSA) Algorithm: public-private key, not strong, efficient enough for blockchain
* Elliptic Curve Cryptography (ECC): used in BTC, ETH
  - stronger than RSA for same number of bits
* Keccak 256 is a commonly used algorithm for hash generation in Ethereum blockchain

### Transaction Integrity
* 256 bit random number -> private key
* ECC applied to private key -> becomes public key
* hash public key -> generate account address (20 bytes)

## Decentralized System
1. validate transaction
2. check resources (e.g. fees)
3. execute transaction
4. proof of work consensus
5. solve puzzle and broadcast block to network

### Consensus Protocol
* every block added contributes to trust

### Robustness
Double spending:
* more than one miner mines a block (solved consensus puzzle) at same time
  - BTC: both branches are built forward, the longer chain is kept as main chain. Transaction from the other chain is returned into unconfirmed pool
  - ETH: ommers receive a small incentive for runner-up blocks, maintained for 6 more blocks
* transaction referring to same asset
  - BTC: only keep the first transaction referring to the digital asset
  - ETH: account number and global nounce are used; global nounce is incremented

### Forks
* Soft fork is like software patch: Bitcoin introduces P2SH conditional payment script feature
* Hard fork: emerging two chains are **incompatible**
* ETH changes Homstead to Metropolis-Byzantium (planned)
  - parallel processing of transaction
  - every 100 block, proof-of-stake is applied to evaluate ledger
  - miner incentive reduced from 5 to 3 ETH
* Managing exceptional situation: Ethereum 2018 hard fork at 4.7 million blocks after DAO attack

____
### Block Chain Basics [[Link]]
#### Module 1
* [https://www.pcmag.com/news/blockchain-the-invisible-technology-thats-changing-the-world]
* Unspent Transaction Outputs (UTXO) [[Link](https://smithandcrown.com/glossary/unspent-transaction-outputs-utxo/)]
* Bitcoin Cash Pools: The Majority of Bitcoin SV Blocks Are Mined By ‘Unknown’ [Yes, Really] [[Link](https://www.ccn.com/bitcoin-cash-pools-the-majority-of-bitcoin-sv-blocks-are-mined-by-unknown-yes-really/)]
* How does the Blockchain Work? (Part 1) [[Link](https://medium.com/blockchain-review/how-does-the-blockchain-work-for-dummies-explained-simply-9f94d386e093)]
* How Does the Blockchain Work? [[Link](https://onezero.medium.com/how-does-the-blockchain-work-98c8cd01d2ae)]
* How Do Bitcoin Nodes Verify Transactions? [[Link](https://smartereum.com/8970/how-do-bitcoin-nodes-verify-transactions/)]
* A Gentle Introduction to Blockchain Technology [[Link](https://bitsonblocks.net/2015/09/09/a-gentle-introduction-to-blockchain-technology/)]
* On Public and Private Blockchains [[Link](https://blog.ethereum.org/2015/08/07/on-public-and-private-blockchains/)]
* What is Cryptocurrency. Guide for Beginners [[Link](https://cointelegraph.com/bitcoin-for-beginners/what-are-cryptocurrencies#accept-as-payment-for-business)]
* 2017 Was Bitcoin's Year. 2018 Will Be Ethereum's [[Link](https://www.coindesk.com/2017-bitcoins-year-2018-will-ethereums/)]
* What is Cryptocurrency: Everything You Need To Know  [[Link](https://blockgeeks.com/guides/what-is-cryptocurrency/)]
* What is the Difference Between Public and Permissioned Blockchains? [[Link](https://www.coindesk.com/information/what-is-the-difference-between-open-and-permissioned-blockchains/)]
* Blockchain [[Link](https://blockchain.info/)]
* Bitcoin Block Explorer [[Link](https://blockexplorer.com/)]
* Etherscan [[Link](https://etherscan.io/)]

#### Module 2
* What is Ethereum? [[Link](http://ethdocs.org/en/latest/introduction/what-is-ethereum.html)]
* Smart Contracts: The Blockchain Technology That Will Replace Lawyers [[Link](https://blockgeeks.com/guides/smart-contracts/)]
* Introduction to Smart Contract [[Link](http://solidity.readthedocs.io/en/develop/introduction-to-smart-contracts.html)]
* Ethereum Whitepaper: A Next-Generation Smart Contract and Decentralized Application Platform [[Link](https://ethereum.org/en/whitepaper/)]
* Account Management [[Link](http://ethdocs.org/en/latest/account-management.html)]
* Native: Account management [[Link](https://geth.ethereum.org/docs/dapp/native-accounts)]
* What Is Meant By The Term “Gas”? [[Link](https://ethereum.stackexchange.com/questions/3/what-is-meant-by-the-term-gas)]
* Vitalik Buterin Doubles Down on Ethereum Incentive Strategy [[Link](https://www.coindesk.com/vitalik-buterin-doubles-ethereum-incentive-strategy/)]
* Ether [[Link](https://www.ethereum.org/ether)]
* Proof of Work vs Proof of Stake: Basic Mining Guide [[Link](https://blockgeeks.com/guides/proof-of-work-vs-proof-of-stake/)]
* Etherscan [[Link](https://etherscan.io/)]

#### Module 3
* What Is Public-Key Cryptography? [[Link](https://www.globalsign.com/en/ssl-information-center/what-is-public-key-cryptography/)]
* Asymmetric Cryptography (Public-Key Cryptography) [[Link](http://searchsecurity.techtarget.com/definition/asymmetric-cryptography)]
* Public Key Cryptography - Computerphile [[Link](https://www.youtube.com/watch?v=GSIDS_lvRv4)]
* Basic Intro to Elliptic Curve Cryptography [[Link](https://qvault.io/cryptography/elliptic-curve-cryptography/)]
* What Is Hashing? Under The Hood of Blockchain [[Link](https://blockgeeks.com/guides/what-is-hashing/)]
* SHA: Secure Hashing Algorithm - Computerphile [[Link](https://www.youtube.com/watch?v=DMtFhACPnTY)]
* Hash Functions [[Link](https://www.cs.hmc.edu/~geoff/classes/hmc.cs070.200101/homework10/hashfuncs.html)]
* Hashing demo [[Link](https://anders.com/blockchain/hash)]
* How Safe Are Blockchains? It Depends. [[Link](https://hbr.org/2017/03/how-safe-are-blockchains-it-depends)]
* Blockchains: Embedding Integrity [[Link](https://infospectives.co.uk/blockchains-embedding-integrity/)]
* Securing the Chain [[Link](https://assets.kpmg/content/dam/kpmg/xx/pdf/2017/05/securing-the-chain.pdf)]
* Is It Chain of Headers Rather Than a Chain of Blocks? [[Link](https://bitcoin.stackexchange.com/questions/35448/is-it-chain-of-headers-rather-than-a-chain-of-blocks)]
* What is a Block Header in Bitcoin? [[Link](https://www.cryptocompare.com/coins/guides/what-is-a-block-header-in-bitcoin/)]

#### Module 4
* Blockchain Based Trust & Authentication For Decentralized Sensor Networks [[Link](https://arxiv.org/pdf/1706.01730.pdf)]
* How the Blockchain will Radically Transform the Economy [[Link](https://www.ted.com/talks/bettina_warburg_how_the_blockchain_will_radically_transform_the_economy?utm_campaign=tedspread--b&utm_medium=referral&utm_source=tedcomshare)]
* A (Short) Guide to Blockchain Consensus Protocols [[Link](https://www.coindesk.com/short-guide-blockchain-consensus-protocols/)]
* Review of blockchain consensus mechanisms [[Link](https://medium.com/wavesprotocol/review-of-blockchain-consensus-mechanisms-f575afae38f2)]
* Blockchain Expert Explains One Concept in 5 Levels of Difficulty | WIRED [[Link](https://www.youtube.com/watch?v=hYip_Vuv8J0)]
* How The Blockchain Is Redefining Trust [[Link](https://www.wired.com/story/how-the-blockchain-is-redefining-trust/)]
* Have Blockchain Forks Shown Hayek to be Right or Wrong? [[Link](http://www.trustnodes.com/2017/12/02/blockchain-forks-shown-hayek-right-wrong)]
* Split on Forks? Blockchain Leaders Learn Tough Lessons from Bitcoin Scaling [[Link](https://www.coindesk.com/split-forks-blockchain-leaders-learn-tough-lessons-bitcoin-scaling/)]
*  Bitcoin, Blockchain Forks & Lightning [[Link](https://www.youtube.com/watch?v=8uF7RVF2osk)]

___
### Smart Contract [[Link](https://www.coursera.org/learn/smarter-contracts/lecture/ZAf5T/smart-contract-basics-why-smart-contracts)]
* Smart Contract: Building blocks for digital markets [[Link](http://www.fon.hum.uva.nl/rob/Courses/InformationInSpeech/CDROM/Literature/LOTwinterschool2006/szabo.best.vwh.net/smart_contracts_2.html)]
* How to Learn Solidity: The Ultimate Ethereum Coding Guide [[Link](https://blockgeeks.com/guides/solidity/)]
* Remix- Solidity IDE [[Link](http://remix.readthedocs.io/en/latest/)]
* Decentralized Applications [[Link](https://www.coursera.org/learn/decentralized-apps-on-blockchain)]
* Blockchain Platforms [[Link](https://www.coursera.org/learn/blockchain-platforms)]
* Structure of a Contract [[Link](http://solidity.readthedocs.io/en/develop/structure-of-a-contract.html)]
* Camel Case [[Link](https://en.wikipedia.org/wiki/Camel_case)]
* Introduction to Smart Contracts [[Link](http://solidity.readthedocs.io/en/develop/introduction-to-smart-contracts.html)]
* Account Types, Gas, and Transactions [[Link](http://ethdocs.org/en/latest/contracts-and-transactions/account-types-gas-and-transactions.html)]
* Ethereum, Tokens, and Smart Contracts [[Link](https://medium.com/@k3no/ethereum-tokens-smart-contracts-80f639f5c46b)]
* Decoding the Enigma of Bitcoin Mining [[Link](https://medium.com/all-things-ledger/decoding-the-enigma-of-bitcoin-mining-f8b2697bc4e2)]

#### Module 2
* What is Solidity? Our Guide to the Language of Ethereum Smart Contracts [[Link]()]
* Intro to Solidity 2017 Edition [[Link](https://www.youtube.com/watch?v=KkN1O8TChbM)]
* Ethereum Smart Contracts In Solidity 1 - State, Functions, Modifiers and Events [[Link](https://www.youtube.com/watch?v=xWKq86PWG0o)]
* Solidity Frequently Asked Questions [[Link](https://solidity.readthedocs.io/en/v0.4.24/frequently-asked-questions.html)]
* Types [[Link](http://solidity.readthedocs.io/en/develop/types.html)]
* Learning Solidity : Tutorial 6 Data Types (Array, Mapping, Struct) [[Link](https://www.youtube.com/watch?v=8UhO3IKApSg)]
* Units and Globally Available Variables [[Link](http://solidity.readthedocs.io/en/develop/units-and-global-variables.html)]
* Solidity Tutorials [[Link](https://ethereumbuilders.gitbooks.io/guide/content/en/solidity_tutorials.html)]
* Types [[Link](http://solidity.readthedocs.io/en/develop/types.html)]
* Enums [[Link](http://solidity.readthedocs.io/en/develop/types.html#enums)]
* Liquid Democracy uses Blockchain to Fix Politics, and Now You Can Vote for It [[Link](https://beta.techcrunch.com/2018/02/24/liquid-democracy-uses-blockchain/)]
* Contracts [[Link](http://solidity.readthedocs.io/en/develop/contracts.html)]

#### Module 3
* Voting [[Link](https://soliditycookbook.com/voting/)]
* Enum [[Link](https://solidity.readthedocs.io/en/develop/types.html#enums)]
* Time Units [[Link](https://solidity.readthedocs.io/en/develop/units-and-global-variables.html#time-units)]
* Solidity Learning: Revert(), Assert(), and Require() in Solidity, and the New REVERT Opcode in the EVM [[Link](https://medium.com/blockchannel/the-use-of-revert-assert-and-require-in-solidity-and-the-new-revert-opcode-in-the-evm-1a3a7990e06e)]
* Technical Introduction to Events and Logs in Ethereum [[Link](https://media.consensys.net/technical-introduction-to-events-and-logs-in-ethereum-a074d65dd61e)]
* Capturing Smart Contract Events in our User Interface (Solidity) [[Link](https://www.youtube.com/watch?v=L5Au5DY8eL4)]

#### Module 4
* [ ] Ethereum Smart Contract Security Best Practices [[link](https://consensys.github.io/smart-contract-best-practices/)]
* [ ] Beyond Smart Contract Best Practices for UX and Interoperability [[link](https://medium.com/@maurelian/beyond-smart-contract-best-practices-for-ux-and-interoperability-6d94d27c1e0f)]
* [ ] Solidity Smart Contract Security Best Practices [[link](https://lightrains.com/blogs/smart-contract-best-practices-solidity)]
* [ ] [[link]()]
* [ ] [[link]()]
* [ ] [[link]()]
