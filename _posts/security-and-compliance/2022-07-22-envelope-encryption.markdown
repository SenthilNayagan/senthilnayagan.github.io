---
layout: post
title:  "Envelope Encryption"
subtitle: "Envelope encryption is a way of encrypting plaintext data using a key and then encrypting that key using an another key. This strategy isn't meant to make things more secure; instead, it's meant to improve performance."
image: assets/images/posts-cover-images/envelope_encryption.jpg
author: senthil
date:   2022-07-22 18:00:40 +0530
tags: ["envelope-encryption", "data-protection", "root-key", "data-key"]
categories: security-and-compliance envelope-encryption
permalink: /:categories/:title
featured: true
hidden: false
---
# What is envelope encryption?
When we encrypt our data using an encryption key, it is protected, but we also need to protect that encryption key. Envelope encryption is the method of first encrypting plaintext data using a data key and then encrypting that data key with a different key.

It's a two-step procedure:

1. Encrypt data with a *data key* (aka *encryption key*).
2. Further encrypt the *data key* with another key called the *root key*.

Having said that, the envelop encryption scheme generates two keys. The only key that is encrypted in this envelop encryption scheme is the data key; the root key is not. The root key has to be kept in plaintext so that it can be used to decrypt the data key.

> This two-step procedure can be made longer by encrypting with another encryption key and then encrypting the resulting encrypted key with yet another encryption key, and so on, as shown below.<br><br>> *Root key --> Encryption-keyN --> ... Encryption-key1 --> Data key --> Data*<br><br>But in the end, though, the top-level key must stay in plaintext so that we can decrypt the rest of the keys and our data. This top-level plaintext key is known as the *root key*.

|![Envelope encryption flow.](/assets/images/posts/envelope-encryption.png)|
|:-:|
|<sub><sup>*Envelope encryption flow.*</sup></sub>|

# How can root keys generated in plaintext be protected?
Thankfully, the key vault saved the day! The key vault safeguards our root keys by securely storing and managing them using specialised cryptographic hardware with the highest level of security. In the case of AWS, the root keys are stored securely in the AWS KMS.

# Where to keep the data keys?
We have just learnt that root keys are stored securely inside a key vault, but where are data keys stored securely? Can't the data keys be safely kept in the same vault as the root keys? We certainly can, but why is it necessary? Remember that the data keys are inherently protected by encryption. So we should not be concerned about where the data keys are kept. We can put them anywhere, but it's best to put them alongside the encrypted data.

# Motivation
This strategy isn't meant to make things more secure; instead, it's meant to improve performance. Public-key algorithms are often sluggish and use asymmetric algorithms. In contrast, symmetric algorithms are very fast. So, the data, considerably very large in size, is quickly encrypted with a symmetric algorithm using a random key. The random key is subsequently encrypted using a public-key scheme. This approach combines the benefits of public-key scheme with the efficiency of symmetric encryption.