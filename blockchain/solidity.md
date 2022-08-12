# Solidity

## Concept
### Calldata vs Memory
* Both are transient data. Not persistent between function calls.
* Calldata is immutable, think of it as defined by the caller
* Memory is mutable, defined by the callee

### Storage
* In a function call, mark a variable as storage usually means intending to persist the change in blockchain
* Persistent across function calls
* Most expensive

### External vs Public
* External is more efficient, it allows EVM to directly read from the function args as given, without copying them into memory
* External is suited when passing in large array
* If do not expect to call the method elsewhere in contract, use external. Otherwise use public
* External function can be called from inside using `this.f()` but it's stupid to waste gas

### Internal (default) vs Private
* Internal: can be called by self and subclass contracts
* Private: cannot be called even by subclass contracts

```
pragma solidity ^0.7.4;

contract ExternalPublicTest {
    function test(uint[20] memory a) public pure returns (uint){
         return a[10]*2;
    }

    function test2(uint[20] calldata a) external pure returns (uint){
         return a[10]*2;
    }    
}
```

| | Call from Outside | Call from Inside     |
| : ------ | :------ | :------ |
| Public   | Y       | Y       |
| External | Y       | N       |
| Internal | N       | Y       |
| Private  | N       | Y       |

### Pure vs View
* Pure: do simple calculation, does not read anything from chain or modify state
  - NO: reading state variable
  - NO: access `this.balance` of `<address>.balance`
  - NO: access `block`, `tx`, `msg`
  - NO: calling non-pure function
  - NO: Using inline assembly that contains certain opcodes

## Security
* For `view` function, do not rely on `require(msg.sender) ==` to check identity. User can set sender however they want
* `msg.sender` is only properly set in *transaction*

## Gas
* All transaction costs 21000 gas at minimum (no interaction with any smart contract).
* Gas is fixed per operation. Think of a car goes "33 mile/gallon". The cost of 1 gallon to move 33 miles is fixed.
* Gas price fluctuates per market demand-supply. Just like gas price displayed in gas station.
* check market: https://ethgasstation.info
* on-chain storage is prohibitively expensive: https://hackernoon.com/ether-purchase-power-df40a38c5a2f

## Internal Transaction
* Not actual transactions
* Not recorded on blockchain
* Initiated by contracts function, either by `address.send()` or `address.call()`
