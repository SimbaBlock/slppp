# SLP++ Electronic Contract Protocol Specification
### Specification version: 0.1
### Date published: April 24, 2020

# PROTOCOL DESCRIPTION

## Contract ID  
```
The contract is identified by sha256 the transaction output(txid + n) which is referred as `contract_id(32 bytes)`.
```

## Transaction Detail

### Create - Create Contract Transaction 
This transaction defines the properties, metadata and contract itself.        
**Transaction inputs**: Any number of inputs or content of inputs, in any order.  

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>Coin<br/>amount</b></td>
</tr>
  <tr>
    <td>...</td>
   <td>
   <b>Lockingscript<sup>1</sup>:</b><br/> 
   'OP_IF OP_DUP OP_HASH160 c079c08dd91583a5a48786f3b9da08893b3687ca OP_EQUALVERIFY OP_CHECKSIG OP_ELSE OP_DUP OP_HASH160 0e406c10d0315942e442946661a0931ce7181fab OP_EQUALVERIFY OP_CHECKSIG OP_ENDIF' (0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 byte, ascii)<br/>
   &lt;len: &gt; (length of metadata)<br/>
   len < 0x4c,  1 byte integer <br/> 
   len <= 0xff, '4c + 1 byte integer' <br/> 
   len <= 0xffff,'4d + 2 bytes integer' <br/> 
   len <= 0xffffffff, '4e + 4 bytes integer' <br/> 
   <b>metadata:</b><br/>
   &lt;protocol_id: 'SLP++\x00'&gt; (6 bytes, ascii)<br/>
   &lt;type<sup>2</sup>: '\x03\x01'&gt; (2 bytes integer)<br/>
   &lt;action: 'CREATE'&gt; (6 bytes, ascii)<br/>
   &lt;title: &gt; (4 to  ∞ bytes, suggested utf-8)<br/>
   &lt;sign_date:&gt; (4 to ∞ bytes, suggested utf-8)<br/>
   &lt;sign_exp_date&gt; (4 to ∞ bytes, suggested utf-8)<br/>
   &lt;exp_date&gt; (4 bytes or ∞ bytes)<br/>
   &lt;mark:&gt; (0 to ∞ bytes, ascii)<br/>
   &lt;data_hash:&gt; (32 bytes, sha256(data))<br/>
   &lt;encrypt: 'FALSE(0)' / 'TRUE(1)'&gt; (1 byte integer)<br/>
   &lt;aes_pwd: (32 bytes ascii by ecdh, if encrypt is true )<br/>
   &lt;pubkey1: &gt; (32 bytes ascii, if encrypt is true)<br/>
   &lt;pubkey2: &gt; (32 bytes ascii, if encrypt is true)<br/>
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
    OP_FALSE : '\x00' (1 byte, ascii)<br>
    OP_RETURN: '\x6a' (1 byte, ascii)<br>
   &lt;len: &gt; (length of data)<br/>
   len < 0x4c,  1 byte integer <br/> 
   len <= 0xff, '4c + 1 byte integer' <br/> 
   len <= 0xffff,'4d + 2 bytes integer' <br/> 
   len <= 0xffffffff, '4e + 4 bytes integer' <br/> 
    <b>data:</b><br/>
    &lt;data: &gt; (0 to ∞ bytes)<br/>
    </td>
    <td>0</td>
  </tr>
 
</table>

<sup>1. The Lockingscript can be any valid script combination.  UPDATE Lockingscript is the same means</sup>   
<sup>2. See more [type](../index.md)</sup>  

### UPDATE - Update Contract Transaction 
  
**Transaction inputs**: Any number of inputs or content of inputs, in any order.  
**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>Coin<br/>amount</b></td>
</tr>
  <tr>
  <td>...</td>
  <td>
   <b>Lockingscript:</b><br/>
   'OP_IF OP_DUP OP_HASH160 c079c08dd91583a5a48786f3b9da08893b3687ca OP_EQUALVERIFY OP_CHECKSIG OP_ELSE OP_DUP OP_HASH160 0e406c10d0315942e442946661a0931ce7181fab OP_EQUALVERIFY OP_CHECKSIG OP_ENDIF'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 byte, ascii)<br/>
   &lt;len: &gt; (length of metadata)<br/>
   len < 0x4c,  1 byte integer <br/> 
   len <= 0xff, '4c + 1 byte integer' <br/> 
   len <= 0xffff,'4d + 2 bytes integer' <br/> 
   len <= 0xffffffff, '4e + 4 bytes integer' <br/> 

   <b>metadata:</b><br/>
&lt;protocol_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;type: '\x03\x01'&gt; (2 bytes integer)<br/>
&lt;action: 'REVOKE/REJECT/APPROVE'&gt; (5 to 16 bytes ascii)<BR>
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

blockchain transaction: 4617f5e4d859d281cc7fc3fd02fe9230d9cd37a2665f3e9a5201603017f9c93c

SCRIPT: ``6376a914602c8763be74d22f96c43d3f3f965171892df64188ac6776a914c079c08dd91583a5a48786f3b9da08893b3687ca88ac686a4c6006534c502b2b0002030106435245415445057469746c650a323032302d30352d30310a323032302d30352d30310a323032302d30352d3031046d61726b2007334386287751ba02a4588c1a0875dbd074a61bd9e6ab7c48d244eacd0c99e00100``

