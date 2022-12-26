---
layout: post
title:  "Envelope Encryption - Putting Your Encryption Key in an Envelope Is the Safer Option"
kicker: "Data Governance"
subtitle: "Envelope encryption is a way of encrypting plaintext data using a key and then encrypting that key using an another key. This strategy is intended not just to make things more secure but also to enhance performance."
image: assets/images/posts-cover-images/envelope-encryption.jpg
author: senthil
date: 2022-07-22 18:00:40 +0530
tags: ["envelope-encryption", "data-protection", "root-key", "data-key"]
categories: data-security-and-compliance
featured: false
hidden: false
---

Before we get into *envelop encryption*, let's go over some of the fundamentals of encryption/decryption.

#### Key material

A key material is a random sequence of bits that is used in a cryptographic algorithm to convert plaintext to ciphertext (encrypted text) and vice versa. To prevent the ciphertext from being decoded back to plaintext, this key material must be kept secret. Note that *public key cryptography*, also known as *asymmetric cryptography*, employs *both* public and private key materials, with the public key material intended to be shared.

#### Key

A *key*, which is just a short name for an *encryption key*, has a *key-id* or *alias*, *key material*, and *other information about it*, such as who created it and so on.

```plain
Plaintext + Key = Ciphertext
Ciphertext - Key = Plaintext

Note: + and - signs indicate encryption and decryption actions, respectively.
```

The image below shows what a key is composed of in general: 

|![Figure 1: Key with key material](/assets/images/posts/key-material.png "Created by Author")|
|:-:|
|<sup>*Figure 1: A key with key id, key material and other metadata.*</sup>|<br/><br/>

```plan
key
 |
 |--- Key ID: 12t9c
 |--- Key Material: 010101110101100
 |--- Other Metadata: [Created By: User1, Created On: 08-01-2022, Modified By: User1, Modified On: 08-01-2022,...]
```

# What is envelope encryption?

When we encrypt our data using an *encryption key*, it is protected, but we also need to protect that encryption key. Envelope encryption is the method of first encrypting plaintext data using a key known as a *data key* and then encrypting that data key with a different key known as a *root key* or *master key*. 

When compared to a real-life use case, the envelope encryption approach is the action of locking the home with a key and keeping the key somewhere safe. 

It's a two-step procedure:

1. Encrypt data with a *data key* (aka *encryption key*).
2. Further encrypt the *data key* with another key called the *root key* or *master key*.

Having said that, the envelop encryption scheme generates two keys. The only key that is encrypted in this envelop encryption scheme is the data key; the root key is not. The root key has to be kept in plaintext so that it can be used to decrypt the data key.

> This two-step procedure can be made longer by encrypting with another encryption key and then encrypting the resulting encrypted key with yet another encryption key, and so on, as shown below.<br><br>*Root key --> Encryption-keyN --> ... Encryption-key1 --> Data key --> Data*<br><br>But in the end, though, the top-level key must stay in plaintext so that we can decrypt the rest of the keys and our data. This top-level plaintext key is known as the *root key*.

|![Envelope encryption high-level flow.](/assets/images/posts/envelope-encryption.png "Created by Author")|
|:-:|
|<sup>*Figure 2: Envelope encryption high-level flow.*</sup>|<br/><br/>

## How can root keys generated in plaintext be protected?

Thankfully, the key vault saved the day! The key vault safeguards our root keys by securely storing and managing them using specialised cryptographic hardware with the highest level of security. In the case of AWS, the root keys are stored securely in the AWS KMS.

## Where to keep the data keys?

We have just learnt that root keys are stored securely inside a key vault, but where are data keys stored securely? Can't the data keys be safely kept in the same vault as the root keys? We certainly can, but why is it necessary? Remember that the data keys are inherently protected by encryption. So we should not be concerned about where the data keys are kept. We can put them anywhere, but it's best to put them alongside the encrypted data.

|![Envelope encryption flow.](/assets/images/posts/envelope-encryption-flow.png "Created by Author")|
|:-:|
|<sup>*Figure 3: Envelope encryption flow.*</sup>|<br/><br/>

## Benefits of envelop encryption

This strategy isn't meant to make things more secure; instead, it's meant to improve performance. Public-key algorithms are often sluggish and use asymmetric algorithms. In contrast, symmetric algorithms are very fast. So, the data, considerably very large in size, is quickly encrypted with a symmetric algorithm using a random key. The random key is subsequently encrypted using a public-key scheme. This approach *combines the benefits of public-key scheme with the efficiency of symmetric encryption*.

Envelope encryption reduces the network load since only the request and delivery of the considerably smaller data key go over the network. The data key is used locally in our application, so we don't have to send the whole block of data to encrypt or decrypt it, which would cause network latency.

## Key rotation

Key rotation is the process of retiring an encryption key and replacing it with a new cryptographic key. Changing the keys on a regular basis helps us meet industry standards and best practices for cryptography. A good security practice is to rotate keys on a regular and automated basis. Key rotation is required by several industrial requirements.

It is important to note that key rotation changes *only* the key material, which is used in encryption or decryption operations. Regardless of how many times the key material changes, the *key id remains unchanged*. So every time we rotate the key, a new key material is created.

|![Key rotation](/assets/images/posts/key-rotation.png "Created by Author")|
|:-:|
|<sup>*Figure 4: Key rotation.*</sup>|<br/><br/>

In general, key-vault tools *safely keep all old versions of the key material forever*, so we can decrypt any data that was encrypted with that key and do not delete any rotated old key materials until we delete the keys. When we use a rotated key to encrypt data, the key-vault uses the current key material. When we use the rotated key to decrypt ciphertext, key-vault uses the key material version that was used to encrypt it.

### Automatic key rotation

It's recommended to rotate keys automatically on a regular schedule. The frequency of rotation is defined by a rotation schedule. 

The rotation schedule may be determined by: 

- Age of the key
- Number or volume of data encrypted with a certain key version

We may enable automatic key rotation for an existing key depending on the key-vault tool we use. When we set up automatic key rotation for the keys, key-vault tools usually make new key materials for those keys on a regular basis, like once a year.

### How often should the keys be rotated? 

Key rotations should ideally occur every few months, but once every three months is recommended. If we want to perform this more often, we:

- Have a large amount of data.
- Have a high-value data.
- Have a shared-environment.

### Benefits of key rotation

- Key rotation reduces the amount of data encrypted with the same key version, making assaults less likely.
- Regular key rotation ensures that our system is capable of handling manual key rotation, whether due to a security breach or for any other reason.