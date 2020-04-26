# SLP++ Electronic Contract Protocol Specification
### Specification version: 0.1
### Date published: April 24, 2020

### Requirements
**1. Need BSV genesis upgrade.**

# PROTOCOL DESCRIPTION

## Transaction Detail

### Create - Create Contract Transaction Outputs

This transaction defines the properties, metadata and contract self. 
```
The contract is identified by sha256 the transaction outputscript which is referred as `contract_id(32 bytes)`.
```

**Transaction inputs**: Any number of inputs or content of inputs, in any order.

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>BSV<br/>amount</b></td>
</tr>
  <tr>
    <td>...</td>
   <td>
   Lockingscript<sup>1</sup>: 'OP_DUP OP_HASH160 986b57ea26555d28c OP_EQUALVERIFY OP_CHECKSIG' (0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
   &lt;lokad_id: 'SLP++\x00'&gt; (6 bytes, ascii)<sup>2</sup><br/>
   &lt;type: 'contract'&gt; (8 bytes ascii)<br/>
   &lt;action: 'CREATE'&gt; (6 bytes, ascii)<br/>
   &lt;title: &gt; (4 to  ∞ bytes, suggested utf-8)<br/>
   &lt;sign_date:&gt; (4 to ∞ bytes, suggested utf-8)<br/>
   &lt;sign_exp_date&gt; (4 to ∞ bytes, suggested utf-8)<br/>
   &lt;exp_date&gt; (4 bytes or ∞ bytes)<br/>
   &lt;mark:&gt; (0 to ∞ bytes, ascii)<br/>
   &lt;data_hash:&gt; (32 bytes)<br/>
   &lt;encrypt: '0' / '1'&gt; (1 byte integer)<br/>
   &lt;aes_pwd: (32 bytes ascii, by ecdh )<br/>
   &lt;pubkey1: &gt; (32 bytes ascii)<br/>
   &lt;pubkey2: &gt; (32 bytes ascii)<br/>
   </td>
    <td>any<sup>2</sup></td>
  </tr>
  
  <tr>
    <td>...</td>
    <td>Any</td>
    <td>any</td>
  </tr>
  
  <tr>
    <td>...</td>
    <td>
    OP_FALSE <br>
    OP_RETURN: data (0 to  ∞ bytes)</td>
    <td>any</td>
  </tr>
 
</table>

<sup>1. The Lockingscript can be any valid script combination.  UPDATE Lockingscript is the same means</sup>   

### UPDATE - Update Contract Transaction Outputs

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>BSV<br/>amount</b></td>
</tr>
  <tr>
  <td>...</td>
  <td>
   Lockingscript: 'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
&lt;lokad_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;type: 'contract'&gt; (8 bytes ascii)<br/>
&lt;thransaction_type: 'REVOKE/REJECT/APPROVE'&gt; (5 to 16 byte ascii)<BR>
&lt;contract_id&gt; (32 bytes)<BR>
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

**Create Contract Transaction**

bsv blockchain transaction:  a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d87b20d2502682647

SCRIPT: ``006a04534c500001010747454e45534953045553445423546574686572204c74642e20555320646f6c6c6172206261636b656420746f6b656e734168747470733a2f2f7465746865722e746f2f77702d636f6e74656e742f75706c6f6164732f323031362f30362f546574686572576869746550617065722e70646620db4451f11eda33950670aaf59e704da90117ff7057283b032cfaec77793139160108010208002386f26fc10000``

**Update Contract Transaction**

bsv blockchain transaction: 6b73adfbe7e5688c53ea4b09bf37de85dfd6dd4e3d38d1c0b4a5b38a9c0ca613

SCRIPT: ``006a04534c50000101044d494e5420a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d87b20d2502682647010208002386f26fc10000``

# Copyright

This protocol specification is published under the terms of the MIT license.
