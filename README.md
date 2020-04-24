# SLP++
SLP++ Layer II protocol, Safety & Simple base on original  [SLP.](https://github.com/simpleledger/slp-specifications)  

### SLP++ electronic contract protocol
[slp++-contract](./slppp-contract.md)

### SLP++ onchain message protocol
[slp++-message](./slppp-message.md)

### SLP++-token protocol
[slp++-token-type-1](./slppp-token-type-1.md) compared to original SLP, SLP++ has below advantages:
```
1. Safety：avoid the chance of spent utxo which contains tokens.
2. Simple: one Transaction outputs attached one or more op_return, no addidtional rules for old op_return style.
3. Free order of Transaction outputs.
```
