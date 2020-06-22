# SLP++ Directory Protocol Specification
### Specification version: 0.1
### Date published: June 22, 2020

# PROTOCOL DESCRIPTION

## Directory ID
```
The directory is identified by sha256 the create directory transaction output (txid + n) which can be regarded as `directory_id`.
```

## Transaction Detail

### Create - Create Directory Transaction

This transaction defines the properties, metadata and directory itself. 

**Transaction inputs**: Any number of inputs or content of inputs, in any order.

**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>Coin<br>amount </b></td>
</tr>
  <tr>
    <td>...</td>
   <td>
   <b>lockingscript<sup>1</sup>:</b><br/>
   'OP_DUP OP_HASH160 986b57ea26555d28c OP_EQUALVERIFY OP_CHECKSIG' (0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 byte, ascii)<br/>
   <b>metadata:</b><br/>
   &lt;protocol: 'SLP++\x00'&gt; (6 bytes, ascii)<br/>
   &lt;protocol_id<sup>2</sup>: '\x01\x07'&gt; (2 bytes integer)<br/>
   &lt;action: 'CREATE' &gt; (5 bytes ascii)<br/>
   &lt;mark:&gt; (0  to  1024 bytes, ascii)<br/>
   &lt;encrypt: '0x00/0x01' &gt; (1 byte integer)<br/>
   &lt;encrypted_pwd &gt; (4 to 32 bytes ascii,if encrypt is true)<br/>
   &lt;data_hash:&gt; (32 bytes, sha256(data))<br/>
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
    OP_FALSE: '\x00'  (1 byte, ascii)<br>
    OP_RETURN: '\x6a' (1 byte, ascii)<br>
   <b>data:</b><br/>
    &lt;bunsinese_action: '+'&gt; (1 byte ascii)<br/>
    &lt;drive_ids: &gt; (32 * n bytes ascii)<br/>
    </td>
    <td>0</td>
  </tr>
 
</table>

<sup>1. The lockingscript can be any valid script combination.  UPDATE's lockingscript are the same means</sup>   
<sup>2. See more [type](../index.md)</sup>  

### UPDATE - Update Directory Transaction
  
**Transaction inputs**: Any number of inputs or content of inputs, in any order.  
**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>Coin</br>amount </b></td>
</tr>
  <tr>
  <td>...</td>
  <td>
   <b>lockingscript:</b><br/>
   'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 byte, ascii)<br/>
   <b>metadata:</b><br/>
&lt;protocol: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;protocol_id: '\x01\x07'&gt; (2 bytes integer)<br/>
&lt;action: 'UPDATE' &gt; (6 bytes ascii)<br/>
&lt;mark&gt; (0 to ∞ bytes)<BR>
&lt;encrypt: '0x00/0x01' &gt; (1 byte integer)<br/>
&lt;encrypted_pwd &gt; (4 to 32 bytes ascii,if encrypt is true)<br/>
&lt;data_hash&gt; (32 bytes, sha256(data))<BR>
&lt;directory_id&gt; (32 bytes)<BR>
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
    OP_FALSE: '\x00'  (1 byte, ascii)<br>
    OP_RETURN: '\x6a' (1 byte, ascii)<br>
   <b>data:</b><br/>
    &lt;bunsineses_action: '+'&gt; (1 byte)<br/>
    &lt; drive_ids: &gt; (32 * n  bytes ascii)<br/>
    &lt;bunsineses_action: '-'&gt; (1 byte)<br/>
    &lt; drive_ids: &gt; (32 * n  bytes ascii)<br/>
    <td>0</td>
  </tr>

</table>

### Remove - Remove Directory Transaction
  
**Transaction inputs**: Any number of inputs or content of inputs, in any order.  
**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>Coin</br>amount </b></td>
</tr>
  <tr>
  <td>...</td>
  <td>
   <b>lockingscript:</b><br/>
   'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 byte, ascii)<br/>
   <b>metadata:</b><br/>
&lt;protocol: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;protocol_id: '\x01\x07'&gt; (2 bytes integer)<br/>
&lt;action: 'REMOVE' &gt; (6 bytes ascii)<br/>
&lt;directory_id&gt; (32 bytes)<BR>
  </td>
  <td>>0</td>
  </tr>

  <tr>
    <td>...</td>
    <td>Any</td>
    <td>any</td>
  </tr>
</table>


### Examples

**CREATE Directory Transaction**
todo

**UPDATE Directory Transaction**
todo

**REMOVE Directory Transaction**
todo

# Copyright

This protocol specification is published under the terms of the MIT license.
