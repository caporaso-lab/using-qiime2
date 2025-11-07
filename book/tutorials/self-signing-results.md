# Self-Signing QIIME 2 Results

Introduced in the 2025.10 release, QIIME 2 {term}`Artifacts <artifact>` and {term}`Visualizations <visualization>` (i.e. {term}`Results <result>`) can now contain a cryptographic signature, thus providing a method for verifying the identity of who created the {term}`result`.
This is achieved using a new {term}`Annotation <annotation>` sub-type: a {term}`Signature <signature>`.

In this tutorial, we will walk through the pre-requisites for {term}`signature` creation, how to add a {term}`signature` to an existing {term}`result`, and how to verify a {term}`result` with an attached {term}`signature`.

## Pre-requisites for creating a Signature

The tool we are leveraging for {term}`signature` creation is [GnuPG](https://www.gnupg.org/documentation/index.html) - more specifically, its cryptographic signature creation and verification.
GnuPG (or GPG), also known as GNU Privacy Guard, is a free and open-source software that provides cryptographic privacy and authentication, allowing users to encrypt and sign data.
This is achieved by generating a GPG key pair (consisting of a public key and private key), which is used to cryptographically sign a particular file (using the key pair to associate an identity with the given file).
Once a file is signed, it can be shared along with the sender's public key (added to the recipient's GPG keyring) to verify the sender's identity.

Before we can create a {term}`signature` and attach it to a QIIME 2 {term}`result`, we need to create a GPG keypair.
Most modern computers come with GPG already installed; you can confirm whether or not you have GPG on your machine by running the following command in your terminal:

```shell
gpg --version
```

If GPG is installed, you should see something like the following:

```shell
gpg (GnuPG) 2.2.4
libgcrypt 1.8.1
Copyright (C) 2017 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Home: /home/ubuntu/.gnupg
Supported algorithms:
Pubkey: RSA, ELG, DSA, ECDH, ECDSA, EDDSA
Cipher: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256, TWOFISH,
        CAMELLIA128, CAMELLIA192, CAMELLIA256
Hash: SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
Compression: Uncompressed, ZIP, ZLIB, BZIP2
```

