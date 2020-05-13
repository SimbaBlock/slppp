# SLP++ FutureSchool Protocol Specification 
### Specification version: 0.1
### Date published: May 13, 2020

# PROTOCOL DESCRIPTION

## School ID
```
The Service is identified by sha256 the create FutureSchool transaction outputscript which is referred as `school_id`.
```


## Transaction Detail

### Create - Create Future School Transaction

This transaction defines the properties, metadata and Future School itself. 

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
   <b>lockingscript<sup>1</sup>:</b><br/> 
   'OP_DUP OP_HASH160 986b57ea26555d28c OP_EQUALVERIFY OP_CHECKSIG' (0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 byte, ascii)<br/>
   <b>metadata:</b><br/>
   &lt;protocol_id: 'SLP++\x00'&gt; (6 bytes, ascii)<br/>
   &lt;type<sup>2</sup>: '\x0403'&gt; (2 bytes integer)<br/>
   &lt;action: 'CREATE'&gt; (6 bytes, ascii)<br/>
   &lt;title: &gt; (0 to 256 bytes, suggested utf-8)<br/>
   &lt;mark:&gt; (0  to  1024 bytes, ascii)<br/>
   &lt;data_hash:&gt; (32 bytes, sha256(data))<br/>
   &lt;encrypt: '0' / '1'&gt; (1 byte integer)<br/>
   &lt;aes_pwd: (32 bytes ascii,if encrypt is true)<br/>
   &lt;pubkey1: &gt; (32 bytes ascii, if encrypt is true)<br/>
   &lt;pubkey2: &gt; (32 bytes ascii, if encrypt is true)<br/>
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
    OP_FALSE: '\x00'  (1 byte, ascii)<br>
    OP_RETURN: '\x6a' (1 byte, ascii)<br>
   <b>data:</b><br/>
    &lt;data: &gt; (0 to ∞ bytes)<br/>
    </td>
    <td>any</td>
  </tr>
 
</table>

<sup>1. The lockingscript can be any valid script combination, which controls who can operate the data, by spent the utxo.  ACTION & ACTION's lockingscript are the same means</sup>     
<sup>2. See more [type](../slppp-index.md)</sup>   
### UPDATE - Update Future School Transaction
  
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
   <b>lockingscript:</b></br> 
   'OP_DUP OP_HASH160 986b59fd99b555d28c OP_EQUALVERIFY OP_CHECKSIG'(0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 byte, ascii)<br/>
   <b>metadata:</b><br/>
&lt;protocol_id: 'SLP++\x00'&gt; (6 bytes, ascii)<BR>
&lt;type: '\x0403'&gt; (2 bytes integer)<br/>
&lt;action: 'UPDATE'&gt; (6 bytes ascii)<BR>
&lt;mark&gt; (0 to ∞ bytes)<BR>
&lt;data_hash&gt; (32 bytes, sha256(data))<BR>
&lt;template_id&gt; (32 bytes)<BR>
  </td>
    <td>any</td>
  </tr>

  <tr>
    <td>...</td>
    <td>Any</td>
    <td>any</td>
  </tr>

  <tr>
    <td>...</td>
    <td>
    OP_FALSE: '\x00'  (1bytes, ascii)<br>
    OP_RETURN: '\x6a' (1bytes, ascii)<br>
    <b>data:</b><br/>
    &lt;data: modified data&gt; (0 to ∞ bytes)<br/>
    <td>any</td>
  </tr>

</table>




### Examples

**CREATE Future School Transaction**

blockchain transaction:  a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d87b20d2502682647

SCRIPT: ``006a04534c500001010747454e45534953045553445423546574686572204c74642e20555320646f6c6c6172206261636b656420746f6b656e734168747470733a2f2f7465746865722e746f2f77702d636f6e74656e742f75706c6f6164732f323031362f30362f546574686572576869746550617065722e70646620db4451f11eda33950670aaf59e704da90117ff7057283b032cfaec77793139160108010208002386f26fc10000``

**UPDATE Future School Transaction**

blockchain transaction: 6b73adfbe7e5688c53ea4b09bf37de85dfd6dd4e3d38d1c0b4a5b38a9c0ca613

SCRIPT: ``006a04534c50000101044d494e5420a26d3191f2be3dc7fffdfa95ad7dc1bc3614079ebd626e0d87b20d2502682647010208002386f26fc10000``


# Copyright

This protocol specification is published under the terms of the MIT license.
