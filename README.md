# SLP++ Layer II Protocol
Layer II protocol, Safety & Simple base on original  [SLP.](https://github.com/simpleledger/slp-specifications)  
SLP++ operate on non-stand UTXOs,which can includes any state of your business.      
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
   &lt;lokad_id: 'SLP++\x00'&gt; (6 bytes, ascii)<sup>2</sup><br/>
   &lt;metadata: &gt; (0 to ∞ bytes)<br/>
   &lt;key: &gt; (0 to ∞ bytes)<br/>
   </td>
    <td>  > 0 </td>
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
    OP_RETURN <sup>3</sup>: data (0 to  ∞ bytes)</td>
    <td>0</td>
  </tr>
 
</table>

<sup>1. The Lockscript can be any valid script combination (more [spec](https://github.com/bitcoin-sv-specs/protocol/blob/master/updates/genesis-spec.md)). </sup>   
<sup>2. The OP_RETURN should be metadata & key managment for  specific business. </sup>   
<sup>3. The OP_FALSE OP_RETURN vout should be common data for  specific business. </sup>   


## Protocol

### SLP++ Blockdrive Storage  Protocol
[slp++ blockdrive](./slppp-blockdrive.md)

### SLP++ Electronic Contract Protocol
[slp++ contract](./slppp-contract.md)

### SLP++ Onchain Message Protocol
[slp++ message](./slppp-message.md)

### SLP++ Utility Token Protocol
[slp++ token-type-1](./slppp-token-type-1.md) compared to original SLP, SLP++ has below advantages:
```
1. Safety,avoid the chance of spent utxo which contain tokens.
2. Simple,one transaction outputs attached one or more op_returns, no addidtional rules compared to old op_return style.
3. Free order of transaction outputs.
```


## Tools  
coming soon  