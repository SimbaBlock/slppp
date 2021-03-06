# SLP++ UTXO Protocol
UTXO based Protocol, Safety & Simple base on original  [SLP.](https://github.com/simpleledger/slp-specifications)  
SLP++ operate on non-standard UTXOs,which can include any state of your services.      
Layer II's services are identified by transaction output: service id = sha256(32 bytes txid + 1 byte integer of n), which convert service from transaction scope to script scope. so we can write any complex scripts to implement our services. 
## Transaction struture

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
   <b>Lockingscript<sup>1</sup></b>:</br>
   'OP_DUP OP_HASH160 986b57ea26555d28c OP_EQUALVERIFY OP_CHECKSIG' (0 to ∞ bytes)<br/>   
   OP_RETURN: '\x6a' (1 byte, ascii)<br/>
   &lt;len: &gt; (length of metadata)<br/>
   len < 0x4c : (1 byte integer) <br/> 
   len <= 0xff : ('4c' + 1 byte integer) <br/> 
   len <= 0xffff : ('4d' + 2 bytes integer)<br/> 
   len <= 0xffffffff : ('4e' + 4 bytes integer) <br/> 
   <b>metadata<sup>2</sup></b>: <br/>
   &lt;protocol: 'SLP++\x00'&gt; (6 bytes, ascii)<br/>
   &lt;protocol_id<sup>3</sup>:\x0000 &gt; (2 bytes integer)<br/>
   &lt;protocol_version:\x00 &gt; (1 byte integer)<br/>
   &lt;data_hash: &gt; (32 bytes, sha256(data))<br/>
   &lt;key: &gt; (0 to ∞ bytes)<br/>
   </td>
   <td>>0</td>
  </tr>
  
  <tr>
    <td>...</td>
    <td>Any</td>
   <td>>any</td>
  </tr>
  
  <tr>
    <td>...</td>
    <td>
    OP_FALSE : '\x00' (1 byte, ascii)<br>
    OP_RETURN: '\x6a' (1 byte, ascii)<br> 
   &lt;len: &gt; (length of data)<br/>
   len < 0x4c : (1 byte integer) <br/> 
   len <= 0xff : ('4c' + 1 byte integer) <br/> 
   len <= 0xffff : ('4d' + 2 bytes integer) <br/> 
   len <= 0xffffffff : ('4e' + 4 bytes integer) <br/> 
   <b>data<sup>4</sup></b>:<br/>
   &lt;data: &gt; (0 to ∞ bytes)<br/>
    </td>
    <td>0</td>
  </tr>
 
</table>

<sup>1. The <b>Lockingscript</b> can be any valid script by combination op_codes(more [spec](https://github.com/bitcoin-sv-specs/protocol/blob/master/updates/genesis-spec.md)), which controls who can spend the utxo. </sup>   
<sup>2. The OP_RETURN should be <b>metadata</b> for specific services. </sup>   
<sup>3. see more [protocol](./index.md). </sup>   
<sup>4. The OP_FALSE OP_RETURN vout should be common <b>data</b> for specific services. </sup>   


## SLP++ Protocol
The complete Protocol see [more](./index.md)      

### [01-Common](./common/)  

### [02-Token](./token/)

### [03-Enterprise](./enterprise/)  

### [04-Education](./education/)  

### [05-Health](./health/)  



## Tools  
coming soon
```
1. UTXO(includes non-standard) management tools.  
2. write complex script for non-standard transaction outputscript.
```
