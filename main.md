# Audit Talent Layer

commit 9e720f3986bae4bb97c10b0dd9d2e1a3075fc81a

## Risks analysis
See risks.md

## Vulnerabilities
### Block overflow 
No loop is used to process datas. This issue should not raise.

### Randomness 
No randomness is used in the project.

### Reentrancy: 
Some are subject to reentrancy but state is modified before external calls.
See reentrancy.md

### Math 
Solidity 0.8 is used, then no check are necessary for under/overflows.
1 math issue found. See in findings.md

### Centralization: 
Upgradable: high risk - give ownership to a DAO or multis wallet.

### msg.sender scope
Scope for msg.sender and \_msgSender() has been checked and seems correctly used : it's only called in constructors, modifiers or external functions.
The only usage in an internal function is in TalentLayerArbitrator::loc:127.
But the function is acutally called only once.

### Right management
See rightmanagement.md 

## Protections
Escrow
Add a pause cirxuit breaker
Id: allow to change the wallet if compromised

Release: add a speed bumper to save funds in case of id theft

Circuit breakers
Speed bumps
Rate limiter
Automatic deprecation

## Code base analysis
### Tests 
Working fine
Todo: assess them

### Coverage: cannot be run.
"CompilerError: Stack too deep."

### Slither 
Runtime report > slither-report.txt

## Gas optimization




