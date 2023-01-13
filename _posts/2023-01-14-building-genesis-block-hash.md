---
layout: post
title:  "Building Bitcoin Genesis Block"
---

While I was writing my earlier post that aims to demystify the process of mining in Bitcoin I looked for how to generate the Bitcoin genesis block's hash. It was not immediately clear so I will write the process in detail here in case someone else decides they'd like to do it also.

If you have access to a Bitcoin node that you can poll with bitcoin-cli, you may run the following code:

```
bitcoin-cli getblockhash 0
```

This will give you the hash of the first block in Bitcoin, otherwise known as the genesis block. That hash is 000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f

Now that you have the hash, you may run the following command line:

```
bitcoin-cli getblock 000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f
```

Here is that command's response:

```
{
  "hash": "000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f",
  "confirmations": 771689,
  "height": 0,
  "version": 1,
  "versionHex": "00000001",
  "merkleroot": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
  "time": 1231006505,
  "mediantime": 1231006505,
  "nonce": 2083236893,
  "bits": "1d00ffff",
  "difficulty": 1,
  "chainwork": "0000000000000000000000000000000000000000000000000000000100010001",
  "nTx": 1,
  "nextblockhash": "00000000839a8e6886ab5951d76f411475428afc90947ee320161bbf18eb6048",
  "strippedsize": 285,
  "size": 285,
  "weight": 1140,
  "tx": [
    "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"
  ]
}
```

There is a lot of information there but I will focus on how the hash has been generated, or rather, what the data and the code that generates it look like.

Here is an example of generating the hash of a real block in Bitcoin:

```
import hashlib
from binascii import unhexlify, hexlify
header_hex = ("01000000" +
 "81cd02ab7e569e8bcd9317e2fe99f2de44d49ab2b8851ba4a308000000000000" +
 "e320b6c2fffc8d750423db8b1eb942ae710e951ed797f7affc8892b0f1fc122b" +
 "c7f5d74d" +
 "f2b9441a" +
 "42a14695")

header_bin = unhexlify(header_hex)
hash = hashlib.sha256(hashlib.sha256(header_bin).digest()).digest()
hexlify(hash).decode("utf-8")
```

I took this code from [here](https://en.bitcoin.it/wiki/Block_hashing_algorithm).

Now, before I proceed detailing further steps let me point out that 81cd02ab7e569e8bcd9317e2fe99f2de44d49ab2b8851ba4a308000000000000 is the hash of the block that preceeds the block whose hash is being calculated. In other words, we should be able to use bitcoin-cli to find a block whose hash is that. Let me try:

```
bitcoin-cli getblock 81cd02ab7e569e8bcd9317e2fe99f2de44d49ab2b8851ba4a308000000000000
```

If you tried that command, you must have gotten something like:

```
error code: -5
error message:
Block not found
```

So, although I said that that hash is the hash of a real block in Bitcoin, the command still responds with an error?!?! So what gives? Well this is little endian representation of the hash. So, the particular Python code that I borrowed from https://en.bitcoin.it/wiki/Block_hashing_algorithm expects its input in little endian representation whereas bitcoin-cli expects it in big endian representation.
Here are both representations of the same hash:

```
81cd02ab7e569e8bcd9317e2fe99f2de44d49ab2b8851ba4a308000000000000
00000000000008a3a41b85b8b29ad444def299fee21793cd8b9e567eab02cd81
```

Look at the first two numbers in the first representation and in the last two numbers in the second representation. \
Look at the third and fourth characters in the first representation (cd) and in the fourth and third from the right in the second representation (cd).
That gives you a visual clue of how the two endian representations differ.

Now that that is clear, let me point out that header_hex in the Python code above is the concatenation of hexadecimal values in little endian representation of the following block specific data:

```
version
previous_blocks_hash
hash_merkle_root
bits
time
nonce 
```

Above, I retrieved the genesis block's information. Here is the same information as well as the command that retrieves it:

```
bitcoin-cli getblock 000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f

{
  "hash": "000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f",
  "confirmations": 771693,
  "height": 0,
  "version": 1,
  "versionHex": "00000001",
  "merkleroot": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
  "time": 1231006505,
  "mediantime": 1231006505,
  "nonce": 2083236893,
  "bits": "1d00ffff",
  "difficulty": 1,
  "chainwork": "0000000000000000000000000000000000000000000000000000000100010001",
  "nTx": 1,
  "nextblockhash": "00000000839a8e6886ab5951d76f411475428afc90947ee320161bbf18eb6048",
  "strippedsize": 285,
  "size": 285,
  "weight": 1140,
  "tx": [
    "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"
  ]
}

```

So, for genesis block, we have:

```
version = 1
previous_blocks_hash = 0
hash_merkle_root = 4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b
bits = 1d00ffff
time = 1231006505
nonce = 2083236893
```

Other than nonce and time, the data is in hexadecimal base. Let us fix that for nonce and time.

```
nonce = 7c2bac1d
time = 495fab29
```

Note: you can use google to map an integer to its hexadecimal value.

A couple more fixes: previous_blocks_hash is supposed to be a 32 byte hash. As a result, it is supposed to look like a 64-character string in the usual character encoding. So,

```
previous_blocks_hash = "0000000000000000000000000000000000000000000000000000000000000000"
```

Also, version is specified to be 4 bytes long. As a result, it is supposed to look like this:

```
version = "00000001"
```

So, here is our data in hexadecimal, big endian representation.

```
version = "00000001"
previous_blocks_hash = "0000000000000000000000000000000000000000000000000000000000000000"
hash_merkle_root = 4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b
bits = 1d00ffff
time = 495fab29
nonce = 7c2bac1d
```

The final step before we can build our header_hex = version + previous_blocks_hash + hash_merkle_root + bits + time + nonce is to represent these hexadecimal values in little endian form.

I pointed out the visual difference between the two representations but the real difference between the two representations is on what side of the number the highest power of the base is. As an example AB = Ax16 + B in one representation, and A + Bx16 in the other. Anyways, once the endiandness has been changed, the data looks like this:

```
version = "01000000"
previous_blocks_hash = "0000000000000000000000000000000000000000000000000000000000000000"
merkleroot = "3ba3edfd7a7b12b27ac72c3e67768f617fc81bc3888a51323a9fb8aa4b1e5e4a"
bits = "29ab5f49"
time = "ffff001d"
nonce = "1dac2b7c"
```

and, header_hex looks like this:

```
0100000000000000000000000000000000000000000000000000000000000000000000003ba3edfd7a7b12b27ac72c3e67768f617fc81bc3888a51323a9fb8aa4b1e5e4a29ab5f49ffff001d1dac2b7c
```

If you run the code above with the data from genesis block, you should get 6fe28c0ab6f1b372c1a6a246ae63f74f931e8365e15a089c68d6190000000000. That is the genesis block's hash in little endian representation.

Hopefully that clarifies how to replicate the calculation of the genesis block's hash.
