# SLP++ Token Type 1 Protocol Specification
### Specification version: 0.1
### Date published: April 23, 2020

### Requirements

A good solution for implementing tokens may have the following properties:

**1. Permissionless**. Its tokens should not require permission to issue or transfer.\
**2. Simple**. The system should be easily understandable and straightforward.\
**3. Robust.** A minimal, unambiguous rule set supports a fault-tolerant consensus layer.\
**4. Non-invasive.** It should require no changes to the underlying Bitcoin  protocol.\
**5. SPV friendly.** Users of light wallets should be able to validate their own transactions.\
**6. Extensible.** The system should allow future versions of tokens including tokens with issuer controlled whitelists for regulated securities , and other needs.\
**7. Supported.** There should be an implementation plan for rapid ecosystem support.\
**8. Need BSV genesis upgrade.**

# PROTOCOL DESCRIPTION

## Transaction Detail

### GENESIS - Token Genesis Transaction Outputs

This is the first transaction which defines the properties, metadata and initial mint quantity of the token. The token is thereafter uniquely identified by sha256 the token genesis transaction OutputScript  which is referred to as `token_id`.

`token_type` indicates the SLP++ sub-protocol:

* 1 - Permissionless Token Type
* 2 - Reserved for Security Token Type 
* 3 - Reserved for Voting Token Type
* 4 - Reserved for Ticketing Token Type
* ...

This document specifies the rules and operation of the Permissionless Token Type (1) only. Tokens of different types cannot be mixed, and so future specifications of other token types will not affect the consensus validity of type <sup>2</sup>.

`mint_baton_vout`: Future token supply increases are made possible if the genesis endows a specific transaction output with a "minting baton" that can be passed along and used for future minting (using 'MINT' transactions, see below). If `mint_baton_vout` is not present or refers to a nonexistent output, then the baton does not exist and the token provably has a one-time issuance.

`decimals`: indicates that 1 token is divisible into 10^`decimals` base units. SLP++ messages store whole numbers indicating token amounts as measured in the base unit, analogous to how bitcoin transactions store BSV amounts measured in the base unit 'satoshis'. 
```
With a token FOO having `decimals` of 6 indicated in the genesis, for example, the quantity 12.53 FOO (as displayed in wallet software) would be represented by 12530000 base units (as 8 bytes, hex 0000000000bf3150). A `decimals` of 8 would give the same divisibility as bitcoin, whereas 0 would give indivisible tokens.
```

**Transaction inputs**: Any number of inputs or content of inputs, in any order.

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>BSV<br/>amount</b></td>
  <td><b>Implied token amount<br/>(base units)</b></td>
</tr>
  <tr>
    <td>...</td>
   <td>
   lockscript<sup>1</sup>: 'OP_DUP OP_HASH160 986b5779484a19fd99e1ea26ff0081d4b555d28c OP_EQUALVERIFY OP_CHECKSIG' (0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
   &lt;lokad_id: 'SLP++\x00'&gt; (6 bytes, ascii)<sup>2</sup><br/>
   &lt;token_type: 1&gt; (1 to 2 byte integer)<br/>
   &lt;transaction_type: 'GENESIS'&gt; (7 bytes, ascii)<br/>
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
    <td>any<sup>3</sup></td>
    <td>0</td>
  </tr>
 
</table>

<sup>1. The lockscript can be any valid script combination.  MINT & SEND lockscript are the same means</sup>   

<sup>2. The Lokad identifier is registered as the number 0x504c532B2B (which, when encoded in the 6-byte little-endian format expected for Lokad IDs, gives the ascii string 'SLP++\x00'). Inquiries and additional information about the Lokad system of OP_RETURN protocol identifiers can be found at https://github.com/Lokad/Terab maintained by Joannes Vermorel.</sup>

<sup>3. SLP does not impose any restrictions on BSV output amounts. Typically however the OP_RETURN output would have 0 BSV (as any BSV sent would be burned), and outputs receiving tokens / mint batons would be sent only the minimal 'dust' amount of 0.00000546 BSV.</sup>

### MINT - Extended Minting Transaction Outputs
#### (used with "baton" to increase supply)

Subsequent minting transactions of `additional_token_quantity` can be performed by spending the "minting baton" UTXO in a special MINT transaction, described here. Note that this could be done by someone other than the GENESIS issuer, if the baton minting authority had been passed to another address.

As with GENESIS, the MINT allows to end the baton, or further pass on the baton to future mint operations: if `mint_baton_vout` is empty or refers to a nonexistent vout, the transaction is valid but the baton is lost. This makes it possible to prove end-of-minting capabilities for a token even after several minting events (it is impossible to duplicate this baton as that would require double-spending the transaction output associated with the baton).

**Transaction inputs**: Any number of inputs or content of inputs, in any order, but with required presence of a 'baton' input (see Consensus Rules).

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>BSV<br/>amount</b></td>
  <td><b>Implied token amount<br/>(base units)</b></td>
</tr>
  <tr>
  <td>...</td>
<td>
   lockscript: 'OP_DUP OP_HASH160 986b5779484a19fd99e1ea26ff0081d4b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
&lt;lokad_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;token_type: 1&gt; (1 to 2 byte integer)<BR>
&lt;thransaction_type: 'MINT'&gt; (4 bytes, ascii)<BR>
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


### SEND - Spend Transaction Outputs
#### (Send / Transfer)
The following transaction format is used to transfer tokens from one or more token holding UTXO(s) to new token holding UTXO(s). The UTXOs associated with unspent tokens will be used within the transaction input and, just like the BSV attached to these UTXOs, will be considered totally spent after this transaction is accepted by the blockchain. Tokens will be assigned to new UTXOs .  Any number of additional BSV outputs will be allowed. A BSV output is freeorder .

**Transaction inputs**: Any number of inputs or content of inputs, in any order, but must include sufficient tokens coming from valid token transactions of matching `token_id`, `token_type` (see Consensus Rules).

**Transaction outputs**:
<table>
  <tr>
    <td><b>v<sub>out</sub></b></td>
    <td><b>OutputScript</b></td>
    <td><b>BSV<br/>amount</b></td>
    <td><b>Implied token amount<br/>(base units)</b></td>
  </tr>
  <tr>
    <td>...</td>
    <td>
lockscript: 'OP_DUP OP_HASH160 986b5779484a19fd99e1ea26ff0081d4b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>      
OP_RETURN: '\x6a' (1 bytes, ascii)<BR>
&lt;lokad id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR/>
&lt;token_type: 1&gt; (1 to 2 byte integer)<BR/>
&lt;transaction_type: SEND&gt; (4 bytes, ascii)<BR/>   
&lt;token_id&gt; (32 bytes)<BR/>
&lt;token_output_quantity&gt; (<b>required</b>, 8 byte integer)<BR/>
  <td>any</td>
  <td>0</td>
  </tr>
</table>

### FROZEN - Frozen UTXO Transaction Outputs

TODO


### Examples

**GENESIS Transaction**

bsv blockchain transaction:  a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d87b20d2502682647

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

bsv blockchain transaction: 6b73adfbe7e5688c53ea4b09bf37de85dfd6dd4e3d38d1c0b4a5b38a9c0ca613

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

bsv blockchain transaction:  424d6948f7e6311816c27ea4a95418467f2e3f3185fcfba765c273d3aca5fde7

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


# Copyright

This protocol specification is published under the terms of the MIT license.
