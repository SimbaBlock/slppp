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
   Lockingscript<sup>1</sup>: 'OP_IF OP_DUP OP_HASH160 c079c08dd91583a5a48786f3b9da08893b3687ca OP_EQUALVERIFY OP_CHECKSIG OP_ELSE OP_DUP OP_HASH160 0e406c10d0315942e442946661a0931ce7181fab OP_EQUALVERIFY OP_CHECKSIG OP_ENDIF' (0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
   &lt;lokad_id: 'SLP++\x00'&gt; (6 bytes, ascii)<br/>
   &lt;type: 'contract'&gt; (8 bytes ascii)<br/>
   &lt;action: 'CREATE'&gt; (6 bytes, ascii)<br/>
   &lt;title: &gt; (4 to  ∞ bytes, suggested utf-8)<br/>
   &lt;sign_date:&gt; (4 to ∞ bytes, suggested utf-8)<br/>
   &lt;sign_exp_date&gt; (4 to ∞ bytes, suggested utf-8)<br/>
   &lt;exp_date&gt; (4 bytes or ∞ bytes)<br/>
   &lt;mark:&gt; (0 to ∞ bytes, ascii)<br/>
   &lt;data_hash:&gt; (32 bytes)<br/>
   &lt;encrypt: 'FALSE(0)' / 'TRUE(1)'&gt; (1 byte integer)<br/>
   &lt;aes_pwd: (32 bytes ascii by ecdh, if encrypt is true )<br/>
   &lt;pubkey1: &gt; (32 bytes ascii)<br/>
   &lt;pubkey2: &gt; (32 bytes ascii)<br/>
   </td>
    <td>>0</td>
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
    <td>0</td>
  </tr>
 
</table>

<sup>1. The Lockingscript can be any valid script combination.  UPDATE Lockingscript is the same means</sup>   

### UPDATE - Update Contract Transaction Outputs
  
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
   Lockingscript: 'OP_IF OP_DUP OP_HASH160 c079c08dd91583a5a48786f3b9da08893b3687ca OP_EQUALVERIFY OP_CHECKSIG OP_ELSE OP_DUP OP_HASH160 0e406c10d0315942e442946661a0931ce7181fab OP_EQUALVERIFY OP_CHECKSIG OP_ENDIF'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
&lt;lokad_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;type: 'contract'&gt; (8 bytes ascii)<br/>
&lt;thransaction_type: 'REVOKE/REJECT/APPROVE'&gt; (5 to 16 byte ascii)<BR>
&lt;contract_id&gt; (32 bytes)<BR>
  </td>
    <td>>0 </td>
  </tr>

  <tr>
    <td>...</td>
    <td>Any</td>
    <td>any</td>
  </tr>

</table>


### Examples

**Create Contract Transaction**

bsv blockchain transaction: d48830fa3a44a3228148dd46a9703eee0cef6d0039570c9d3a4aac6d28e00a3b

SCRIPT: ``6376a914c079c08dd91583a5a48786f3b9da08893b3687ca88ac6776a9140e406c10d0315942e442946661a0931ce7181fab88ac686a06534c502b2b0008636f6e747261637406435245415445057469746c650a323032302d30352d30310a323032302d30352d30310a323032302d30352d3031046d61726b20c775e7b757ede630cd0aa1113bd102661ab38829ca52a6422ab782862f2686460100``

**Update Contract Transaction**

bsv blockchain transaction: 7b0529f8ba4da4bce6ab9cc74eaf3f135c991ca9bbfd9df07be56248ef7da8ff

SCRIPT: ``6376a914c079c08dd91583a5a48786f3b9da08893b3687ca88ac6776a9140e406c10d0315942e442946661a0931ce7181fab88ac686a06534c502b2b0008636f6e747261637407415050524f5645206f7ec00797a365687069dd90476bb8c97b5c4449a643b860223ca48e5ddf53c1``

# Copyright

This protocol specification is published under the terms of the MIT license.
