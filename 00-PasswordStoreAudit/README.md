# PasswordStore Smart Contract Security Review

This repository contains a **security review of the PasswordStore smart contract** as part of my smart contract auditing practice while studying security research from the Cyfrin security course.

## Audit Overview

| Category   | Details                  |
| ---------- | ------------------------ |
| Contract   | PasswordStore            |
| Audit Type | Practice Security Review |
| Findings   | 2 High, 1 Low            |
| Tools Used | Foundry, Anvil, Cast     |

---

## Findings Summary

| ID  | Title                                         | Severity |
| --- | --------------------------------------------- | -------- |
| H-1 | Storing password on-chain exposes it publicly | High     |
| H-2 | `setPassword` lacks access control            | High     |
| L-1 | Incorrect NatSpec documentation               | Low      |

---

# High Severity Findings

## H-1: Storing Password On-Chain Makes It Public

### Description

All data stored on-chain is publicly accessible and can be read directly from the blockchain.

The `PasswordStore::s_password` variable is intended to be private and only accessed through the `PasswordStore::getPassword()` function, which is intended to be callable only by the contract owner.

However, since the password is stored directly in contract storage, anyone can read it using blockchain inspection tools.

### Impact

Anyone can read the stored password directly from the blockchain, completely breaking the intended privacy of the protocol.

### Proof of Concept

The following steps demonstrate that the password can be read directly from contract storage.

1. Start a local blockchain

```
anvil
```

2. Deploy the contract and set the password

```
forge script script/Deploy.s.sol
```

3. Read the password directly from contract storage

```
cast storage <contract_address> 1
```

The returned value contains the password stored in the contract.

### Recommended Mitigation

Avoid storing plaintext passwords on-chain.

Possible solutions include:

**1. Hash the Password**

Store a hash instead of the plaintext password.

Example approach:

* Store `keccak256(password)`
* When verifying input, hash the provided password and compare hashes.

**2. Off-Chain Storage**

Store sensitive data off-chain and only keep a reference or hash on-chain.

**3. Encryption**

Encrypt the password before storing it on-chain. However, this requires secure key management and introduces additional complexity.

---

## H-2: `setPassword` Can Be Called by Anyone

### Description

The `PasswordStore::setPassword` function lacks access control, allowing any address to call it and change the password.

```solidity
function setPassword(string memory newPassword) external {
    s_password = newPassword;
    emit SetNewPassword();
}
```

The intended behavior appears to be that only the contract owner should be able to set the password.

### Impact

Unauthorized users can change the password, which may:

* Lock out the legitimate owner
* Disrupt system functionality
* Allow malicious manipulation of authentication logic

### Proof of Concept

Add the following test to `PasswordStore.t.sol`.

```solidity
function test_anyone_can_set_password(address randomAddress) public {
    vm.assume(randomAddress != owner);

    vm.prank(randomAddress);
    string memory expectedPassword = "myNewPassword";
    passwordStore.setPassword(expectedPassword);

    vm.prank(owner);
    string memory actualPassword = passwordStore.getPassword();

    assertEq(expectedPassword, actualPassword);
}
```

The test passes, demonstrating that any address can update the password.

### Recommended Mitigation

Restrict access to the contract owner.

```solidity
function setPassword(string memory newPassword) external {
    require(msg.sender == owner, "Only the owner can set the password");
    s_password = newPassword;
    emit SetNewPassword();
}
```

---

# Low Severity Findings

## L-1: Incorrect NatSpec Documentation

### Description

The NatSpec comment for `getPassword()` incorrectly references a parameter that does not exist.

```solidity
/*
 * @notice This allows only the owner to retrieve the password.
 * @param newPassword The new password to set.
 */
function getPassword() external view returns (string memory)
```

The function signature does not include a `newPassword` parameter.

### Impact

Incorrect documentation may confuse developers interacting with the contract.

### Recommended Mitigation

Remove the incorrect parameter documentation.

```diff
/*
 * @notice This allows only the owner to retrieve the password.
- * @param newPassword The new password to set.
 */
function getPassword() external view returns (string memory)
```

---

# Conclusion

This security review was conducted as part of my smart contract auditing practice. The objective of this review was to identify common vulnerabilities such as:

* Improper access control
* Sensitive data exposure
* Documentation inconsistencies

This repository documents my progress in learning smart contract security auditing and vulnerability analysis.
