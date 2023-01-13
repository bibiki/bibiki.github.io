---
layout: post
title:  "Demystifying Mining"
---

Demystifying Mining
===================

You have heard that Bitcoin mining "solves complex mathematical problems" but that is an inaccurate description of what happens.
Mining really is no different than counting. So, I want to show that mining is really a very simple process and remove the intimidation that "solves complex mathematical problems" may induce.

In the course of this article I will introduce fragments of code. If you feel inclined to copy the code please know that the end of the article contains the code unfragmented, in full, and tested.

Let us start by playing a game: I give you a number and in return, you give me two numbers. Let us name the number I give you "data" and the two numbers that you give me "nonce" and "hash". Also, there is a property that binds these three numbers together; I am arbitrarily chosing the property to be this:

The sum of data and nonce is congruent 0 modulo 17.

So, If I give you 2, you can respond with 15 and 17. 32 and 34 will work also as 2 + 32 = 34, and 34 is congruent 0 modulo 17. So, for data = 2, your nonce is 32 and your hash is 34.

Now, for some given data, say data = 2, a very naive way to find a nonce is to start checking if nonce = 0 meets the requirements. If not, increase nonce by 1 and check that. Continue this process until you find a nonce such that data + nonce is congruent to 0 modulo 17.

Let me write this in python:

```
def mine(data):
    nonce = 0
    combine = data + nonce
    while not combine % 17 == 0:
        nonce += 1
        combine = data + nonce
    return [nonce, combine]
```

The way this function is written, it will return the smallest nonnegative nonce that meets the criteria. So, for the example mentioned above, that means that nonce = 15, and combine = nonce + data = 17 will be returned.

So, for the purposes of my game here, finding an acceptable nonce is no different than counting from 0 upwards.

Let me improve here my wordchoice so that it better resembles what is usually used in bitcoin mining. Specifically, let us call the transformation combine = data + nonce "hashing".

So, the hash of data and nonce is data + nonce. Let me write a function for that:

```
def summation_hash(data, nonce):
    return data + nonce
```

Now, my mine function may look like this:

```
def mine(data):
    nonce = 0
    hashed = summation_hash(data, nonce) #this line changed. it initally was 'combine = data + nonce'
    while not hashed % 17 == 0: #hashed is the sum of data and nonce
        nonce += 1
        hashed = summation_hash(data, nonce)
    return [nonce, hashed]
```

So, I just introduced some abstraction of some code, but my mine function continues to count in search of an acceptable nonce. As the next step to show how mining for Bitcoin is no different than counting, I will continue improving abstraction of this code and then show that mining Bitcoin is an instance of the same abstraction.

Specifically, the line hashed % 17 == 0 is a test of whether a given nonce results in an acceptable hash. So, let us think of it as an acceptability test, and let us call the particular function that I am using in my game is_congruent17. Here is the code for that function:

```
def is_congruent17(hashed):
    return hashed % 17 == 0
```

This allows me to rewrite the mine function as follows:

```
def mine(data):
    nonce = 0
    hashed = summation_hash(data, nonce)
    while not is_congruent17(hashed): #this line changed. it initially was 'while not hashed % 17 == 0:'
        nonce += 1
        hashed = summation_hash(data, nonce)
    return [nonce, hashed]
```

Although it does not add much to the argument that counting and mining Bitcoin conform to a common abstraction, I will abstract the generation of the next nonce as a function.

So, nonce += 1 can be replaced by the call to some function. Namely:

```
def next_positive(n):
    return n + 1
```

And now, the mine function looks like this:

```
def mine(data):
    nonce = 0
    hashed = summation_hash(data, nonce)
    while not is_congruent17(hashed):
        nonce = next_positive(nonce) #this line changed. it initially was 'nonce += 1'
        hashed = summation_hash(data, nonce)
    return [nonce, hashed]
```

Now that I abstracted away the hashing and the generation of the next nonce to try, my mine function can be freed from having to know what functions to use for those two steps. Instead, we can tell the mine function what functions to use for those steps via arguments to it. This is my last step of abstraction before I move to the last part of showing how mining Bitcoin is really counting.

So, here is the last version of my mine function:

```
def mine(data, next_nonce_generator, hasher, acceptability_tester):
    #I introduced arguments to this function so that I can feed it
    #functions it needs, instead of it havaing to know what functions to use
    nonce = 0
    hashed = hasher(data, nonce)
    while not acceptability_test(hashed):
        nonce = next_nonce_generator(nonce)
        hashed = hasher(data, nonce)
    return [nonce, hashed]
```

And that is it. Up to now, I started with a function whose execution was no different then counting from zero and testing a hash for some desirable property, and then I seperated the steps of counting and testing for acceptability from the function that actually counts and the one that actually does the acceptability test.
To see how the mine function would be called for the game I specified in the beginning:

```
mine(data, next_positive, summation_hash, is_congruent17)
```

Now, let me go back to Bitcoin mining. It starts with some data and the miner needs to find some nonce such that the hash256 of the hash256 of data and nonce has a certain number of trailing zeroes. Right, hash of hash.

Assuming that I have a bitcoin specific hasher function (bitcoin_hasher), and a bitcoin specific function to generate the next nonce (bitcoin_next_nonce), and a bitcoin specific acceptability tester function (2_trailing_zeroes), I can simply call the mine function as follows and start mining for Bitcoin:

```
mine(transaction_data, bitcoin_next_nonce, bitcoin_hasher, 2_trailing_zeroes)
```

Now, of course, since I am using a specific 2_trailing_zeroes function here, this does not allow for automatic difficulty adjustment, but that is easily replaced by some other function that does take into account the current mining difficulty of Bitcoin.

Small Digression
================

