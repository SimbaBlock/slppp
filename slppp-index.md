# SLP++ Layer II Protocol Specification

`protocol_catalog` :   

* main_catalog: 1 byte ascii
* sub_catalog:  1 byte ascii  
```
current catalog: 
  0x01, [token/payments](./token);
     0x01, utilty token
     0x02, nft token	   
  0x02, [enterprise](./enterprise)
     0x01, contract
     0x02, message
     0x03, blockdrive
  0x03, [enduation](./edu)
     0x01,training 
     0x02,mooc    
  0x04, [medical](./medical) 
     0x01, ehr
```