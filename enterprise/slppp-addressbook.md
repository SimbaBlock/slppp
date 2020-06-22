# SLP++ Addressbook Protocol Specification
### Specification version: 0.1
### Date published: June 22, 2020

# PROTOCOL DESCRIPTION

## Contact ID
```
The Contact is identified by sha256 the create Contact transaction output(txid + n) which can be regarded as `contact_id`.
```

## Addressbook ID
```
The Addressbook is identified by sha256 the create Addressbook transaction output(txid + n) which can be regarded as `addressbook_id`.
```


## Transaction Detail

### Create - Create Transaction

This transaction defines the properties, metadata and addressbook itself. 

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
   lockingscript<sup>1</sup>: 'OP_DUP OP_HASH160 986b57ea26555d28c OP_EQUALVERIFY OP_ADDRESSBOOKSIG' (0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 byte, ascii)<br/>
   &lt;protocol: 'SLP++\x00'&gt; (6 bytes, ascii)<br/>
   &lt;protocol_id<sup>2</sup>: '\x03\x03'&gt; (2 bytes integer)<br/>
   &lt;action: 'CREATE'&gt; (6 bytes, ascii)<br/>
   &lt;type: 'CONTACT/ADDRESSBOOK'&gt; (0 to ∞ bytes ascii)<br/>
   &lt;data_hash:&gt; (32 bytes, sha256(data))<br/>
   &lt;encrypt: '0' / '1'&gt; (1 byte integer)<br/>
   &lt;encrypted_ passwd: &gt; (32 bytes, if encrypt is true )<br/>
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
    &lt;bunsinese_action: '+'&gt; (1 byte ascii, if type is 'ADDRESSBOOK')<br/>
    &lt;contact_ids: &gt; (32 * n bytes ascii, if type is 'ADDRESSBOOK')<br/>
    &lt;name: &gt; (0 to ∞ bytes ascii, if type is 'CONTACT')<br/>
    &lt;contact: &gt; (0 to ∞ bytes ascii, if type is 'CONTACT')<br/>
    &lt;tags: &gt; (0 to ∞ bytes ascii, if type is 'CONTACT')<br/>
    &lt;mark: &gt; (0 to ∞ bytes ascii)<br/>
    </td>
    <td>0</td>
  </tr>
 
</table>

<sup>1. The lockingscript can be any valid script combination.  UPDATE&REMOVE lockingscript are the same means</sup>   
<sup>2. See more [type](../index.md)</sup>  

### UPDATE - Update Addressbook Transaction
  
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
   lockingscript: 'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_ADDRESSBOOKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 byte, ascii)<br/>
&lt;protocol: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;protocol_id: '\x03\x03'&gt; (2 bytes integer)<br/>
&lt;action: 'UPDATE'&gt; (6 byte ascii)<BR>
&lt;type: 'CONTACT/ADDRESSBOOK'&gt; (7 or 11 bytes ascii)<br/>
&lt;data_hash&gt; (32 bytes, sha256(data))<BR>
&lt;encrypt: '0' / '1'&gt; (1 byte integer)<br/>
&lt;encrypted_ passwd: &gt; (32 bytes, if encrypt is true )<br/>
&lt;contact_id / addressbook_id&gt; (32 bytes)<BR>
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
    &lt;bunsinese_action: '+'&gt; (1 byte ascii, if type is 'ADDRESSBOOK')<br/>
    &lt;contact_ids: &gt; (32 * n bytes ascii, if type is 'ADDRESSBOOK')<br/>
    &lt;bunsinese_action: '-'&gt; (1 byte ascii, if type is 'ADDRESSBOOK')<br/>
    &lt;contact_ids: &gt; (32 * n bytes ascii, if type is 'ADDRESSBOOK')<br/>
    &lt;name: &gt; (0 to ∞ bytes ascii, if type is contact)<br/>
    &lt;contact: &gt; (0 to ∞ bytes ascii, if type is contact)<br/>
    &lt;tags: &gt; (0 to ∞ bytes ascii, if type is contact)<br/>
    &lt;mark: &gt; (0 to ∞ bytes ascii)<br/>
    <td>0</td>
  </tr>

</table>


### REMOVE - Remove Addressbook Transaction

**Transaction inputs**: Any number of inputs or content of inputs, in any order.  
**Transaction outputs**:
<table>
<tr>
  <td><b>v<sub>out</sub></b></td>
  <td><b>OutputScript </b></td>
  <td><b>Coin</br>amount</b></td>
</tr>
  <tr>
  <td>...</td>
  <td>
   lockingscript: 'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_ADDRESSBOOKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 byte, ascii)<br/>
&lt;protocol: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;protocol_id: '\x03\x03'&gt; (2 bytes integer)<br/>
&lt;action: 'REMOVE'&gt; (6 bytes ascii)<BR>
&lt;contact_id/addressbook_id&gt; (32 bytes)<BR>
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

**CREATE Addressbook Transaction**

blockchain transaction:  a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d87b20d2502682647

SCRIPT: ``006a04534c500001010747454e45534953045553445423546574686572204c74642e20555320646f6c6c6172206261636b656420746f6b656e734168747470733a2f2f7465746865722e746f2f77702d636f6e74656e742f75706c6f6164732f323031362f30362f546574686572576869746550617065722e70646620db4451f11eda33950670aaf59e704da90117ff7057283b032cfaec77793139160108010208002386f26fc10000``

**UPDATE Addressbook Transaction**

blockchain transaction: 6b73adfbe7e5688c53ea4b09bf37de85dfd6dd4e3d38d1c0b4a5b38a9c0ca613

SCRIPT: ``006a04534c50000101044d494e5420a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d87b20d2502682647010208002386f26fc10000``

**REMOVE Addressbook Transaction**

blockchain transaction: 6b73adfbe7e5688c53ea4b09bf37de85dfd6dd4e3d38d1c0b4a5b38a9c0ca613

SCRIPT: ``006a04534c50000101044d494e5420a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d87b20d2502682647010208002386f26fc10000``

# Copyright

This protocol specification is published under the terms of the MIT license.
