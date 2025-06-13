# 🔍 Smart Contract Audit – BOB-USDCBridge

**Auditor:** B.Ali 
**Date:** June 13, 2025  
**Contract:** BOB-USDCBridge.sol  
**Framework:** Manual review + functional analysis

---

## 1. Overview

This smart contract is part of a system for transferring USDC tokens between different blockchain layers (Layer 1 ↔ Layer 2). The contract includes functions for locking, burning, and unlocking tokens, controlled by roles.

---

## 2. Scope of the Audit

- Review of `burnLockedUSDC()`, `lockUSDC()`, `unlockUSDC()` functions  
- Checking for vulnerabilities such as:  
  - Missing access control  
  - Incorrect burning logic  
  - Possible role abuse  
- Checking for optimizations and best practices

---

## 3. Findings

| Severity | ID | Title                                  | Status     |
|----------|----|----------------------------------------|------------|
| High     | #1 | `burnLockedUSDC()` burns all tokens    | Resolved   |
| Medium   | #2 | Missing role-based access control      | Acknowledged |

---

### ❗ Issue #1 – `burnLockedUSDC()` burns all tokens  
**Severity:** High  
**Description:**  
The `burnLockedUSDC()` function calls `usdc.burn(balance)`, where `balance = usdc.balanceOf(address(this))`. This burns all tokens held by the contract, regardless of whether they were actually "locked."

**Risks:**  
- Tokens accidentally sent directly to the contract address will also be burned.  
- No distinction between actual deposits and mistakenly sent tokens.

**Recommendation:**  
Only burn the amount explicitly locked in `deposits[l1][l2]`, not the entire balance.

---

### ⚠️ Issue #2 – Missing role-based access control  
**Severity:** Medium  
**Description:**  
The `burnLockedUSDC()` function lacks restrictions via `onlyRole()` or similar checks. This allows anyone to call the function and burn tokens.

**Recommendation:**  
Add `onlyRole(BURNER_ROLE)` or restrict access to the contract owner.

---

## 4. Summary and Recommendations

The contract is well-structured and solves a real problem — transferring assets between chains.  
However, critical aspects like access control and safe burning logic must be addressed before production use.

**Recommendations:**  
- Implement strict role-based access control  
- Add tests for unexpected inputs  
- Write unit tests for edge cases

---

## 5. Notes

> This is an independent audit for portfolio purposes only. It was not officially requested by the BOB project team. Public code was used for educational purposes.