OUTPUTSCRIPT BROKEN DOWN:
<table>
<tr>
<td>6376a914602c8763be74d22f96c43d3f3f965
171892df64188ac6776a914c079c08dd91583
a5a48786f3b9da08893b3687ca88ac68</td>
<td>OP_IF OP_DUP OP_HASH160 602c8763be74d22f96c43d3f3f965171892df641 
  OP_EQUALVERIFY OP_CHECKSIG OP_ELSE OP_DUP OP_HASH160 
  c079c08dd91583a5a48786f3b9da08893b3687ca OP_EQUALVERIFY OP_CHECKSIG OP_ENDIF</td>
</tr>
 <tr>
  <td>6a</td>
  <td>OP_RETURN</td>
 </tr>
 <tr>
  <td>4c60</td>
  <td>OP_PUSHDATA1 + 60 (length of metadata) (2 bytes)</td>
 </tr>
 <tr>
  <td>06</td>
  <td>length of protocol_id field (6 bytes)</td>
 </tr>
 <tr>
  <td>534c502b2b00</td>
  <td>SLP++\x00</td>
 </tr>
 <tr>
  <td>02</td>
  <td>length of type field (2 bytes)</td>
 </tr>
 <tr>
  <td>0301</td>
  <td>contract</td>
 </tr>
 <tr>
  <td>06</td>
  <td>length of action field (6 bytes)</td>
 </tr>
 <tr>
  <td>435245415445</td>
  <td>'CREATE'</td>
 </tr>
 <tr>
  <td>05</td>
  <td>length of title field(5 bytes)</td>
 </tr>
 <tr>
  <td>
   7469746c65<br/>
  </td>
  <td>title</td>
 </tr>
 <tr>
  <td>0a</td>
  <td>length of sign_date field(10 bytes)</td>
 </tr>
 <tr>
  <td>323032302d30352d3031</td>
  <td>2020-05-01</td>
 </tr>
  <tr>
  <td>0a</td>
  <td>length of sign_exp_date field(10 bytes)</td>
 </tr>
 <tr>
  <td>323032302d30352d3031</td>
  <td>2020-05-01</td>
 </tr>
 <tr>
  <td>0a</td>
  <td>length of exp_date field(10 bytes)</td>
 </tr>
 <tr>
  <td>323032302d30352d3031</td>
  <td>2020-05-01</td>
 </tr>
 <tr>
  <td>04</td>
  <td>length of mark field(4 bytes)</td>
 </tr>
 <tr>
  <td>6d61726b</td>
  <td>mark</td>
 </tr>
 <tr>
  <td>20</td>
  <td>length of data_hash field(32 bytes)</td>
 </tr>
 <tr>
  <td>07334386287751ba02a4588c1a0875db
d074a61bd9e6ab7c48d244eacd0c99e0
</td>
  <td>sha256(data)</td>
 </tr>
  <tr>
  <td>01</td>
  <td>length of encrypt field(1 byte)</td>
 </tr>
 <tr>
  <td>00</td>
  <td>FALSE</td>
 </tr>
</table>

**Update Contract Transaction**

blockchain transaction: 459ff9525975e4ef51222440458a4c14e428e6e74f8dbfd7ed6cd096745083cf

SCRIPT: ``6376a914602c8763be74d22f96c43d3f3f965171892df64188ac6776a914c079c08dd91583a5a48786f3b9da08893b3687ca88ac686a3306534c502b2b0002030107415050524f564520259dea63aec87659725e390a137c0c04996a2fdd340bfec7edc5559b7e03fecb``

OUTPUTSCRIPT BROKEN DOWN:
<table>
<tr>
<td>6376a914602c8763be74d22f96c43d3f3f965
171892df64188ac6776a914c079c08dd91583
a5a48786f3b9da08893b3687ca88ac68</td>
<td>OP_IF OP_DUP OP_HASH160 602c8763be74d22f96c43d3f3f965171892df641 
  OP_EQUALVERIFY OP_CHECKSIG OP_ELSE OP_DUP OP_HASH160 
  c079c08dd91583a5a48786f3b9da08893b3687ca OP_EQUALVERIFY OP_CHECKSIG OP_ENDIF</td>
</tr>
 <tr>
  <td>6a</td>
  <td>OP_RETURN</td>
 </tr>
 <tr>
  <td>33</td>
  <td>length of metadata (1 bytes)</td>
 </tr>
 <tr>
  <td>06</td>
  <td>length of protocol_id field (6 bytes)</td>
 </tr>
 <tr>
  <td>534c502b2b00</td>
  <td>SLP++\x00</td>
 </tr>
 <tr>
  <td>02</td>
  <td>length of type field (2 bytes)</td>
 </tr>
 <tr>
  <td>0301</td>
  <td>contract</td>
 </tr>
 <tr>
  <td>07</td>
  <td>length of thransaction_type field (7 bytes)</td>
 </tr>
 <tr>
  <td>415050524f5645</td>
  <td>'APPROVE'</td>
 </tr>
  <td>20</td>
  <td>length of contract_id field(32 bytes)</td>
 </tr>
 <tr>
  <td>259dea63aec87659725e390a137c0c04996a2fdd
    340bfec7edc5559b7e03fecb
</td>
  <td>contract_id, sha256(outputscript)</td>
 </tr>
</table>

# Copyright

This protocol specification is published under the terms of the MIT license.
