# Libbitcoin Consensus

Forked from https://github.com/libbitcoin/libbitcoin-consensus

## Goal
 * remove bitcoin core consensus files make it work with local copy of bitcoin-sv.  

## Dependencies

This library requires [libsecp256k1](https://github.com/libbitcoin/secp256k1). The test cases have a boost dependency.

## Configure Options

There is a dependency on [boost test](http://www.boost.org/doc/libs/1_57_0/libs/test/doc/html/index.html) for `make check` builds (tests). The `--without-tests` option disables test builds and eliminates the boost check during configure.

## Supported Platforms

**Ubuntu** (gcc and clang) and **OSX** (clang) are regularly tested via a [travis build matrix](https://travis-ci.org/libbitcoin/libbitcoin-consensus). There are also Visual Studio 2017, 2015 and 2013 solutions for **Windows** builds, however the VS2013 build is not currently supported due to a compiler incompatibility introduced in recent versions.

# About

This library includes the following 35 files considered to be bitcoin script consensus-critical (in bitcoin core). These files are identical to those used in version 0.19.0 of bitcoin core.

```
amount.h
attributes.h
hash.cpp
hash.h
prevector.h
pubkey.cpp
pubkey.h
serialize.h
span.h
tinyformat.h
uint256.cpp
uint256.h
version.h
compat/byteswap.h
compat/endian.h
crypto/common.h
crypto/hmac_sha512.cpp
crypto/hmac_sha512.h
crypto/ripemd160.cpp
crypto/ripemd160.h
crypto/sha1.cpp
crypto/sha1.h
crypto/sha256.cpp
crypto/sha256.h
crypto/sha512.cpp
crypto/sha512.h
primitives/transaction.cpp
primitives/transaction.h
script/interpreter.cpp
script/interpreter.h
script/script.cpp
script/script.h
script/script_error.h
util/strencodings.cpp
util/strencodings.h
```

# Libbitcoin Integration

Libbitcoin natively implements consensus checks that are redundant with `libbitcoin-consensus`. Libbitcoin includes a full bitcoin client and server SDK. This includes the full node implementation [libbitcoin-node](https://github.com/libbitcoin/libbitcoin-node), which builds on [libbitcoin](https://github.com/libbitcoin/libbitcoin) and [libbitcoin-blockchain](https://github.com/libbitcoin/libbitcoin-blockchain).

The `libbitcoin-blockchain` configuration provides the `--with-consensus` option. This allows the developer to select either `libbitcoin` native or `libbitcoin-consensus` checks. The option defaults to `yes` so that by default all `libbitcoin-node` and `libbitcoin-server` builds use the same script consensus checks as a bitcoin core node.
