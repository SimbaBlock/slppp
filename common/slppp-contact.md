# SLP++ Contact Protocol Specification
### Specification version: 0.1
### Date published: June 22, 2020

# PROTOCOL DESCRIPTION

## Contact ID
```
The Contact is identified by sha256 the create Contact transaction output(txid + n) which can be regarded as `contact_id`.
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
   &lt;protocol_id<sup>2</sup>: '\x01\x06'&gt; (2 bytes integer)<br/>
   &lt;action: 'CREATE'&gt; (6 bytes, ascii)<br/>
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
    &lt;name: &gt; (0 to ∞ bytes ascii)<br/>
    &lt;contact: &gt; (0 to ∞ bytes ascii)<br/>
    &lt;tags: &gt; (0 to ∞ bytes ascii)<br/>
    &lt;mark: &gt; (0 to ∞ bytes ascii)<br/>
    </td>
    <td>0</td>
  </tr>
 
</table>

<sup>1. The lockingscript can be any valid script combination.  UPDATE&REMOVE lockingscript are the same means</sup>   
<sup>2. See more [type](../index.md)</sup>  

### UPDATE - Update contact Transaction
  
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
&lt;protocol_id: '\x01\x06'&gt; (2 bytes integer)<br/>
&lt;action: 'UPDATE'&gt; (6 byte ascii)<BR>
&lt;data_hash&gt; (32 bytes, sha256(data))<BR>
&lt;encrypt: '0' / '1'&gt; (1 byte integer)<br/>
&lt;encrypted_ passwd: &gt; (32 bytes, if encrypt is true )<br/>
&lt;contact_id&gt; (32 bytes)<BR>
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
    &lt;name: &gt; (0 to ∞ bytes ascii)<br/>
    &lt;contact: &gt; (0 to ∞ bytes ascii)<br/>
    &lt;tags: &gt; (0 to ∞ bytes ascii)<br/>
    &lt;mark: &gt; (0 to ∞ bytes ascii)<br/>
    <td>0</td>
  </tr>

</table>


### REMOVE - Remove contact Transaction

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
&lt;protocol_id: '\x01\x06'&gt; (2 bytes integer)<br/>
&lt;action: 'REMOVE'&gt; (6 bytes ascii)<BR>
&lt;contact_id&gt; (32 bytes)<BR>
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

**CREATE contact Transaction**

todo

**UPDATE contact Transaction**

todo

**REMOVE contact Transaction**

todo

# Copyright

This protocol specification is published under the terms of the MIT license.
