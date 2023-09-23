---
title: WEB3-SolanaContract
cover: 'https://plus.unsplash.com/premium_photo-1693239593009-41630af47610?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxfDB8MXxyYW5kb218MHx8fHx8fHx8MTY5NTQzMjczMA&ixlib=rb-4.0.3&q=80&w=1080'
date: 2023-09-23 09:11:55
updated: 2023-09-23 09:11:55
tags: Solana
categories: WEB3
---



## HelloWorld Contract Code Review
### Hello World Contract Code Review

From：[Github](https://github.com/solana-labs/example-helloworld/blob/master/src/program-rust/src/lib.rs)

```
use borsh::{BorshDeserialize, BorshSerialize};  
use solana_program::{  
account_info::{next_account_info, AccountInfo},  
entrypoint,  
entrypoint::ProgramResult,  
msg,  
program_error::ProgramError,  
pubkey::Pubkey,  
};
```
In the first section, the program imports some other codes. From code above, we can see that the program mainly use borsh and solana_program. Borsh is a tool used to serialize and deserialize data. Solana_program is a tool for program to interact with Solana chain. In Rust, these libraries are uploaded to crate. To import them, you just simply including these in cargo.toml as dependencies. Then, it can be installed through cargo command. Then, you can just simply import them as code above.

```
/// Define the type of state stored in accounts  
#[derive(BorshSerialize, BorshDeserialize, Debug)]
pub struct GreetingAccount { /// number of greetings  
pub counter: u32,  
}
```

Now, the program defines a struct called GreetingAccount. This is used to store a public variable called counter, which is a variable used to trace number of hellos a client has sent to this program. Public variable means it can be publicly accessed by simply calling it. This struct has inherited the functions of BorshSerailize, BorshDeserialize and Debug. It means any functions of these 3 can also be used by GreetingAccount.

In Solana, this kind of changing variables need to be stored in a separate account. So, a user needs to provide a storage account for the program to store these data into. We would get into this shortly.

Now, let’s move to the main logic of the contract.

```
// Declare and export the program's entrypointentrypoint!(process_instruction); 
// Program entrypoint's implementationpub 
fn process_instruction(    
program_id: &Pubkey, // Public key of the account the hello world    program was loaded into    
accounts: &[AccountInfo], // The account to say hello to         
_instruction_data: &[u8], // Ignored, all helloworld instructions are hellos
)
```

In Solana, every program needs to define an entry point. All Solana program takes 3 parameters in entry point: (1) public key of the program, (2) a list of accounts that the program can write to and (3) instruction data.

(1) above is easy to understand. It basically is the program id of your program. (2) is a bit confusing. So, as mentioned above, your program needs some accounts for them to keep track of some changing data. In this case, the changing data is the counter variable. If you need more than one accounts, the account info would be put in as an array and pass to the program. For (3), we may ignore instruction data as all instructions are just simply hello. Now, let’s move to ProgramResult:
```
-> ProgramResult{msg!("Hello World Rust program entrypoint");     // Iterating accounts is safer then indexing  
let accounts_iter = &mut accounts.iter();     // Get the account to say hello to  
let account = next_account_info(accounts_iter)?;     
```


Then, the program result would be the major logic. After the welcome message, the first thing the program does is to make the accounts array iterable. So that the program can go through the array by using next_account_info() function. Again, this program only needs one account. So, we just need the first element of the account info array.
```
// The account must be owned by the program in order to modify its data  
if account.owner != program_id {          
msg!("Greeted account does not have the correct program id");        
return Err(ProgramError::IncorrectProgramId);      
}
```

As mentioned earlier, the program has to be owner of the account in order to amend any data in it. So, you need to check if the account owner points to the program. Otherwise, return an error message.
```
// Increment and store the number of times the account has been greetedlet mut greeting_account = GreetingAccount::try_from_slice(&account.data.borrow())?;    
greeting_account.counter += 1;      
greeting_account.serialize(&mut &mut account.data.borrow_mut()[..])?;       
msg!("Greeted {} time(s)!", greeting_account.counter);       
Ok(())  
}
```

Now, grab the data from the account provided by the user and use try_from_slice() to deserialize it. try_from_slice() is a function defined in BorshDeserialize. Now, increment the counter variable from the account by 1. Then, serialize the updated counter variable and put it back to the account. Lastly, the program call the updated counter variable from the account and print it out.

### Hello World Contract API
From:[GitHub](https://github.com/solana-labs/example-helloworld/blob/master/src/client/main.ts) 

#### Run The Program
```
git clone https://github.com/solana-labs/example-helloworld.git
cd example-helloworld/
npm install


solana address
solana balance address # if balance not enough ,need to get airdrop

npm run build:program-rust
solana program deploy dist/program/helloworld.so
npm run start # Can see the hello result
```


#### Program Flow
So, let’s go through main.ts first. Its main() function in main.ts looks like below:
```
async function main()   
{    
console.log("Let's say hello to a Solana account...");     
// Establish connection to the cluster    
await establishConnection();     
// Determine who pays for the fees    
await establishPayer();     
// Check if the program has been deployed    
await checkProgram();     
// Say hello to an account    
await sayHello();     
// Find out how many times that account has been greeted    
await reportGreetings();     
console.log('Success');}
```

So, it basically outline the flow of the program:

1. Establish connection to cluster;
2. Get ready to pay lamport;
3. Say hello;
4. print number of hellos.

You may notice that there is a checkProgram() function too. However, in this passage, I would skip this part as I want to focus more on how a client program interact with a Smart Contract.

Ok. Now, let’s go through each step one by one:
- 1. Establish connection to cluster
```
//utils.tsasync function getConfig(): Promise<any>   
{    
// Path to Solana CLI config file    
const CONFIG_FILE_PATH = path.resolve(      
os.homedir(),      
'.config',      
'solana',      
'cli',      
'config.yml',    
);    
const configYml = await fs.readFile(CONFIG_FILE_PATH, {encoding: 'utf8'});    
return yaml.parse(configYml);  
}export async function getRpcUrl(): Promise<string>   
{    
 try   
 {      
   const config = await getConfig();      
   if (!config.json_rpc_url) throw new Error('Missing RPC URL');            
   return config.json_rpc_url;    
 } catch (err)   
 {      
   console.warn(        
   'Failed to read RPC url from CLI config file, falling back to   
   localhost',      
   );      
   return 'http://localhost:8899';    
 }  
}//helloworld.tsexport async function establishConnection(): Promise<void>   
{    
const rpcUrl = await getRpcUrl();    
connection = new Connection(rpcUrl, 'confirmed');    
const version = await connection.getVersion();  console.log('Connection to cluster established:', rpcUrl, version);  
}
```

So, main logic of establish connection is situated in helloworld.ts. It utilize a function in util.ts called getRpcUrl() to look for current config info. getRpcUrl() calls another function within utils.ts named getconfig() to get the information in ~/.config/solana/cli/config.yml. If you open this file in your local environment, it looks something like this:
```
cat /root/.config/solana/cli/config.yml  
---  
json_rpc_url: "http://127.0.0.1:8899"  
websocket_url: ""  
keypair_path: /root/wallet/wallet1.json  
address_labels:  
"11111111111111111111111111111111": System Program

```
So, it basically stores all your config info.
Now, getRpcURL() function tries to read the json_rpc_url parameter and return it as a result. This parameter basically indicates which cluster your environment is connected to. In above case, you are connected to a local cluster. If it is empty, it would throw error and it would return local cluster value by default. Finally, establishConnection() function would capture this value and establish connection to the cluster.

- 2. Get ready to pay lamport
This step is executed in the establishedPayer() function in helloworld.ts. Solana is just like other Blockchain. You need to pay fee for a transaction. In this client program, it will call for airdrop if the account is not sufficient to pay for the transaction. Now, we will go through the related code in 2 parts:
```
export async function establishPayer(): Promise<void>   
{    
 let fees = 0;    
 if (!payer) {      
 const {feeCalculator} = await connection.getRecentBlockhash();       
 // Calculate the cost to fund the greeter account      
 fees += await  connection.getMinimumBalanceForRentExemption(GREETING_SIZE);       
 // Calculate the cost of sending transactions      
 fees += feeCalculator.lamportsPerSignature * 100;    
 payer = await getPayer();    
}
```

So, the first part just calculate the lamport needed to send the hello transaction. So, we can get an airdrop if fund is insufficient. Getpayer() function is an function imported from util.ts which is used to return the keypair of your account. Code of getpayer() is as per below:
```
export async function getPayer(): Promise<Keypair> {    
try {      
 const config = await getConfig();      
 if (!config.keypair_path) throw new Error('Missing keypair path');       
 return await createKeypairFromFile(config.keypair_path);  }   
catch (err) {      
 console.warn(        
   'Failed to create keypair from CLI config file, falling back to   
   new random keypair',      
 );      
return Keypair.generate();    
}  
}
```

Then, second part of estabilishdPayer() function is simple. Just request an airdrop if its balance is insufficient to fund the transaction and a log message at the end:
```
let lamports = await connection.getBalance(payer.publicKey);    
if (lamports < fees) {      
// If current balance is not enough to pay for fees, request an airdrop      
const sig = await connection.requestAirdrop(        
   payer.publicKey,        
   fees - lamports,      
   );      
await connection.confirmTransaction(sig);      
lamports = await connection.getBalance(payer.publicKey);    
}console.log(      
 'Using account',   
 payer.publicKey.toBase58(),   
 'containing',      
 lamports / LAMPORTS_PER_SOL,      
 'SOL to pay for fees',  );
```

- 3. Say hello
So, now the we move to the most interesting part. So, let’s take a look:
```
const GREETING_SEED = 'hello';    
greetedPubkey = await PublicKey.createWithSeed(      
payer.publicKey,      
GREETING_SEED,      
programId,  );
```
The above code is placed in helloworld.ts. As mentioned in [previous post](https://medium.com/@lianxiongdi/solana-development-6-understanding-how-smart-contract-works-in-solana-d60bc3d9f673), user needs to feed an account to the program to store the counter variable. The above code is written exactly to serve this purpose. It uses a function imported from web3.js named createWithSeed(). This is a way to generate a public key using a seed word (i.e. “hello” from above example) and a program ID. The program ID mentioned would also be the owner of this account so the program can amend data in this account at will. You can find some more details of this function in its [doc](https://solana-labs.github.io/solana-web3.js/classes/PublicKey.html).

So, now, we can use the greetedPubkey to send an instruction to instruct the program to update the account.
```
export async function sayHello(): Promise<void> {       
   console.log(  
      'Saying hello to',   
      greetedPubkey.toBase58()  
   );    
   const instruction = new TransactionInstruction({      
      keys: [{pubkey: greetedPubkey, isSigner: false, isWritable: true}],      
      programId,      
      data: Buffer.alloc(0), // All instructions are hellos    
   });    
   await sendAndConfirmTransaction(      
      connection,      
      new Transaction().add(instruction),      
      [payer],    
   );  
}
```

The code above is used to construct an instruction to interact with the program. After a greeting message, it sends a transaction with keys about account info, programId and data to the cluster. These information is exactly matched with entrypoint of the program as mentioned in [previous post](https://medium.com/@lianxiongdi/solana-development-6-understanding-how-smart-contract-works-in-solana-d60bc3d9f673). The keys array in it mainly indicates the account parameters. Because if you remember, a program cannot call for account data. It can only relies on client to provide such information. Finally, the instruction is sent out using sendAndConfirmTransaction() function imported from web3.js.

-  4. print number of hellos
It is all worked in reportGreetings() function in helloworld.ts. It first get account info using connection.getAccountInfo() function provided by web3.js and extract the information from the account, deserialise it and print it out. Major code is as below:
```
export async function reportGreetings(): Promise<void> {    
const accountInfo = await connection.getAccountInfo(greetedPubkey);  
if (accountInfo === null) {      
   throw 'Error: cannot find the greeted account';    
}    
const greeting = borsh.deserialize(      
   GreetingSchema,      
   GreetingAccount,      
   accountInfo.data,    
);    
console.log(      
   greetedPubkey.toBase58(),      
   'has been greeted',      
   greeting.counter,      
   'time(s)',  );  
}

```

The most important part in above code is how we use borsh.deserialize() function to deserialise. In borsh deserialisation, you will create an object to store the deserialsed values. In here, the object is called greeting. In order to construct this object, you need a class to define properties of this object (i.e. GreetingAccount in code above). Also, you need to define a Schema to map the deserialised data to the object (i.e. GreetingSchema in above code) and data to be deserialised. If you remember from [previous post](https://medium.com/@lianxiongdi/solana-development-6-understanding-how-smart-contract-works-in-solana-d60bc3d9f673), the data structure to be deserialized is like this:
```
GreetingAccount {         
pub counter: u32,  
}

```

So, the GreetingSchema code would be as below:
```
/** * Borsh schema definition for greeting accounts */  
const GreetingSchema = new Map([  [GreetingAccount, {kind: 'struct', fields: [['counter', 'u32']]}],]);

```

Easy. Right? Just map GreetingAccount to a struct type data structure as above. Now, let’s take a look of GreetingAccount class:
```
/** * The state of a greeting account managed by the hello world program */class GreetingAccount {    
counter = 0;constructor(fields: {counter: number} | undefined = undefined) {         
   if (fields) {        
      this.counter = fields.counter;      
   }    
}  
}
```

So, borsh.deserialize() function will pass the deserialised data to the class to construct a new object. So, what the class does is just give it a counter property and set to 0 by default. Then, it contains a constructor which would create an object once the data arrives. After that, it read the data and put it back into counter variable.





### More Contract
- https://github.com/solana-labs/solana-program-library