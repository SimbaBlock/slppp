# SLP++ Layer II Protocol
Layer II Protocol, Safety & Simple base on original  [SLP.](https://github.com/simpleledger/slp-specifications)  
SLP++ operate on non-standard UTXOs,which can includes any state of your business.      
Layer II's service is identified by transaction outputscript, which convert to script level from transaction level. 
## Transaction struture

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
   OP_RETURN<sup>2</sup>: '\x6a' (1 bytes, ascii)<br/>
   &lt;lokad_id: 'SLP++\x00'&gt; (6 bytes, ascii)<br/>
   &lt;data_hash: &gt; (32 bytes,sha256(data))<br/>
   &lt;metadata: &gt; (1 to ∞ bytes)<br/>
   &lt;key: &gt; (0 to ∞ bytes)<br/>
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
    OP_FALSE : '\x00' (1 bytes, ascii)<br>
    OP_RETURN<sup>3</sup>: '\x6a' (1 bytes, ascii)<br> 
   &lt;data: &gt; (0 to ∞ bytes)<br/>
    </td>
    <td>0</td>
  </tr>
 
</table>

<sup>1. The Lockscript can be any valid script by combination op_codes.(more [spec](https://github.com/bitcoin-sv-specs/protocol/blob/master/updates/genesis-spec.md)). </sup>   
<sup>2. The OP_RETURN should be metadata & key managment for specific business. </sup>   
<sup>3. The OP_FALSE OP_RETURN vout should be common data for specific business. </sup>   


## SLP++ Protocol

### [Blockdrive](./slppp-blockdrive.md)  

### [Contract](./slppp-contract.md)  

### [Message](./slppp-message.md)  

### [Utility Token](./slppp-token-type-1.md)  


## Tools  
coming soon
```
1. non-standard UTXO management tools.  
2. write complex script for non-standard transaction outputscript.
```