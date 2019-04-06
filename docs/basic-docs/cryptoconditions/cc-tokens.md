# Contract Module: Tokens

## Introduction

The `tokens` Crypto Conditions smart contract enables core-asset support for the on-chain creation of colored coins, also called tokens. The functionality is facilitated by utxo technology. Tokens can be generated on any chain where the [ac_cc](../installations/asset-chain-parameters.html#ac-cc) is enabled.

The `tokens` smart contract requires locking a proportional amount of satoshis of the native coins. These satoshis create the supply for the token.

For example, if you desire to create a one-of-a-kind token, use 1 satoshi in its creation.

There is also a support for non-fungible tokens. Each non-fungible token has the amount of 1 and contains additional array of data about itscorresponding asset. These data has a evalcode inside which binds this non-fungible token to a cc contract responsible for validation. The `tokeninfo` method allows to see if a token is non-fungible. 

## tokenaddress

**tokenaddress (pubkey)**

The `tokenaddress` method returns information about a token address according to a specific `pubkey`. If no `pubkey` is provided, the `pubkey` used to the launch the daemon is the default.

### Arguments:

| Structure | Type               | Description                       |
| --------- | ------------------ | --------------------------------- |
| pubkey    | (string, optional) | the pubkey of the desired address |

### Response:

| Structure       | Type     | Description                                                                                                                      |
| --------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------- |
| result          | (string) | whether the command executed successfully                                                                                        |
| AssetsCCaddress | (string) | taking the token contract's EVAL code as a modifier, this is the public address that corresponds to the token contract's privkey |
| Assetsmarker    | (string) | the unmodified public address generated from the token contract's privkey                                                        |
| CCaddress       | (string) | taking the token contract's EVAL code as a modifier, this is the CC address from the pubkey of the user                          |
| myCCaddress     | (string) | taking the token contract's EVAL code as a modifier, this is the CC address from the pubkey of the user                          |
| myaddress       | (string) | the public address of the pubkey used to launch the chain                                                                        |

#### :pushpin: Examples:

Command:

```bash
./komodo-cli -ac_name=HELLOWORLD tokenaddress 028702e30d8465d6aa85f35d2f58c06a6ee17f23f376b56044dadf7b793f2c12b9
```

Response:

```json
{
  "result": "success",
  "AssetsCCaddress": "RGKRjeTBw4LYFotSDLT6RWzMHbhXri6BG6",
  "Assetsmarker": "RFYE2yL3KknWdHK6uNhvWacYsCUtwzjY3u",
  "CCaddress": "RG6mr23tQ9nUhmi5GEnYqjfkqZt9x2MRXz",
  "myCCaddress": "RG6mr23tQ9nUhmi5GEnYqjfkqZt9x2MRXz",
  "myaddress": "RDjG4sM1y4udiJSszF6BLotqUnZX79Rom9"
}
```

## tokenbalance

**tokenbalance tokenid (pubkey)**

The `tokenbalanced` method checks the token balance according to a provided `pubkey`. If no `pubkey` is provided, the `pubkey` used the launch the daemon is the default.

### Arguments:

| Structure | Type     | Description                                                                                                                |
| --------- | -------- | -------------------------------------------------------------------------------------------------------------------------- |
| tokenid   | (string) | the txid that identifies the token                                                                                         |
| pubkey    | (string) | the pubkey for which to examine the balance; if no pubkey is provided, the pubkey used to launch the daemon is the default |

### Response:

| Structure | Type     | Description                                                                                             |
| --------- | -------- | ------------------------------------------------------------------------------------------------------- |
| result    | (string) | whether the command executed succesfully                                                                |
| CCaddress | (string) | taking the token contract's EVAL code as a modifier, this is the CC address from the pubkey of the user |
| tokenid   | (string) | the txid that identifies the token                                                                      |
| balance   | (number) | the balance of the address that corresponds to the pubkey                                               |

#### :pushpin: Examples:

Command:

```bash
./komodo-cli -ac_name=HELLOWORLD tokenbalance c5bbc34e6517c483afc910a3b0585c40da5c09b7c5d2d9757c5c5075e2d41b59
```

Response:

```json
{
  "result": "success",
  "CCaddress": "RRPpWbVdxcxmhx4xnWnVZFDfGc9p1177ti",
  "tokenid": "c5bbc34e6517c483afc910a3b0585c40da5c09b7c5d2d9757c5c5075e2d41b59",
  "balance": 99989
}
```

Check the token balance of a specific pubkey

```bash
./komodo-cli -ac_name=HELLOWORLD tokenbalance c5bbc34e6517c483afc910a3b0585c40da5c09b7c5d2d9757c5c5075e2d41b59 028bb4ae66aa4f1960a4aa822907e800eb688d9ab2605c8067a34b421748c67e27
```

Response:

```json
{
  "result": "success",
  "CCaddress": "RQymbXA8FfWw2AaHv7oC8JRKo9W5HkFVMm",
  "tokenid": "c5bbc34e6517c483afc910a3b0585c40da5c09b7c5d2d9757c5c5075e2d41b59",
  "balance": 999900011
}
```


## tokencreate

**tokencreate name supply description**

The `tokencreate` method creates a new token.

For every token created, the method requires one satoshi of the parent blockchain's coins. For example, `1 COIN` of the blockchain provides `100000000` tokens.

The method returns a hex-encoded transaction which should then be broadcast using `sendrawtransaction`.

`sendrawtransaction` then returns a `txid`, which is your `tokenid`.

::: tip
Tokens that can be divided and transferred in fractional amounts can be created too. If you consider 10 tokens as a single unit, then this unit can be named anything and it will be divisible to a single decimal place. This can be handled on the application side as it is just a change in the way of interpreting the numbers.
:::

### Arguments:

| Structure     | Type     | Description                                      |
| ------------- | -------- | ------------------------------------------------ |
| name          | (string) | the proposed name of the token                   |
| supply        | (number) | the intended supply of the token, given in coins |
| "description" | (string) | the description of the token                     |

### Response:

| Structure | Type     | Description                                                                                          |
| --------- | -------- | ---------------------------------------------------------------------------------------------------- |
| result:   | (string) | whether the command succeeded                                                                        |
| hex:      | (string) | a raw transaction in hex-encoded format; you must broadcast this transaction to complete the command |

#### :pushpin: Examples:

Command:

```bash
./komodo-cli -ac_name=HELLOWORLD tokencreate TAK 10 "Testing phase."
```

Response:

```json
{
  "result": "success",
  "hex": "01000000022c223cfc9c3349aed24ca89e44af6fcdb030150443bd6ac55e2080ce4b097c3002000000484730440220316605c400c47e2d5aa6104ac5c5229e71683b8db9482efa1655d257690d338802202344f254b208a6d724f52f4503531cf005a8ca68119bde4b6cb281ab9fccaf1101ffffffff80e66c0c47311449c5effc2782134006f05fd31e79659bc4b0608d7e247e280c0000000049483045022100ec494d3fa5c76fe0382e83980affdfd091509fb4e18b20fff8c095374e6b6bee022015ddaf95dc8b03e8cbba00ff7a377b80a7bd2200a68669718c329c617549757701ffffffff0400a0724e18090000302ea22c8020bc485b86ffd067abe520c078b74961f6b25e4efca6388c6bfd599ca3f53d8dae8103120c008203000401cc1027000000000000232102adf84e0e075cf90868bd4e3d34a03420e034719649c41f371fc70d8e33aa2702acc01f66fa15090000232103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abcac0000000000000000396a37e3632103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc0354414b0e54657374696e672070686173652e00000000"
}
```

Step 2: Broadcast the raw transaction hex

```bash
./komodo-cli -ac_name=HELLOWORLD sendrawtransaction 01000000012c223cfc9c3349aed24ca89e44af6fcdb030150443bd6ac55e2080ce4b097c300200000049483045022100dc83b88f5ed1f01aab7dee8bd8f2b3c0bf83537c9b3cbb0c6ea78ebafdf4c6f60220518440e7f43d24c5733531a8d5a825dbb90e716f7ba20c0d469e7004c1fcc5aa01ffffffff0400ca9a3b00000000302ea22c8020bc485b86ffd067abe520c078b74961f6b25e4efca6388c6bfd599ca3f53d8dae8103120c008203000401cc1027000000000000232102adf84e0e075cf90868bd4e3d34a03420e034719649c41f371fc70d8e33aa2702acc055cbbe15090000232103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abcac0000000000000000396a37e3632103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc0354414b0e54657374696e672070686173652e00000000
```

Response from Step 2: (This is your token ID)

```bash
e4895451cae47f8f10303c3594888b739f044f7c778623318d877e8df365cc66
```

Step 3 (Optional): Use decoderawtransaction to verify the output is sane

```bash
./komodo-cli -ac_name=HELLOWORLD decoderawtransaction 01000000012c223cfc9c3349aed24ca89e44af6fcdb030150443bd6ac55e2080ce4b097c300200000049483045022100dc83b88f5ed1f01aab7dee8bd8f2b3c0bf83537c9b3cbb0c6ea78ebafdf4c6f60220518440e7f43d24c5733531a8d5a825dbb90e716f7ba20c0d469e7004c1fcc5aa01ffffffff0400ca9a3b00000000302ea22c8020bc485b86ffd067abe520c078b74961f6b25e4efca6388c6bfd599ca3f53d8dae8103120c008203000401cc1027000000000000232102adf84e0e075cf90868bd4e3d34a03420e034719649c41f371fc70d8e33aa2702acc055cbbe15090000232103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abcac0000000000000000396a37e3632103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc0354414b0e54657374696e672070686173652e00000000
```

Response:

```json
{
  "txid": "e4895451cae47f8f10303c3594888b739f044f7c778623318d877e8df365cc66",
  "size": 335,
  "version": 1,
  "locktime": 0,
  "vin": [
    {
      "txid": "307c094bce80205ec56abd43041530b0cd6faf449ea84cd2ae49339cfc3c222c",
      "vout": 2,
      "scriptSig": {
        "asm": "3045022100dc83b88f5ed1f01aab7dee8bd8f2b3c0bf83537c9b3cbb0c6ea78ebafdf4c6f60220518440e7f43d24c5733531a8d5a825dbb90e716f7ba20c0d469e7004c1fcc5aa01",
        "hex": "483045022100dc83b88f5ed1f01aab7dee8bd8f2b3c0bf83537c9b3cbb0c6ea78ebafdf4c6f60220518440e7f43d24c5733531a8d5a825dbb90e716f7ba20c0d469e7004c1fcc5aa01"
      },
      "sequence": 4294967295
    }
  ],
  "vout": [
    {
      "value": 10.0,
      "valueSat": 1000000000,
      "n": 0,
      "scriptPubKey": {
        "asm": "a22c8020bc485b86ffd067abe520c078b74961f6b25e4efca6388c6bfd599ca3f53d8dae8103120c008203000401 OP_CHECKCRYPTOCONDITION",
        "hex": "2ea22c8020bc485b86ffd067abe520c078b74961f6b25e4efca6388c6bfd599ca3f53d8dae8103120c008203000401cc",
        "reqSigs": 1,
        "type": "cryptocondition",
        "addresses": ["RRPpWbVdxcxmhx4xnWnVZFDfGc9p1177ti"]
      }
    },
    {
      "value": 0.0001,
      "valueSat": 10000,
      "n": 1,
      "scriptPubKey": {
        "asm": "02adf84e0e075cf90868bd4e3d34a03420e034719649c41f371fc70d8e33aa2702 OP_CHECKSIG",
        "hex": "2102adf84e0e075cf90868bd4e3d34a03420e034719649c41f371fc70d8e33aa2702ac",
        "reqSigs": 1,
        "type": "pubkey",
        "addresses": ["RFYE2yL3KknWdHK6uNhvWacYsCUtwzjY3u"]
      }
    },
    {
      "value": 99889.9996,
      "valueSat": 9988999960000,
      "n": 2,
      "scriptPubKey": {
        "asm": "03fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc OP_CHECKSIG",
        "hex": "2103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abcac",
        "reqSigs": 1,
        "type": "pubkey",
        "addresses": ["RANyPgfZZLhSjQB9jrzztSw66zMMYDZuxQ"]
      }
    },
    {
      "value": 0.0,
      "valueSat": 0,
      "n": 3,
      "scriptPubKey": {
        "asm": "OP_RETURN e3632103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc0354414b0e54657374696e672070686173652e",
        "hex": "6a37e3632103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc0354414b0e54657374696e672070686173652e",
        "type": "nulldata"
      }
    }
  ]
}
```


## tokeninfo

**tokeninfo tokenid**

The `tokeninfo` method reveals information about any token.

### Arguments:

| Structure | Type     | Description                        |
| --------- | -------- | ---------------------------------- |
| tokenid   | (string) | the txid that identifies the token |

### Response:

| Structure   | Type     | Description                                                   |
| ----------- | -------- | ------------------------------------------------------------- |
| result      | (string) | whether the command executed successfully                     |
| tokenid     | (string) | the identifying txid for the token id                         |
| owner       | (string) | the identifying pubkey of the token creator                   |
| name        | (string) | the name of the token                                         |
| supply      | (number) | the total supply of the token                                 |
| description | (string) | the token description provided by the owner at token creation |
| data        | (string,optional) | the data of non-fungible token, in hex |
| IsImported | (string,optional) | if 'yes' this token is imported from another chain |
| sourceChain | (string,optional) | the imported token source chain name |
| sourceTokenId | (string,optional) | the tokenid of the source token for imported token |

#### :pushpin: Examples:

Command:

```bash
./komodo-cli -ac_name=HELLOWORLD tokeninfo 43850dfce744581ef44775086625745adecd628993c5ff4c1c786cfd21009add
```

Response:

```json
{
  "result": "success",
  "tokenid": "43850dfce744581ef44775086625745adecd628993c5ff4c1c786cfd21009add",
  "owner": "03fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc",
  "name": "TAKA",
  "supply": "100000.00000000",
  "description": "Testing phase 3."
}
```

## tokenlist

**tokenlist**

The `tokenlist` method lists all available tokens on the asset chain.

### Arguments:

| Structure | Type | Description |
| --------- | ---- | ----------- |
| (none)    |      |

### Response:

| Structure | Type               | Description                           |
| --------- | ------------------ | ------------------------------------- |
| tokenid   | (array of strings) | the identifying txid for the token id |

#### :pushpin: Examples:

Command:

```bash
./komodo-cli -ac_name=HELLOWORLD tokenlist
```

Response:

```bash
[
  "307c094bce80205ec56abd43041530b0cd6faf449ea84cd2ae49339cfc3c222c",
  "e7d034fb7dbad561c9a86dcbcc64aa89e1d311891b4e7c744280b7de13b1186f",
  "21020a609c162fa2d0bc223acfff14bb0b886743303f5e4a661dade7a69b24a5",
  "c5bbc34e6517c483afc910a3b0585c40da5c09b7c5d2d9757c5c5075e2d41b59",
  "e4895451cae47f8f10303c3594888b739f044f7c778623318d877e8df365cc66",
  "045a31b7e38b1538d111ea87ad9ec53952a70e9a5e8d076f7ed7923d8723f02d",
  "f4131ee56a47273195a899f60a187862aa8e39a974b5a19d860e2fe69f60242f",
  "9217014eae0a83a0b64632f379c1b474859794f9eaf1cf1eecf5804ed6124a5e",
  "9eec77a3e02dec0ca60ead7e8cfb6cb6809c40fe54b804e51d5c6c2a445ffbf3",
  "43850dfce744581ef44775086625745adecd628993c5ff4c1c786cfd21009add"
]
```


## tokentransfer

**tokentransfer tokenid destpubkey amount**

The `tokentransfer` method transfers tokens from one cc address to another.

It is similar to the [sendmany](../komodo-api/wallet.html#sendmany) method used to send coins on the parent chain.

The method returns a raw hex, which must be broadcast using [sendrawtransaction](../komodo-api/rawtransactions.html#sendrawtransaction) to complete the command.

::: tip
The source `txid/vout` needs to be specified as it is critical to match outputs with inputs.
:::

::: tip
A token may be burned by using `tokentransfer` to send to a burn address.
:::

### Arguments:

| Structure  | Type               | Description                                |
| ---------- | ------------------ | ------------------------------------------ |
| tokenid    | (string, optional) | the identifying txid for the token id      |
| destpubkey | (string)           | the pubkey where the tokens should be sent |
| amount     | (number)           | the number of tokens to send               |

### Response:

| Structure | Type     | Description                                                                                          |
| --------- | -------- | ---------------------------------------------------------------------------------------------------- |
| result:   | (string) | whether the command succeeded                                                                        |
| hex:      | (string) | a raw transaction in hex-encoded format; you must broadcast this transaction to complete the command |

#### :pushpin: Examples:

Step 1: Create the rawtransaction

```bash
./komodo-cli -ac_name=HELLOWORLD tokentransfer e4895451cae47f8f10303c3594888b739f044f7c778623318d877e8df365cc66 02ebc786cb83de8dc3922ab83c21f3f8a2f3216940c3bf9da43ce39e2a3a882c92 500000
```

Response:

```json
{
  "result": "success",
  "hex": "01000000023b61e44ce3cedf536b52d8da11faacd041494a078e971551ed4e2bd496bc8da1000000006a4730440220111c67172740c0c2556979fdf84639ba299ff22586ebd220f25aa301f029003f02203da97a2575c0ed1b309774309f5dc952ee305a46cd83e95eae99e3564a1772f6012103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abcffffffff66cc65f38d7e878d312386777c4f049f738b8894353c30108f7fe4ca515489e4000000007b4c79a276a072a26ba067a565802103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc8140c875a14edcbece61a6c18721398c927dc1e4509863e075b3922a8e3a2da6848e037142436e9102b529ee93a9ec618a4c67b63c52790d71812bb94179056913bba100af038001e3a10001ffffffff0420a1070000000000302ea22c8020541be9f843b476373fc18d8c8fab59c98c2c009f49c07fa66b7b431e4142feae8103120c008203000401cce028933b00000000302ea22c8020bc485b86ffd067abe520c078b74961f6b25e4efca6388c6bfd599ca3f53d8dae8103120c008203000401cc28b9486cb2430000232103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abcac0000000000000000246a22e374e4895451cae47f8f10303c3594888b739f044f7c778623318d877e8df365cc6600000000"
}
```

Step 2: Broadcast using `sendrawtransaction`

```bash
./komodo-cli -ac_name=HELLOWORLD sendrawtransaction 01000000023b61e44ce3cedf536b52d8da11faacd041494a078e971551ed4e2bd496bc8da1000000006a4730440220111c67172740c0c2556979fdf84639ba299ff22586ebd220f25aa301f029003f02203da97a2575c0ed1b309774309f5dc952ee305a46cd83e95eae99e3564a1772f6012103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abcffffffff66cc65f38d7e878d312386777c4f049f738b8894353c30108f7fe4ca515489e4000000007b4c79a276a072a26ba067a565802103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc8140c875a14edcbece61a6c18721398c927dc1e4509863e075b3922a8e3a2da6848e037142436e9102b529ee93a9ec618a4c67b63c52790d71812bb94179056913bba100af038001e3a10001ffffffff0420a1070000000000302ea22c8020541be9f843b476373fc18d8c8fab59c98c2c009f49c07fa66b7b431e4142feae8103120c008203000401cce028933b00000000302ea22c8020bc485b86ffd067abe520c078b74961f6b25e4efca6388c6bfd599ca3f53d8dae8103120c008203000401cc28b9486cb2430000232103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abcac0000000000000000246a22e374e4895451cae47f8f10303c3594888b739f044f7c778623318d877e8df365cc6600000000
```

Response:

```bash
ProcessAssets
AssetValidate (t)
vin1 1000000000, vout0 500000, vout1 999500000, transfer validated 10.00000000 -> 10.00000000
AssetValidate.(t) passed
88ac2d4d27654e9d8ac195d5ab482ee9895303902eaacfbb687b1e736bb06fb4
```

Step 3: Decode the raw transaction and check against the following if the data is sane

```bash
./komodo-cli -ac_name=HELLOWORLD decoderawtransaction 01000000023b61e44ce3cedf536b52d8da11faacd041494a078e971551ed4e2bd496bc8da1000000006a4730440220111c67172740c0c2556979fdf84639ba299ff22586ebd220f25aa301f029003f02203da97a2575c0ed1b309774309f5dc952ee305a46cd83e95eae99e3564a1772f6012103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abcffffffff66cc65f38d7e878d312386777c4f049f738b8894353c30108f7fe4ca515489e4000000007b4c79a276a072a26ba067a565802103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc8140c875a14edcbece61a6c18721398c927dc1e4509863e075b3922a8e3a2da6848e037142436e9102b529ee93a9ec618a4c67b63c52790d71812bb94179056913bba100af038001e3a10001ffffffff0420a1070000000000302ea22c8020541be9f843b476373fc18d8c8fab59c98c2c009f49c07fa66b7b431e4142feae8103120c008203000401cce028933b00000000302ea22c8020bc485b86ffd067abe520c078b74961f6b25e4efca6388c6bfd599ca3f53d8dae8103120c008203000401cc28b9486cb2430000232103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abcac0000000000000000246a22e374e4895451cae47f8f10303c3594888b739f044f7c778623318d877e8df365cc6600000000
```

Response:

```json
{
  "txid": "88ac2d4d27654e9d8ac195d5ab482ee9895303902eaacfbb687b1e736bb06fb4",
  "size": 524,
  "version": 1,
  "locktime": 0,
  "vin": [
    {
      "txid": "a18dbc96d42b4eed5115978e074a4941d0acfa11dad8526b53dfcee34ce4613b",
      "vout": 0,
      "scriptSig": {
        "asm": "30440220111c67172740c0c2556979fdf84639ba299ff22586ebd220f25aa301f029003f02203da97a2575c0ed1b309774309f5dc952ee305a46cd83e95eae99e3564a1772f601 03fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc",
        "hex": "4730440220111c67172740c0c2556979fdf84639ba299ff22586ebd220f25aa301f029003f02203da97a2575c0ed1b309774309f5dc952ee305a46cd83e95eae99e3564a1772f6012103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc"
      },
      "sequence": 4294967295
    },
    {
      "txid": "e4895451cae47f8f10303c3594888b739f044f7c778623318d877e8df365cc66",
      "vout": 0,
      "scriptSig": {
        "asm": "a276a072a26ba067a565802103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc8140c875a14edcbece61a6c18721398c927dc1e4509863e075b3922a8e3a2da6848e037142436e9102b529ee93a9ec618a4c67b63c52790d71812bb94179056913bba100af038001e3a10001",
        "hex": "4c79a276a072a26ba067a565802103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc8140c875a14edcbece61a6c18721398c927dc1e4509863e075b3922a8e3a2da6848e037142436e9102b529ee93a9ec618a4c67b63c52790d71812bb94179056913bba100af038001e3a10001"
      },
      "sequence": 4294967295
    }
  ],
  "vout": [
    {
      "value": 0.005,
      "valueSat": 500000,
      "n": 0,
      "scriptPubKey": {
        "asm": "a22c8020541be9f843b476373fc18d8c8fab59c98c2c009f49c07fa66b7b431e4142feae8103120c008203000401 OP_CHECKCRYPTOCONDITION",
        "hex": "2ea22c8020541be9f843b476373fc18d8c8fab59c98c2c009f49c07fa66b7b431e4142feae8103120c008203000401cc",
        "reqSigs": 1,
        "type": "cryptocondition",
        "addresses": ["RLB1YWh4N115NFh8tbArCBGaTQ3F43Yg1F"]
      }
    },
    {
      "value": 9.995,
      "valueSat": 999500000,
      "n": 1,
      "scriptPubKey": {
        "asm": "a22c8020bc485b86ffd067abe520c078b74961f6b25e4efca6388c6bfd599ca3f53d8dae8103120c008203000401 OP_CHECKCRYPTOCONDITION",
        "hex": "2ea22c8020bc485b86ffd067abe520c078b74961f6b25e4efca6388c6bfd599ca3f53d8dae8103120c008203000401cc",
        "reqSigs": 1,
        "type": "cryptocondition",
        "addresses": ["RRPpWbVdxcxmhx4xnWnVZFDfGc9p1177ti"]
      }
    },
    {
      "value": 744335.99945,
      "valueSat": 74433599945000,
      "n": 2,
      "scriptPubKey": {
        "asm": "03fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abc OP_CHECKSIG",
        "hex": "2103fe754763c176e1339a3f62ee6b9484720e17ee4646b65a119e9f6370c7004abcac",
        "reqSigs": 1,
        "type": "pubkey",
        "addresses": ["RANyPgfZZLhSjQB9jrzztSw66zMMYDZuxQ"]
      }
    },
    {
      "value": 0.0,
      "valueSat": 0,
      "n": 3,
      "scriptPubKey": {
        "asm": "OP_RETURN e374e4895451cae47f8f10303c3594888b739f044f7c778623318d877e8df365cc66",
        "hex": "6a22e374e4895451cae47f8f10303c3594888b739f044f7c778623318d877e8df365cc66",
        "type": "nulldata"
      }
    }
  ]
}
```
