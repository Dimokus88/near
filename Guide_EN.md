<div align="center">
  
![pba](https://user-images.githubusercontent.com/23629420/163564929-166f6a01-a6e2-4412-a4e9-40e54c821f05.png)
  
# Near Stake Wars on Akash Network!

| [Akash Network](https://akash.network/) | [Forum Akash Network](https://forum.akash.network/) | 
|:--:|:--:|
  
___
  
Technical support and our news channels:

| [Discord Akash](https://discord.gg/WR56y8Wt) | [Telegram Akash EN](https://t.me/AkashNW) | [TwitterAkash](https://twitter.com/akashnet_) |
|:--:|:--:|:--:|

</div>
<div align="center">
  
| [Discord NEAR](https://discord.gg/gjD9P3RDer) | [Explorer Shardnet](https://explorer.shardnet.near.org/) | [Site NEAR](https://near.org/) | [Twitter NEAR](https://twitter.com/nearprotocol) |
|:--:|:--:|:--:|:--:|
  
</div>

___

***Near*** launched the ***Stake Wars***, the purpose of which is to create validators that will create small snapshots of the network - chunks. [Here ***Near*** posts challenges](https://github.com/near/stakewars-iii/tree/main/challenges). As a result of the event, validators for the main network will be selected.

___

#### Сontents:
1. [Setup](https://github.com/Dimokus88/near/blob/main/Guide_EN.md#setup).
2. [Create and register a validator](https://github.com/Dimokus88/near/blob/main/Guide_EN.md#create-and-register-a-validator).
3. [Load your validator_key.json](https://github.com/Dimokus88/near/blob/main/Guide_EN.md#load-your-validator_keyjson).

___

## Setup

### Install Akashlytics

Instructions for installing and configuring ***Akashlytics*** [available here](https://github.com/Dimokus88/guides/blob/main/Akashlytics/EN-guide.md).

### Creating an account (wallet) in the network Near - Shardnet.

Instructions for creating an account (wallet) [on the Near - Shardnet network](https://github.com/Dimokus88/near/blob/main/Shardnet_wallet.md#create-a-wallet-on-the-near---shardnet-network).
  
___

### Create and register a validator.

* Deploy the container using ***this [deploy.yml](https://github.com/Dimokus88/near/blob/main/deploy.yml)***, don't forget to specify the ***root*** password .

>P.S. If you already have ***validator_key.json*** then just [insert direct link](https://github.com/Dimokus88/near/blob/main/Guide_EN.md#load-your-validator_keyjson) download to the ```link_key``` variable and run the deployment, ***nothing else to do!***

* We are waiting for this message in the `LOGS`-`LOGS` tab:
  
<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180196355-6df94bdc-e778-4db6-bb4f-c7306c1579c7.png" width=60% </p>

* We connect via `ssh` to a running container using the parameters specified in `LEASES`:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180058824-bd322297-39a1-4111-bcde-7b2d49dfc50e.png" width=50% </p>   

The user is ***root**, the password is the one you specified in the ***manifest (deploy.yml)***:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180059094-c278dd3a-f7c6-4eb1-8df3-493429af91ed.png" width=50% </p>   

At this stage, the node is deployed and  ***has already begun to synchronize*** , you can view the node logs with the command:
  
```
tail -30 /root/.near/nohup.err
```
  
* To register a validator, enter the command:
  
```
near login
``` 
  
and answer the request with `y` (yes) , a link will be returned in response:
  

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180059646-235c2fd5-805b-4288-9e42-771586a92355.png" width=60% </p>   

* Follow the link in the browser where the account is opened in the ***near*** test network and confirm the link between the node and the account:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180060700-55aaa8b6-f43f-4d20-8877-aeae354356ce.png" width=40% </p>   

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180060751-7ca65aa2-e39e-4321-8f10-842b0467913b.png" width=40% </p>   

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180060903-3b5204bb-c2f5-43ba-84c9-c6f5557c9013.png" width=40% </p>   

When you see such a message in response - everything is in order, return to the terminal:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180062810-e94ccfe3-7c4e-4b4b-b456-f80a59006f30.png" width=50% </p>   

* Enter the name of your account, in response you will receive a message stating that the account is linked:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180063083-d6dd3e1a-af55-42a1-a7ec-9d288a88d368.png" width=60% </p>   

* Now you need to create validator keys (***validator_key.json***), do the command where ***XX is your account id***, for me the command will look like this: `near generate-key akash_user.factory.shardnet. near`:
```
near generate-key xx.factory.shardnet.near
```
  
In response, you will receive a message stating that a key pair has been created:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180063876-ad6dc292-46e5-45cd-a74f-6d193a19ecd6.png" width=80% </p>   

* Copy the wallet file to the `.near` folder, where ***XX is your account id***. :
```
cp ~/.near-credentials/shardnet/ХХ.shardnet.near.json ~/.near/validator_key.json
```

* Modify the `validator_key.json` file:
```
nano ~/.near/validator_key.json
```

In the `account_id` field add `.factory` to get `XX.factory.shardnet.near.json` where ***XX is your account id***.

Change the name of the field from `private_key` to `secret_key`.

* Save changes: press `ctrl+x`, then `y` and `enter`. Create a validator with the command:
```
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "<pool id>", "owner_id": "<accountId>", "stake_public_key": "<public key>", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="<accountId>" --amount=30 --gas=300000000000000
```

where:
 
`<pool id>` - your account name (in my example `akash_user`).
  
`<accountId>` - full account Id (in my example `akash_user.shardnet.near`).
  
`<public key>` - public key from `validator_key.json`.
    
<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180076787-2d0d845c-921b-4ac8-8701-5cdb8e940975.png" width=80% </p>   
    
On a successful command, you will receive a response with a link in explorer, [***example***](https://explorer.shardnet.near.org/transactions/V4a6mJzP71PGBKmuqFcwwySFgdXWiLkux4UFfTtzezw):
    
<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180077001-34055547-683a-41d4-aff7-e834e89398d7.png" width=60% </p>   
    
* To delegate additional tokens, you need to run the command:
```
near call <staking_pool_id> deposit_and_stake --amount <amount> --accountId <accountId> --gas=300000000000000
```

where:
    
`<staking_pool_id>` is your validator name (in my example `akash_user.factory.shardnet.near`).
  
`<accountId>` - full account Id (in my example `akash_user.shardnet.near`).
  
`<amount>` - the number of tokens for the delegation.

In response, the link in explorer will also be received [***example***](https://explorer.shardnet.near.org/transactions/CRUdCmQ7tAh8vFk7QkDRhu8iRH9SVtP6RrGKhgEnRE5Q):

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180078082-97aed13b-d040-4521-9c3d-423f8b9b7ab7.png" width=60% </p>
    
***The validator has been created***, don't forget to save the `validator_key.json` file locally, this can be done by copying the output of the command:
```
cat ~/.near/validator_key.json | jq
```
  
By default, ***Akash Network*** providers use ***ephemeral data storage***, which means that when the container is restarted, all data can be ***lost*** in order to avoid the node losing your ` validator_key.json` it is recommended to follow the steps from [next paragraph](https://github.com/Dimokus88/near/blob/main/Guide_EN.md#load-your-validator_keyjson).  
  
[To start](https://github.com/Dimokus88/near/blob/main/Guide_EN.md#contents).

___
  
  
## Load your validator_key.json
  
To load an existing `validator_key.json` file, you can use the `link_key` built-in variable in [deploy.yml](https://github.com/Dimokus88/near/blob/main/deploy.yml) . One way is to download using ***Google drive***, now we will consider it:

I could advise you to use a ready-made link generator from the Internet, but this is a private key, so it's safer if you create a link yourself:  

Access the file on `google` drive and copy its link, it will look like this:
`https://drive.google.com/open?id=xxxxxxxxxxxxxx-xxxxxxxxxxxx&authuser=gmail%40gmail.com&usp=drive_fs`
 you need to take the part: `id=xxxxxxxxxxxxxx-xxxxxxxxxxxx` and insert before it: `https://drive.google.com/uc?export=download&`.
Thus, you will get a link to a ***direct download*** file:
`https://drive.google.com/uc?export=download&id=xxxxxxxxxxxxxx-xxxxxxxxxxxx` .
  
Then, go to your deployment in ***Akashlytics***, `UPDATE` tab. Uncomment the `link_key` line (remove the "#" symbol) and paste your link after the "=" symbol.
  
![image](https://user-images.githubusercontent.com/23629420/180197150-3c9d7026-cd19-41c9-be22-d83270d309c0.png)
  
After that, press `UPDATE DEPLOYMENT` (or `CREATE DEPOYMENT`), confirm the transaction and your container will be deployed as a validator node. And if the container is reloaded, then your `validator_key.json` will be downloaded and the node will start correctly.
  
[To start](https://github.com/Dimokus88/near/blob/main/Guide_EN.md#contents).

___
