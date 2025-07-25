---
platform: ios
title: Weak Hashing Algorithms
id: MASTG-TEST-0211
type: [static, dynamic]
weakness: MASWE-0021
profiles: [L1, L2]
---

## Overview

To test for the use of weak hashing algorithms in iOS apps, we need to focus on methods from cryptographic frameworks and libraries that are used to perform hashing operations.

- **CommonCrypto**: [CommonDigest.h](https://opensource.apple.com/source/CommonCrypto/CommonCrypto-36064/CommonCrypto/CommonDigest.h) defines the following **hashing algorithms**:
    - `CC_MD2`
    - `CC_MD4`
    - `CC_MD5`
    - `CC_SHA1`
    - `CC_SHA224`
    - `CC_SHA256`
    - `CC_SHA384`
    - `CC_SHA512`

- **CryptoKit**: Supports three cryptographically secure **hashing algorithms** and two insecure ones in a dedicated class called [`Insecure`](https://developer.apple.com/documentation/cryptokit/insecure):
    - `SHA256`
    - `SHA384`
    - `SHA512`
    - `Insecure.MD5`
    - `Insecure.SHA1`

Note: the **Security** framework only supports asymmetric algorithms and is therefore out of scope for this test.

## Steps

1. Run a static analysis tool such as @MASTG-TOOL-0073 on the app binary, or use a dynamic analysis tool like @MASTG-TOOL-0039, and look for uses of the cryptographic functions that perform hashing operations.

## Observation

The output should contain the disassembled code of the functions using the relevant cryptographic functions.

## Evaluation

The test case fails if you can find the use of weak hashing algorithms within the source code. For example:

- MD5
- SHA-1

**Stay up-to-date**: This is a non-exhaustive list of weak algorithms. Make sure to check the latest standards and recommendations from organizations such as the National Institute of Standards and Technology (NIST), the German Federal Office for Information Security (BSI) or any other relevant authority in your region.

**Context Considerations**:

To reduce false positives, make sure you understand the context in which the algorithm is being used before reporting the associated code as insecure. Ensure that it is being used in a security-relevant context to protect sensitive data.

For example, using MD5 for hashing passwords is considered weak, but using MD5 for checksums or non-cryptographic purposes is acceptable.
