# SLP++ Message  Protocol Specification
### Specification version: 0.1
### Date published: April 25, 2020

# PROTOCOL DESCRIPTION

## Message ID
```
The message is identified by sha256 the transaction output(txid + n) which is referred as `message_id(32 bytes)`.
```

## Transaction Detail    
### PEER - Peer to Peer Message Transaction Outputs

**Transaction inputs**: Any number of inputs or content of inputs, in any order.

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>Note</b><br/></td>
</tr>
  <tr>
    <td>...</td>
   <td>
   lockingscript<sup>1</sup>: 'OP_DUP OP_HASH160 986b57ea26555d28c OP_EQUALVERIFY OP_CHECKSIG' (0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
   &lt;len: &gt; (length of metadata)<br/>
   len < 0x4c : (1 byte integer) <br/> 
   len <= 0xff : ('4c' + 1 byte integer) <br/> 
   len <= 0xffff : ('4d' + 2 bytes integer)<br/> 
   len <= 0xffffffff : ('4e' + 4 bytes integer) <br/> 
   <b>metadata:</b><br/>
   &lt;protocol_id: 'SLP++\x00'&gt; (6 bytes, ascii)<br/>
   &lt;type<sup>2</sup>: '\x01\x01'&gt; (2 bytes integer)<br/>
   &lt;action: 'PEER' &gt; (4 to 5  bytes ascii)<br/>
   &lt;data:&gt; (0 to ∞ bytes)<br/>
   &lt;ref: '0' / '1'&gt; (1 byte integer)<br/>
   &lt;message_id:&gt; (32 bytes asscii,quote the old message_id, if ref is true)<br/>	  
   &lt;encrypt: '0' / '1'&gt; (1 byte integer, )<br/>
   &lt;aes_pwd: &gt; (32 bytes ascii by ecdh, if encrypt is true )<br/>
   </td>
    <td>...</td>
  </tr>
  
  <tr>
    <td>...</td>
    <td>Any</td>
    <td>...</td>
  </tr>
 
</table>

<sup>1. The lockingscript can be any valid script combination.  GROUP,JGROUP & LGROUP lockingscript are the same means</sup>   
<sup>2. See more [type](../index.md)</sup>

### GROUP - Group Message Transaction Outputs

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>Note</b></td>
</tr>
  <tr>
  <td>...</td>
  <td>
   lockingscript: 'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
   &lt;len: &gt; (length of metadata)<br/>
   len < 0x4c : (1 byte integer) <br/> 
   len <= 0xff : ('4c' + 1 byte integer) <br/> 
   len <= 0xffff : ('4d' + 2 bytes integer)<br/> 
   len <= 0xffffffff : ('4e' + 4 bytes integer) <br/> 
   <b>metadata:</b><br/>
&lt;protocol_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;type: '\x01\x01'&gt; (2 bytes integer)<br/>
&lt;action: 'GROUP'&gt; (5 byte ascii)<BR>
&lt;ref: '0' / '1'&gt; (1 byte integer)<br/>
&lt;message_id:&gt; (32 bytes asscii,quote the old message, if ref is true)<br/>	  
&lt;encrypt: '0' / '1'&gt; (1 byte integer)<br/>
&lt;aes_pwd:&gt; (32 bytes ascii by ecdh, if encrypt is true )<br/>
  </td>
    <td>...</td>
  </tr>

  <tr>
  <td>...</td>
  <td>
   lockingscript: 'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
   &lt;len: &gt; (length of data)<br/>
   len < 0x4c : (1 byte integer) <br/> 
   len <= 0xff : ('4c' + 1 byte integer) <br/> 
   len <= 0xffff : ('4d' + 2 bytes integer)<br/> 
   len <= 0xffffffff : ('4e' + 4 bytes integer) <br/> 
   <b>metadata:</b><br/>   
&lt;protocol_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;type: '\x01\x01'&gt; (2 bytes integer)<br/>
&lt;action: 'GROUP'&gt; (5 byte ascii)<BR>
&lt;ref: '0' / '1'&gt; (1 byte integer)<br/>
&lt;message_id:&gt; (32 bytes asscii,quote the old message_id,  if ref is true)<br/>	  
&lt;encrypt: '0' / '1'&gt; (1 byte integer)<br/>
&lt;aes_pwd:&gt;(32 bytes ascii by ecdh, if encrypt is true )<br/>
  </td>
    <td>...</td>
  </tr>

  <tr>
    <td>...</td>
    <td>Any</td>
    <td>...</td>
  </tr>

  <tr>
  <td>...</td>
  <td>
   OP_FALSE <br>
   OP_RETURN <br> 
   &lt;len: &gt; (length of data)<br/>
   len < 0x4c : (1 byte integer) <br/> 
   len <= 0xff : ('4c' + 1 byte integer) <br/> 
   len <= 0xffff : ('4d' + 2 bytes integer)<br/> 
   len <= 0xffffffff : ('4e' + 4 bytes integer) <br/> 
   <b>metadata:</b><br/>
   &lt;data: &gt;(0 to ∞ bytes, ascii, encrypt by aes)<br/>
  </td>
    <td>...</td>
  </tr>



</table>


### JGROUP - Join Group Message Transaction Outputs

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>Note</b></td>
</tr>
  <tr>
  <td>...</td>
  <td>
   lockingscript: 'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
   &lt;len: &gt; (length of metadata)<br/>
   len < 0x4c : (1 byte integer) <br/> 
   len <= 0xff : ('4c' + 1 byte integer) <br/> 
   len <= 0xffff : ('4d' + 2 bytes integer)<br/> 
   len <= 0xffffffff : ('4e' + 4 bytes integer) <br/> 
   <b>metadata:</b><br/>
&lt;protocol_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;type: '\x01\x01'&gt; (2 bytes integer)<br/>
&lt;action: 'JGROUP'&gt; (6 byte ascii)<BR>
  </td>
    <td>...</td>
  </tr>

  <tr>
  <td>...</td>
  <td>
   lockingscript: 'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
   &lt;len: &gt; (length of data)<br/>
   len < 0x4c : (1 byte integer) <br/> 
   len <= 0xff : ('4c' + 1 byte integer) <br/> 
   len <= 0xffff : ('4d' + 2 bytes integer)<br/> 
   len <= 0xffffffff : ('4e' + 4 bytes integer) <br/> 
   <b>metadata:</b><br/>
&lt;protocol_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;type: '\x01\x01'&gt; (2 bytes integer)<br/>
&lt;action: 'JGROUP'&gt; (5 byte ascii)<BR>
  </td>
    <td>...</td>
  </tr>

  <tr>
    <td>...</td>
    <td>Any</td>
    <td>...</td>
  </tr>
</table>

### LGROUP - Leave Group Message Transaction Outputs

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>Note</b></td>
</tr>
  <tr>
  <td>...</td>
  <td>
   lockingscript: 'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
   &lt;len: &gt; (length of metadata)<br/>
   len < 0x4c : (1 byte integer) <br/> 
   len <= 0xff : ('4c' + 1 byte integer) <br/> 
   len <= 0xffff : ('4d' + 2 bytes integer)<br/> 
   len <= 0xffffffff : ('4e' + 4 bytes integer) <br/> 
   <b>metadata:</b><br/>
&lt;protocol_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;type: '\x01\x01'&gt; (2 bytes integer)<br/>
&lt;action: 'LGROUP'&gt; (5 byte ascii)<BR>
  </td>
    <td>...</td>
  </tr>

  <tr>
  <td>...</td>
  <td>
   lockingscript: 'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 bytes, ascii)<br/>
   &lt;len: &gt; (length of data)<br/>
   len < 0x4c : (1 byte integer) <br/> 
   len <= 0xff : ('4c' + 1 byte integer) <br/> 
   len <= 0xffff : ('4d' + 2 bytes integer)<br/> 
   len <= 0xffffffff : ('4e' + 4 bytes integer) <br/> 
   <b>metadata:</b><br/>
&lt;protocol_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;type: '\x01\x01'&gt; (2 bytes integer)<br/>
&lt;action: 'LGROUP'&gt; (5 byte ascii)<BR>
  </td>
    <td>...</td>
  </tr>

  <tr>
    <td>...</td>
    <td>Any</td>
    <td>...</td>
  </tr>
</table>

### Examples

**CREARE Message Peer to Peer Transaction**

blockchain transaction:  

**GROUP Message Transaction**

blockchain transaction: 

**JGROUP Message Transaction**  
todo  

**LGROUP Message Transaction**   
todo  


# Copyright

This protocol specification is published under the terms of the MIT license.