If GPG is not installed on your machine, you'll receive a `command not found` error; if this is the case, you will need to install GPG before you can proceed.
You can find instructions on how to natively install GPG on their website, linked [here](https://www.gnupg.org/download/).

```{note}
We have intentionally chosen not to include GPG in our QIIME 2 environment files.
While installing and using GPG directly on your own machine is safe and recommended, distributing GPG through our Conda environments would require us (as software maintainers) to rely on additional third-party wrappers and distribution channels.
These added layers of abstraction could expose the QIIME 2 {term}`Distribution` process to potential supply-chain or man-in-the-middle attacks.
To minimize that risk, we ask users to install GPG directly from trusted system package sources.
```

Once GPG is installed on your machine, you will need to create a new keypair for use in {term}`signature` creation.
If you already have a keypair you would like to use for this purpose, you can disregard the instructions below and skip to the {term}`signature` creation section.

You can create a new GPG keypair using the following command:

```shell
gpg --full-generate-key
```

You will then be prompted to make selections on the type of keypair to create.
Since the selections provided in GPG are subject to change over time, we have chosen not to go through them in this tutorial but we encourage you to review GPG's keypair creation documentation, which can be found [here](https://www.gnupg.org/gph/en/manual.html#AEN26).

Once you have created your keypair, you will need to copy the keypair's fingerprint for use in {term}`signature` creation.
You can obtain the fingerprint of your keypair by using the following command:

```shell
gpg --fingerprint
```

The output of this command should look something like this:

```shell
pub   ed25519 2025-09-24 [SC] [expires: 2027-09-24]
      7B2B 042B D7F2 7AE4 DF86  18D0 80D7 286B 1D44 DB32
uid           [ultimate] Example User <example@mail.me>
sub   cv25519 2025-09-24 [E] [expires: 2027-09-24]
```

This command shows all of the keypairs on your GPG keyring.
If you have multiple keypairs present, you will see multiple entries that look something like the above example.

In each keypair entry, you will see a line that looks something like this:

```shell
7B2B 042B D7F2 7AE4 DF86  18D0 80D7 286B 1D44 DB32
```

This is the line that contains the keypair fingerprint.
This is what you will need to copy and provide as input when creating a new {term}`signature`.

## Creating a Signature

Now that you have your keypair fingerprint, you can create a new {term}`signature` to self-sign a {term}`result`.
Note that you will need a {term}`result` that was created from QIIME 2 version 2025.4 (or newer).

Below is the workflow for {term}`signature` creation using an example {term}`result` and keypair fingerprint.

`````{tab-set}

````{tab-item} Command line interface
```shell
qiime tools annotation-create \
--input-path my-result.qza \
--annotation-type Signature \
--name 'mysignature' \
--fingerprint '7B2B 042B D7F2 7AE4 DF86  18D0 80D7 286B 1D44 DB32' \
--output-path my-signed-result.qza
```
````

````{tab-item} Python 3 API
```python
import qiime2
from qiime2.core.annotate import Signature

result = qiime2.sdk.Result.load('my-result.qza')
signature = \
    Signature(name='mysignature',
              fingerprint='7B2B 042B D7F2 7AE4 DF86  18D0 80D7 286B 1D44 DB32')

result.add_annotation(signature)
result.save('my-signed-result.qza')
```
````
`````

## Verifying a Signature

Once you have created a signed {term}`result`, you can share this with other people (colleagues, research collaborators, etc) and they will be able to verify who created this {term}`result`.
This is something you may want to do when training classifiers (to assert the identity of the person who performed this), or if you are dealing with any sort of sensitive study and would like to confirm that the {term}`result` you have received was created from a trusted source.

In order to verify the {term}`signature` on an existing {term}`result`, you will first need to have the signer's public key added to your GPG keyring.
On the other end, if you have sent a signed {term}`result` to someone else, you will need to share your public key with them in order for them to verify the signed {term}`result` came from you.

[This section](https://www.gnupg.org/gph/en/manual.html#AEN57) of the GPG documentation walks through exchanging public keys, which includes both how to export your public key to share with someone, and how to import someone else's public key to add to your keyring.

Once you have the signer's public key added to your keyring, you can verify the signed {term}`result`.

Below is the workflow for verifying a signed {term}`result` using an example {term}`result`.

`````{tab-set}

````{tab-item} Command line interface
```shell
qiime tools signature-verify \
--input-path signed-result.qza \
--name 'signature-name'
```
````

````{tab-item} Python 3 API
```python
import qiime2

result = qiime2.sdk.Result.load('signed-result.qza')
result.verify('signature-name')
```
````
`````

If the {term}`signature` is verified successfully, you will see the following:

`````{tab-set}

````{tab-item} Command line interface
```shell
Signature signature-name on Result signed-result.qza verified successfully.
```
````

````{tab-item} Python 3 API
```python
'Signature `signature-name` verified successfully.'
```
````
`````

If {term}`signature` verification fails, you will receive an error message with details on the failure.

Note that the `--name` parameter corresponds to the name of the {term}`annotation` (i.e. the {term}`signature` name).
If you do not know the name of the {term}`signature`, you can run the following command to list all {term}`annotations <annotation>` that have been added to the {term}`result` and pull the name of the {term}`signature` from that list.

`````{tab-set}

````{tab-item} Command line interface
```shell
qiime tools annotation-list \
--input-path signed-result.qza

ID:        ccbedc23-03f0-4092-ac24-4eeed467ece1
name:      signature-name
type:      Signature
```
````

````{tab-item} Python 3 API
```python
import qiime2

result = qiime2.sdk.Result.load('signed-result.qza')

for annotation in result.iter_annotations():
    print(f"Name: {annotation.name}\nType: {annotation.annotation_type}")

Name: signature-name
Type: Signature
```
````
`````
