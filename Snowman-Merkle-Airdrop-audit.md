# Mini Audit Report – Snowman Merkle Airdrop

**Auditor:** B.Ali 
**Date:** 2025-06-13  
**Contract:** Snowman Merkle Airdrop

## 1. Overview

This smart contract implements a token airdrop using Merkle Proofs to verify eligible recipients. It allows users to claim tokens allocated to their address only once.

## 2. Findings

| Severity | ID  | Title                              | Status |
|----------|-----|----------------------------------|--------|
| Low      | #1  | No time limit for claiming tokens | Noted  |
| Medium   | #2  | Assumes token contract complies with ERC20 | Noted  |

### Issue #1 – No time limit for claiming tokens  
**Severity:** Low  
**Description:**  
The contract does not enforce any deadline for claiming tokens, which could lead to tokens being claimed long after the intended period.  
**Recommendation:**  
Consider adding a claim deadline to limit the claim period.

### Issue #2 – Assumes token contract complies with ERC20  
**Severity:** Medium  
**Description:**  
The contract assumes the token's `transfer` function returns a boolean value indicating success, as per ERC20 standard. If the token deviates from the standard, transfers may not behave as expected.  
**Recommendation:**  
Use OpenZeppelin’s SafeERC20 library for safer token transfers.

## 3. Security Measures

- Prevents double claiming by tracking claimed addresses.  
- Uses Merkle Proofs for efficient and secure verification of eligible addresses and amounts.

## 4. Summary

Overall, the contract is well implemented for its intended purpose. The main security checks are in place, but adding enhancements like claim deadlines and safer token transfer methods would improve robustness.

## 5. Notes

> This mini audit is for educational purposes and portfolio building.

