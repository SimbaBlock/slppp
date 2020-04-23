# SLPPP Token Type 1 Protocol Specification
### Specification version: 0.1
### Date published: April 23, 2020

## Authors
Jonald Fyookball, James Cramer, Unwriter, Mark B. Lundeberg, Calin Culianu, Ryan X. Charles, Pisa

## Acknowledgements


# Table of Contents


# SECTION I: BACKGROUND

## Introduction


## Summary


## Use Cases


## Requirements

We believe that a good solution for implementing tokens should have the following properties:

**1. Permissionless**. Its tokens should not require permission to issue or transfer.\
**2. Simple**. The system should be easily understandable and straightforward.\
**3. Robust.** A minimal, unambiguous rule set supports a fault-tolerant consensus layer.\
**4. Non-invasive.** It should require no changes to the underlying Bitcoin Cash protocol.\
**5. SPV friendly.** Users of light wallets should be able to validate their own transactions.\
**6. Extensible.** The system should allow future versions of tokens including tokens with issuer controlled whitelists for regulated securities ([Appendix A](#appendix-a-regulated-security-tokens)), and other needs.\
**7. Supported.** There should be an implementation plan for rapid ecosystem support.

## Comparison to other token schemes


## Design Philosophy and Challenges

Understanding the thinking behind the SLP protocol may assist in its evaluation.

### Simplicity and Consensus


### Why SPV-friendly permissionless tokens are difficult


### Hybrid Security Model



# SECTION II: PROTOCOL DESCRIPTION

## Transaction Detail

### Formatting
### GENESIS - Token Genesis Transaction

This is the first transaction which defines the properties, metadata and initial mint quantity of the token. The token is thereafter uniquely identified by the token genesis transaction hash which is referred to as `token_id`.

`token_type` indicates the SLP sub-protocol:

* 1 - Permissionless Token Type
* 2 - Reserved for Security Token Type (see [Appendix A](#appendix-a-regulated-security-tokens))
* 3 - Reserved for Voting Token Type
* 4 - Reserved for Ticketing Token Type
* ...

This document specifies the rules and operation of the Permissionless Token Type (1) only. Tokens of different types cannot be mixed, and so future specifications of other token types will not affect the consensus validity of type 1.

`mint_baton_vout`: Future token supply increases are made possible if the genesis endows a specific transaction output with a "minting baton" that can be passed along and used for future minting (using 'MINT' transactions, see below). If `mint_baton_vout` is not present or refers to a nonexistent output, then the baton does not exist and the token provably has a one-time issuance.

`decimals`: indicates that 1 token is divisible into 10^`decimals` base units. SLP messages store whole numbers indicating token amounts as measured in the base unit, analogous to how bitcoin transactions store BCH amounts measured in the base unit 'satoshis'. With a token FOO having `decimals` of 6 indicated in the genesis, for example, the quantity 12.53 FOO (as displayed in wallet software) would be represented by 12530000 base units (as 8 bytes, hex 0000000000bf3150). A `decimals` of 8 would give the same divisibility as bitcoin, whereas 0 would give indivisible tokens.

The genesis transaction includes an initial minting of `initial_token_mint_quantity` base units, placed on the second transaction output (vout=1).

**Transaction inputs**: Any number of inputs or content of inputs, in any order.

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>ScriptPubKey ("Address")</b></td>
  <td><b>BSV<br/>amount</b></td>
  <td><b>Implied token amount<br/>(base units)</b></td>
</tr>
  <tr>
    <td>...</td>
   <td>
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
   &lt;lokad_id: 'SLP++\x00'&gt; (6 bytes, ascii)<sup>1</sup><br/>
   &lt;token_type: 1&gt; (1 to 2 byte integer)<br/>
   &lt;admin_type: 'GENESIS'&gt; (7 bytes, ascii)<br/>
   &lt;token_ticker&gt; (0 to ∞ bytes, suggested utf-8)<br/>
   &lt;token_name&gt; (0 to ∞ bytes, suggested utf-8)<br/>
   &lt;token_document_url&gt; (0 to ∞ bytes, suggested ascii)<br/>
   &lt;token_document_hash&gt; (0 bytes or 32 bytes)<br/>
   &lt;decimals&gt; (1 byte in range 0x00-0x09)<br/>
   &lt;mint_baton_vout&gt; (0 bytes, or 1 byte in range 0x00-0xff)<br/>
   &lt;initial_token_mint_quantity&gt; (8 byte integer)
   </td>
    <td>any<sup>2</sup></td>
    <td>0</td>
  </tr>
  
  <tr>
    <td>...</td>
    <td>(n=mint_baton_vout)  
     Mint baton receiver</td>
    <td>any<sup>2</sup></td>
    <td>0 <br/> + 'baton'</td>
  </tr>

  <tr>
    <td>...</td>
    <td>Any</td>
    <td>any<sup>2</sup></td>
    <td>0</td>
  </tr>
 
</table>

<sup>1. The Lokad identifier is registered as the number 0x504c53 (which, when encoded in the 4-byte little-endian format expected for Lokad IDs, gives the ascii string 'SLP\x00'). Inquiries and additional information about the Lokad system of OP_RETURN protocol identifiers can be found at https://github.com/Lokad/Terab maintained by Joannes Vermorel.</sup>

<sup>2. SLP does not impose any restrictions on BCH output amounts. Typically however the OP_RETURN output would have 0 BCH (as any BCH sent would be burned), and outputs receiving tokens / mint batons would be sent only the minimal 'dust' amount of 0.00000546 BCH.</sup>

### MINT - Extended Minting Transaction
#### (used with "baton" to increase supply)

Subsequent minting transactions of `additional_token_quantity` can be performed by spending the "minting baton" UTXO in a special MINT transaction, described here. Note that this could be done by someone other than the GENESIS issuer, if the baton minting authority had been passed to another address.

As with GENESIS, the MINT allows to end the baton, or further pass on the baton to future mint operations: if `mint_baton_vout` is empty or refers to a nonexistent vout, the transaction is valid but the baton is lost. This makes it possible to prove end-of-minting capabilities for a token even after several minting events (it is impossible to duplicate this baton as that would require double-spending the transaction output associated with the baton).

**Transaction inputs**: Any number of inputs or content of inputs, in any order, but with required presence of a 'baton' input (see Consensus Rules).

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>ScriptPubKey ("Address")</b></td>
  <td><b>BSV<br/>amount</b></td>
  <td><b>Implied token amount<br/>(base units)</b></td>
</tr>
  <tr>
  <td>...</td>
<td>
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
&lt;lokad_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;token_type: 1&gt; (1 to 2 byte integer)<BR>
&lt;admin_type: 'MINT'&gt; (4 bytes, ascii)<BR>
&lt;token_id&gt; (32 bytes)<BR>
&lt;mint_baton_vout&gt; (0 bytes or 1 byte between 0x00-0xff)<BR>
&lt;additional_token_quantity&gt; (8 byte integer)
  </td>
    <td>any</td>
    <td>0</td>
  </tr>

  <tr>
    <td>...</td>
    <td>Mint baton receiver<br/>(n=mint_baton_vout)</td>
    <td>any</td>
    <td>0<br/> + 'baton'</td>
  </tr>
  <tr>
    <td>...</td>
    <td>Any</td>
    <td>any</td>
    <td>0</td>
  </tr>

</table>


### SEND - Spend Transaction
#### (Send / Transfer)
The following transaction format is used to transfer tokens from one or more token holding UTXO(s) to new token holding UTXO(s). The UTXOs associated with unspent tokens will be used within the transaction input and, just like the BCH attached to these UTXOs, will be considered totally spent after this transaction is accepted by the blockchain. Tokens will be assigned to new UTXOs vout=1 up to vout=19 as indicated within the OP_RETURN statement.  Any number of additional BCH-only outputs will be allowed. A BCH-only output can come before token outputs, but a token quantity of 0 must be specified for this output.

**Transaction inputs**: Any number of inputs or content of inputs, in any order, but must include sufficient tokens coming from valid token transactions of matching `token_id`, `token_type` (see Consensus Rules).

**Transaction outputs**:
<table>
  <tr>
    <td><b>v<sub>out</sub></b></td>
    <td><b>ScriptPubKey ("Address")</b></td>
    <td><b>BSV<br/>amount</b></td>
    <td><b>Implied token amount<br/>(base units)</b></td>
  </tr>
  <tr>
    <td>...</td>
    <td>
OP_RETURN: '\x6a' (1 bytes, ascii)<BR>
&lt;lokad id: 'SLP++\x00'&gt; (4 bytes, ascii)<BR/>
&lt;token_type: 1&gt; (1 to 2 byte integer)<BR/>
&lt;token_id&gt; (32 bytes)<BR/>
&lt;token_output_quantity&gt; (<b>required</b>, 8 byte integer)<BR/>
  <td>any</td>
  <td>0</td>
  </tr>
</table>

### FROZEN - Frozen UTXO Transaction


**Transaction outputs**:
<table>
  <tr>
    <td><b>v<sub>out</sub></b></td>
    <td><b>ScriptPubKey ("Address")</b></td>
    <td><b>BSV<br/>amount</b></td>
  </tr>
  <tr>
    <td>...</td>
    <td>
    OP_RETURN: '\x6a' (1 bytes, ascii)><br/>
    &lt;lokad_id: 'SLP++\x00'&gt; (4 bytes, ascii)<br/>
    &lt;token_type: 1&gt; (1 to 2 byte integer)<br/>
    &lt;admin_type: 'FROZEN'&gt; (6 bytes, ascii) <br/>
    &lt;token_id&gt; (32 bytes)<br/>
    &lt;frozen utxo's txid&gt; (32 bytes)<br/>
    &lt;frozen utxo's vout&gt; (1 byte integer)<br/>
  </td>
    <td>any</td>
  </tr>
  <tr>
    <td>...</td>
    <td>Any</td>
    <td>any</td>
  </tr>

</table>


### Examples

**GENESIS Transaction**

xsv blockchain transaction:  a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d87b20d2502682647

SCRIPT: ``006a04534c500001010747454e45534953045553445423546574686572204c74642e20555320646f6c6c6172206261636b656420746f6b656e734168747470733a2f2f7465746865722e746f2f77702d636f6e74656e742f75706c6f6164732f323031362f30362f546574686572576869746550617065722e70646620db4451f11eda33950670aaf59e704da90117ff7057283b032cfaec77793139160108010208002386f26fc10000``

SCRIPT BROKEN DOWN:
<table>
 <tr>
  <td>00</td>
  <td>OP_FALSE</td>
 <tr>
 <tr>
  <td>6a</td>
  <td>OP_RETURN</td>
 <tr>  
 <tr>
  <td>04</td>
  <td>length of lokad_id field (4 bytes)</td>
 <tr>
 <tr>
  <td>534c5000</td>
  <td>SLP\x00</td>
 <tr>
 <tr>
  <td>01</td>
  <td>length of token_type (1 byte)</td>
 <tr>
 <tr>
  <td>01</td>
  <td>token_type (1)</td>
 <tr>
 <tr>
  <td>07</td>
  <td>number of bytes in transaction_type (7 bytes)</td>
 <tr>
 <tr>
  <td>47454e45534953</td>
  <td>'GENESIS'</td>
 <tr>
 <tr>
  <td>04</td>
  <td>length of token_ticker (4 bytes)</td>
 <tr>
 <tr>
  <td>55534454</td>
  <td>'USDT'</td>
 <tr>
 <tr>
  <td>23</td>
  <td>length of token_name (35 bytes)</td>
 <tr>
 <tr>
  <td>
   546574686572204c74642e20555320646f6c6c61722062616<br/>
   36b656420746f6b656e73
  </td>
  <td>'Tether Ltd. US dollar backed tokens'</td>
 <tr>
 <tr>
  <td>41</td>
  <td>length of token_document_url (65 bytes)</td>
 <tr>
 <tr>
  <td>
   68747470733a2f2f7465746865722e746f2f77702d636f6e7<br/>
   4656e742f75706c6f6164732f323031362f30362f54657468<br/>
   6572576869746550617065722e706466
  </td>
  <td>'https://tether.to/wp-content/uploads/2016/06/TetherWhitePaper.pdf'</td>
 <tr>
 <tr>
  <td>20</td>
  <td>length of token_document_hash (32 bytes)</td>
 <tr>
 <tr>
  <td>
   db4451f11eda33950670aaf59e704da90117ff7057283b032<br/>
   cfaec7779313916
  </td>
  <td>token_document_hash</td>
 <tr>
 <tr>
  <td>01</td>
  <td>length for decimals (1 byte)</td>
 <tr>
 <tr>
  <td>08</td>
  <td>decimals (8)</td>
 <tr>
 <tr>
  <td>01</td>
  <td>length for mint_baton_vout (1 byte)</td>
 <tr>
 <tr>
  <td>02</td>
  <td>mint_baton_vout (2)</td>
 <tr>
 <tr>
  <td>08</td>
  <td>length of initial_token_mint_quantity (8 bytes)</td>
 <tr>
 <tr>
  <td>002386f26fc10000</td>
  <td>initial_token_mint_quantity (10,000,000,000,000,000)</td>
 </tr>
</table>

**MINT Transaction**

xsv blockchain transaction: 6b73adfbe7e5688c53ea4b09bf37de85dfd6dd4e3d38d1c0b4a5b38a9c0ca613

SCRIPT: ``006a04534c50000101044d494e5420a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d87b20d2502682647010208002386f26fc10000``

SCRIPT BROKEN DOWN:
<table>
 <tr>
  <td>00</td>
  <td>OP_FALSE</td>
 </tr>
 <tr>
  <td>6a</td>
  <td>OP_RETURN</td>
 </tr>
 <tr>
  <td>04</td>
  <td>Length of lokad_id field (4 bytes)</td>
 </tr>
 <tr>
  <td>534c5000</td>
  <td>SLP\x00</td>
 </tr>
 <tr>
  <td>01</td>
  <td>length of token_type (1 byte)</td>
 </tr>
 <tr>
  <td>01</td>
  <td>token_type (1)</td>
 </tr>
 <tr>
  <td>04</td>
  <td>length of transaction_type field (4 bytes)</td>
 </tr>
 <tr>
  <td>4d494e54</td>
  <td>'MINT'</td>
 </tr>
 <tr>
  <td>20</td>
  <td>length of token_id (32 bytes)</td>
 </tr>
 <tr>
  <td>
   a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d8<br/>
   7b20d2502682647
  </td>
  <td>token_id</td>
 </tr>
 <tr>
  <td>01</td>
  <td>length of mint_baton_vout (1 byte)</td>
 </tr>
 <tr>
  <td>02</td>
  <td>mint_baton_vout (2)</td>
 </tr>
 <tr>
  <td>08</td>
  <td>length of additional_token_quantity (8 bytes)</td>
 </tr>
 <tr>
  <td>002386f26fc10000</td>
  <td>additional_token_quantity (10,000,000,000,000,000)</td>
 </tr>
</table>

**SEND Transaction**

xsv blockchain transaction:  424d6948f7e6311816c27ea4a95418467f2e3f3185fcfba765c273d3aca5fde7

SCRIPT: ``006a04534c500001010453454e4420a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d87b20d25026826470800005af3107a40000800232bff5f46c000``

**FROZEN Transaction**

[to be determined]


# Transaction Validation Security Model

Users have several methods for validating incoming transactions:

1. **Full self-validation.**  The user checks each relevant UTXO input from the current transaction leading all the way back to genesis. This is the most complete and secure method since it captures the complete proof DAG, but also the most burdensome.  Depending on the implementation, wallets could parse transactions individually, or they could request the DAG set of transactions from a server that gives a complete validation.<p>The feasibility of full self-validation is only limited by the size of the transaction graph  (see figure below). It does not require a fully validating Bitcoin node.

2. **Limited self-validation.** The user checks each relevant UTXO, working backward from the current transaction, but does not go all the way back to the genesis token transaction.  This provides validation except for when an attacker creates a longer chain than is being validated for.

3. **Wallet-integrated proxy validation.**  A wallet can query (via API) an infrastructure service such as BitDB to determine if an SLP transaction is valid.

4. **Non-wallet proxy validation.**  Typically, users check their transactions on block explorers.   There is plethora of both token and non-token supporting block explorers in the broader cryptocurrency ecosystem.  BCH token block explorers are expected to flourish.

5. **Checkpoint based validation.**  The user checks each relevant UTXO input from the current transaction leading all the way back to a checkpoint provided by a trusted token token validator using a checksum commitment transaction. This involves a degree of trust in the token’s validator, however its actions are all recorded in the blockchain and attached to it’s identity, so it is easy to exclude a validator that’s been proven fraudulent.

Transactions form a directed acyclic graph (DAG).  Full validation terminates at the token genesis transaction which is a self-evidently valid transaction.

![Transaction DAG](images/image_0.png)

**Figure 1.** An illustration of the SLP token transaction graph for a token where the issuer has used multiple minting transactions to increase token supply. The genesis transaction includes the initial mint transaction. If no "baton" is included with the genesis transaction then future supply increases are not possible.

The validation methods listed above can be used together in various combinations and have a synergistic effect as part of a *trust-but-verify* model which trusts proxies for full validation and verifies with partial self validation.

A good token wallet should always perform some kind of full validation.  This best practice creates a low probability of success for an attack, which discourages attackers --  further increasing the effectiveness of partial validation.  Verifying the information from proxies is possible to any chosen degree, and a client can check with multiple proxies or nodes until he’s convinced the SLP chain is valid.

Additionally, although an attack is not necessarily costly in fees, it may be costly in coordination efforts, because long chains of transactions need to be dispersed across the span of time to avoid being blatantly suspicious.

# SECTION III: FURTHER ANALYSIS

## Wallet Implementation

SPV and/or light wallets such as Electron Cash or the Bitcoin.com wallet can be modified to support tokens in addition to regular BCH.  Several key considerations for such modification are described below.

### Balance and History

Wallets can be modified to incorporate token validation schemes (as discussed above in previous sections) to compute a token balance for each address the wallet manages, in addition to usual BCH balances that a wallet normally displays.

The wallet’s engine will receive a transaction history for the addresses it manages in the usual way, and in addition it will parse any transactions it sees with the relevant OP_RETURN messages (either genesis transactions or send/receive transactions).  It will then apply consensus and validation to filter out invalid "noise" or fake token transactions and collate the valid results appropriately to compute a balance and history for each token that it sees and for each address that it manages.

It should be noted that since anyone can mint any arbitrary new token using SLP, only a certain subset of extant tokens on the blockchain will have any value or be of interest to a particular user.

Optionally, the user should be offered the capability to only view tokens to which they have "subscribed", or which they consider “official” or “of interest” (for example by explicitly specifying the minting transaction hash of a particular token they wish to follow, or perhaps by opting-out of any particular tokens they aren’t interested in monitoring).  Thus, wallet software can be configurable to suppress the display of potential token ‘noise’ for a particular set of wallet addresses.

### Token Safety

The most important principle that every token-wallet implementation must observe is protection against losing tokens.  Tokens are lost when a token-containing UTXO is spent on a non-token transaction (or a wrongly-typed token).

Wallets can use a separate set of key/address pairs, perhaps with a distinct derivation path as previously discussed. The best implementations will even avoid spending outputs with *any* OP_RETURN data, as they could be used for other token schemes or blockchain applications.

Even if wallets maintain a separate set of addresses for SLP, it’s still possible to end up with token-holding UTXOs that also contain significant BCH (or vice-versa). Should the user wish to spend some of that BCH while leaving the token balance unchanged, wallet implementations should be able to handle this by sending change appropriately.

Token spends should be managed by the wallet intelligently so as to minimize the amount of BCH associated with a token UTXO (preferably keeping token UTXOs at or close to the dust limit).

### Funding Transaction Fees

Wallets may find that their token-holding UTXOs do not contain sufficient BCH funds to craft a transaction that spends a token.  When building an implementation, developers need to plan for making available extra funds to pay for fees, while keeping the boundary entact between different types of unspent outputs.

### Sending and Receiving

Wallet software is free to offer any user experience it sees fit for managing tokens. Spending screens can be anything from very simple forms to more complex per-UTXO breakdowns.  This specification leaves it up to the wallet implementor to decide how best to manage their users’ control over tokens.

### Handling multiple token types

Any number of different token_ids can be sent to the same SLP address, so in principle a single address could have thousands of distinct token balances on it. Only the UTXOs are limited to a single token_id.

One suggestion is to create a drop-down selector that allows users to choose the token_id of interest.  Other token_ids are not forgotten, they are just not displayed and their UTXOs are prevented from getting involved.

Validation should be deferred for uninteresting token_ids as it can take a lot of time and bandwidth.

### Token names

Obviously any token creator can choose any name for their token, which makes it trivial to issue "counterfeit" tokens.  Wallets can make use of issuer-chosen metadata as long as it is not solely relied on, and the wallet informs the user to manually verify the hash identifier with a trusted source when adding a new token type to the wallet.

### Validation in Light Wallets

Simple Ledger Protocol has been designed so that the validity judgement of every transaction is strictly a function of its own contents, the contents of its input transactions, and the validity judgements on the input transactions. This allows a straightforward structure for light wallets to construct a directed acyclic graph (DAG) proof for a given transaction. In the graph (multi-graph in fact), transactions are vertices and TxIn entries are directed edges. SLP validity is completely independent of block-chain confirmations or even double spending.<sup>4</sup> Any SLP validation judgement is therefore permanent, since the SLP consensus rules are also permanently fixed.

In principle, DAG proving is as simple as recursively obtaining all ancestor token transactions and then computing validity starting at the deepest transaction ("GENESIS"). Some optimizations on the DAG-proving mechanism have however been made in the reference light wallet implementation (Electron Cash SLP edition).
* Pruning: Whenever it becomes known that a given TxIn provides 0 tokens (previous tx is non-SLP, mismatched token ID, judged invalid, or explicit 0 tokens for that TxOut), the associated edge is prune from the graph. Transactions with known validity judgements also have their TxIn edges pruned.
* Short-cut invalidation: Even when the validity states of inputs are not yet all determined, it is possible to calculate the total sum of *possibly* valid token inputs and compare to the output sum.
* Caching: Future validation requests on different transactions having the same token_id may easily overlap with the DAG of previous validation runs. Various degrees of caching are possible.

This pruning and short circuit invalidation process allows resources (in particular, client bandwidth to ElectrumX server) to be focussed only on those parts of the DAG which are strictly relevant for proof. Caching increases the efficiency for validating multiple token transactions.

The reference implementation uses a breadth-first search, seeking to prioritize transactions based on their 'depth', defined as the length of the shortest non-pruned directed path from root to vertex. If the DAG is too large, such a breadth first search can be truncated at a desired depth at which point proxy results may be used (see [Transaction Validation Security Model](#transaction-validation-security-model)).

<sup>4. Note on double spends: SLP validators should be designed to tolerate double spends (multiple TxIns referring to a single TxOut) as these are technically "SLP valid", even though the bitcoin network should never allow these double spends to appear in the mempool or blockchain.</sup>


## Proxies

To support light wallets, what is needed is a full node with code that watches all "SLP"-tagged transactions in mempool/blocks and classifies them as valid/invalid, as they roll in.

Due to the SLP design, this classification can be done immediately and permanently, as validity is independent of block confirmation / block reorganizations (Only miner confirmations prevent double spends).

Clients can query the proxy database and get a judgement on any given transaction; however it is up to the querier to decide whether or not to trust the judgement. Ideally, The judgement should be cryptographically signed and kept as a way to demonstrate bad behaviour by the proxy.

A more advanced protocol might support users querying for a DAG-proof in one lumped transmission, which lets the users prove validity for themselves in the fastest manner possible. However it remains to be seen whether this places a large CPU / network bandwidth burden onto the service. If it is burdensome, it will probably be a good idea for the token issuers to fund/run such a service.

### Tokengraph

Rather than each having a separate node to support each type of token, it makes sense for there to be "all purpose" nodes that support any token type, in the same way that BitDB network can currently support any kind of OP_RETURN metadata protocol. To implement this system, we can build an infrastructure technology which acts as a virtual state machine that maintains a graph data structure. 

We will call this "Tokengraph". Although not part of the SLP protocol per se, we shall describe here how it operates.  

State machines are made up of a "State storage", an "Input" that triggers a state transition, and a "Transition algorithm" that determines the next state. Here's an overview of how Tokengraph implements these components to build a graph state machine:

Tokengraph utilizes BitDB as "Input", uses an additional graph database as "State storage", and implements Simple Ledger Protocol as its "Transition" algorithm. Whenever there's a new Bitcoin transaction or block, BitDB parses its OP_RETURN into its JSON serialization format as specified in https://bitdb.network and broadcasts it as a zeromq message (Input). The state machine is listening to these events, and processes the events following the SLP rules to decides on the next state (Transition). Lastly, the next state is stored into the graph database, creating an up-to-date global state (State storage). The entire global state at any point can be reconstructed identically just from BitDB and SLP state machine.

Once the global state is constructed this way, it becomes trivial to make flexible queries into the graph to make sense of everything that's happening in the token universe to build various applications on top, including the Proxy API, block explorer, and more. The SLP state transition algorithm is implemented as follows:

1. The state machine maintains a graph data structure made up of two types of vertices ("Address" vertices and "Transaction" vertices) and directed edges between them.
2. When an address makes a SLP transaction, a "transaction" vetex is created.
3. The state machine filters all the SLP-valid inputs from the transaction and creates an "in-edge" from the sender address vertices into the transaction vertex (Many-to-One)
4. The state machine also finds all the SLP-valid outputs from the same transaction and runs it through the SLP state transition algorithm to make sure the state transition is valid. "Out-edges" are created from the transaction vertex into all the SLP-valid output address vertices (One-to-Many).

For example for a "SEND" transaction, it looks at all the "in-edges" into the current transaction vertex, sums up all their available UTXO spendable amount, and compares it with the sum of all the output spend amounts. If the total SLP-valid spend amounts are not larger than the total available UTXO input balance, the "SEND" is considered valid and the corresponding "out edges" are created. This, along with other protocol rules, strictly follows the Simple Ledger Protocol.
  
## Economic Implications

There are several economic-related questions regarding different aspects of the protocol and what they imply economically.  Here are some key conclusions:

1. SLP transactions are carried in Bitcoin transactions and thus do not change economic incentives for their confirmation by miners.

2. Prevalent use of metadata in transactions can be paid for using the standard miner fees with per-byte pricing.

3. Pruned metadata can be archived by issuers and other token stakeholders.  In fact, this is part of the protocol’s "proof-of-trust" where the issuer will be required to maintain the set of metadata associated with the issuer’s token so that he can make regular commitments to prove the issuer agrees with the consensus ruleset for the token type.

4. Archival data retains full Proof-of-Work security as it is and will always be an immutable as part of the merkle tree.

5. The Bitcoin Cash UTXO set will grow as a result of token usage.  However, this is not fundamentally different from UTXO set growth from non-token usage, and is a necessary effect that comes from more blockchain usage in general, which is desired.

# Appendix A: Regulated Security Tokens

# Copyright

This protocol specification is published under the terms of the MIT license.
