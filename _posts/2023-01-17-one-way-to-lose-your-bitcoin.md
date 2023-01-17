---
layout: post
title:  "One Way to Lose Your Bitcoin"
---

<span style="color:red">This is an illustration of what NOT to do.</span>

Last few days Bitcoin Twitter has been abuzz by Luke Dashjr's loss of his Bitcoin. It is in no way clear how this happened, if this happened at all.

Luke is one of Bitcoin developers and is kept in high regard by the community. I intend in no way to cast doubt in what he says but when he first said that his Bitcoin has been stolen people joked that this might be Luke's personal variation on 'I had a boating accident and lost my stash,' which is a common joke in the community and is used to signal that one has no Bitcoin. That might as well be the case but then he must be who moved his coins to an address that he controls. Since he said that that address is where the thief has sent Luke's Bitcoin, then he is effectively setting FBI on himself when he is asking them for help about this loss. Makes no sense. I believe Luke's Bitcoin has really ben stolen. But if I am wrong, then Luke knows how to stop any surveilence of the funds in the later address?!?! I highly doubt this. If there is a third possibility that I am not seeing, I'd love to be made aware of it.

With that made clear, the next natural and very interesting question is 'how?' Of all people, how does this happen to someone as Bitcoin savy as Luke? Luke says he does not know and he has consistently responded with 'wrong' to any speculation by others as to how it happened. In particular, he says that multiple of his 'wallets' have been compromised and that both cold and hot wallets in his control have been stolen from. My understanding is that somehow the individual or group that stole Luke's funds managed to do so from more than one private key. That is, from at least one private key not associated with a hot wallet. This is very scary.

Just earlier tonight, Samson Mow tweetted this 'If you can access your coins in under 2 weeks, it’s not cold storage. #Bitcoin' and this is the point I will be making in this post.

Some people say Luke's case is an inside job. I am more prone to believe that some vulnerability has been exploited. It is natural for somebody that has some Bitcoin to want to be able to recover their funds without immediate access to the hardware that contains their private keys. But, if some strategy X helps you to recover quickly that which you don't want to remember, it'll help others to recover quickly that which they never knew. And, to illustrate my point, I will here devise a naive strategy to generate a private key and instead of remembering my private key I will rely on my understanding of the strategy to regenerate the keys at some other point in time, probably using some hardware I have never used before that.

Here is my naive strategy
=========================
Instead of remembering my seed phrase, I will organize the 2048 possible words in a 32x64 table and I will draw a noninterruptible line on it connecting 12 of the cells. That is a perfectly valid private key, or rather seed phrase that corresponds to a key.

Now, instead of having to remember 12 words in a sequence, we simply rely on our ability to remember a line on a table. To make matters worse, my strategy will be to exclude lines shorter than 12 steps. What that means is that no two consequtive words in my seed phrase are the same. Effectively, I am shrinking the number of possible keys my strategy generates.

But, the lower that number is, the fewer guesses an attacker has to make to guess right. At this point, one might think 'but how is an attacker going to know my strategy for drawing the lines.' The simple answer is 'they don't have to.' The vulnerability comes from the fact that humans think alike. What helps one to remember their line of choice is that the line fits some pattern and patterns are the same for other people. And if another person tries to come up with a strategy to easily remember their seed phrase, they are extremely likely to come up with the same strategy I am laying out here. The strategy, not just the line.

Although this particular strategy's weakness, as described in words, is not too huge, the truth of the matter is that any patterned strategy is vulnerable. And the assumption that there is some pattern that nobody will think of is naive. I'd not be surprised to learn that somebody's funds have been or will be stolen as a result of vulnerabilities akin to the one I am describing here.

The Counting Argument
=====================

With a list of 2048 words, one can generate 2048^12 different keys. With the strategy I described above, the number of possible keys is 2048*2047^11. The quotient between these two is 1.005386862727986. This number indicates that the number of keys the strategy generates is pretty close to the number of keys that may be generated without this strategy. However, the strategy only excludes keys whose seed phrase includes the same word in two consequtive positions. Soon as there is some more effort invested in understanding the psychology of writing lines connecting 12 cells on a 32x64 grid, the number of keys to try will rapidly diminish.

Just for the sake of argument, let us assume the strategy generates only the keys whose seed phrase consists of 12 unique words. The number of such keys is 2048*2047*...*2037. Compared to the difficulty of guessing correctly a fully randomly generated key, this strategy's difficulty is more than 3% lower.

The Message Here
======================

The crux of the message I am trying to convey is that there is no pattern that can replace randomness. This is not a problem with Bitcoin. This is rather a problem with how to safely remember private keys. For patterned ways to generate keys, the vulnerability is with the human nature, the natural fact that one person's brain will most of the time devise most of the same strategies as another's.

If you have lots of Bitcoin, I do not know how you should keep it safe. If you have little, your problem is small. In either case, people are working on various solutions to that problem. Although highly discouraged, some are exploring custodies. That is what you are using when you keep Bitcoin in an exchange. Others are exploring hardware wallets. And some, which probably is Luke's case, are devising their own strategies and devices. I know I would have probably written my own seed phrase-to-private key application if I had Luke's stash, and this is another reason not to ask him what setup he used.