Earlier, when I abstracted away the counting, or rather the guessing of the next nonce to try, I said that that step was not going to add much to my argument that mining Bitcoin is the same as counting. However, it does help illustrate a point that helps the understanding of Making Sure this Actually Works section.
Here goes. Using my next_positive(n) function in playing the simplistic game I started with will lead to mine function trying nonces ranging from 0 to 15 for data = 2 until it finds 17 as the hash. However, from what is known from addition modulo some number, we can avoid having to guess; we can instead deduce in a finite number of steps the nonce that will let the acceptability test pass.
In particular, since my game has specified that we need a nonce such that data + nonce is congruent to 0 modulo 17, instead of passing in to my mine function next_positive, I can simply pass in to it a function whose result, regardless of what input it recieves, is 17 - (data % 17).
In the particular case that data = 2, data % 17 is again 2, and 17 - 2 = 15.

In code, it would look something like:

```
remainder = data % 17
nonce = 17 - remainder
nonce_generator = lambda x: nonce
mine(data, nonce_generator, summation_hash, is_congruent17)
```

So, since my game depends on some hash function that allows for a way to find a suitable nonce starting with an easily identifiable hash beforehand, I can take advantage of that and mine a block in my game in a finite number of steps. That is a huge improvement in a particular miner's efficiency in finding a block.
However, Bitcoin depends on using a function that does not afford such kind of manipulations. **Creating such functions, identifying functions suitable for use in Bitcoin, is indeed a nontrivial matter.** But, using them the way Bitcoin uses them is not complex at all. It is, perhaps, akin to watching television. Watching television is in no way complex, but building one definitely is a nontrivial matter for most of us.


Making Sure this Actually Works
===============================

To make sure that this is an accurate description of what mining is, I will take the mine function that I used for **counting** and I will use it to replicate the finding of the genesis block's hash.

I will provide a hasher function, a next_nonce_generator function, and an acceptability_test function.

For the purposes of this article, I will use this implementation of the hasher function (code mostly borowed from https://en.bitcoin.it/wiki/Block_hashing_algorithm):

```
def bitcoin_hasher(data, nonce):
    nonce = str(nonce)
    if len(nonce) % 2 == 1:
        nonce = "0" + nonce
    full_header = data + nonce
    header_bin = unhexlify(full_header)
    hash = hashlib.sha256(hashlib.sha256(header_bin).digest()).digest()
    hash = hexlify(hash).decode("utf-8")
    return hash
```

My next_nonce_generator will be this:

```
def bitcon_nonce_generator(x):
    #gen will be a generator whose scope will be global
    #and will play the role of a source of nonces
    return next(gen)

```
In Bitcoin, nonce is rather seeked by counting. But to keep things simpler and, again, to remove the idea that bitcoin mining solves complex mathematical problems, and to show that mining really is as simple as counting, I will use that dummy nonce generator.

And, finally, my hash acceptability tester will be this:

```
def genesis_block_difficulty_tester(hash):
    return str(hash).endswith("0000000000")
```

My acceptability tester is also inaccurate. I think that the actual tester in Bitcoin rather makes sure that the hash is lower than the largest hexadecimal number whose 64-digit hexadecimal representation starts with a particular number of zeroes, depending on the mining difficulty at the time the hash is being seeked for.

With the functions specified above, the following line of code should return the actual genesis block's hash:

```
mine(data, bitcoin_nonce_generator, bitcoin_hasher, genesis_block_difficulty_tester)
```

Here is the complete code that you can use to verify that the last call of mine does what it is intended to do.
Once again, the same mine function that I used to count in me game is counting in Bitcoin's mining for blocks.

```
import hashlib
from binascii import unhexlify, hexlify

#the game part of the code starts here
def summation_hash(data, nonce):
    return data + nonce

def is_congruent17(hashed):
    return hashed % 17 == 0

def next_positive(n):
    return n + 1

def mine(data, next_nonce_generator, hasher, acceptability_tester):
    nonce = 0
    hashed = hasher(data, nonce)
    while not acceptability_tester(hashed):
        nonce = next_nonce_generator(nonce)
        hashed = hasher(data, nonce)
    return [nonce, hashed]

[game_nonce, game_hash] = mine(2, next_positive, summation_hash, is_congruent17)
#the game part of the code ends here

#set up for data and nonce source simulation for genesis block
version = "01000000"
previous_blocks_hash = "0000000000000000000000000000000000000000000000000000000000000000"
merkleroot = "3ba3edfd7a7b12b27ac72c3e67768f617fc81bc3888a51323a9fb8aa4b1e5e4a"
time = "ffff001d"
bits = "29ab5f49"

data = version + previous_blocks_hash + merkleroot + bits + time

def nonces():
    #just a source of nonces, to avoid having to count and map ints onto hexes, or counting in hexes
    yield "2dac2b7c"
    yield "3dac2b7c"
    yield "4dac2b7c"
    yield "1dac2b7c" #the last one is the nonce that results on the genesis block's hash for the data it consists of

#the implementation of bitcoin_nonce_generator will retrieve nonces from this generator
gen = nonces()

def bitcoin_hasher(data, nonce):
    nonce = str(nonce)
    if len(nonce) % 2 == 1:
        nonce = "0" + nonce
    full_header = data + nonce
    header_bin = unhexlify(full_header)
    hash = hashlib.sha256(hashlib.sha256(header_bin).digest()).digest()
    hash = hexlify(hash).decode("utf-8")
    return hash

def bitcoin_nonce_generator(x):
    return next(gen)

def genesis_block_difficulty_tester(hash):
    return str(hash).endswith("0000000000")

[nonce, hash] = mine(data, bitcoin_nonce_generator, bitcoin_hasher, genesis_block_difficulty_tester)
print("nonce", nonce)
print("hash", hash)
```
