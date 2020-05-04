# SLP++ Layer II Protocol List

`protocol type`: 2 bytes integer. 

```
  common: 0x01
     message: 0x01
     blockdrive: 0x02
     project: 0x03
     blog: 0x04

  token/payments: 0x02
     utility token: 0x01
     nft token: 0x02 	   

  enterprise: 0x03
     contract: 0x01

  enduation: 0x04
     training: 0x01 
     mooc: 0x02    

  health: 0x05
     ehr: 0x01
```  
For example, the type 0x0101 is [message](./common/slppp-message.md), the type 0x0301 is [contract](./common/slppp-contract.md) etc.  