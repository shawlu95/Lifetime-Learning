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
* solidity is a high-level specialized language for smart contract compiled into binary
* every node (Ethereum Virtual Machines) should be able to execute contract code
* add a layer of logic to the trust infra supported by blockchain
* two types of account:
  - externally owned accounts (EOA): controlled by private key
  - contract accounts: activated only by EOA; represents a smart contract
* one ether = 10^18 wei
* start gas: max # of gas
* gas price: fee for computation
* memory-based proof-of-work
* incentive:
  - every action requires gas
  - gas cost does not vary. fixed for every type of operation
  - if fees specified in transaction is not sufficient, transaction is rejected. Must be sufficient account balance
  - gas limit: the amount of gas point available
  - gas spent: actual amount spent on each block
  - winner miner: 3 ether base fees + transaction fees
    * other miners don't win: ommers, create ommer blocks (added as side block to the main chain), receive consolation fees

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