---
title: SolanaAPI
cover: ''
date: 2023-09-21 16:58:54
updated: 2023-09-21 16:58:54
tags:WEB3
categories: WEB3
---




## solana-api

### id.json 私钥导入
id.json import




### Demo
Doc:
https://github.com/solana-developers/web3-examples


#### TX
```

// Create transaction
const tx = new web3.Transaction({
  ...blockhashInfo,
});

// console.log(tx)

// Add our hello world program instruction
tx.add(
  new web3.TransactionInstruction({
    programId: pg.PROGRAM_ID,
    keys: [],
    data: Buffer.from([]),
  })
);
// Sign transaction
tx.sign(pg.wallet.keypair);
// Send the transaction to the Solana cluster
const txHash = await pg.connection.sendRawTransaction(tx.serialize());
console.log(txHash);
// this txHash can view in 

```