# Audit Talent Layer

commit 9e720f3986bae4bb97c10b0dd9d2e1a3075fc81a

## Vulnerabilities
### Block overflow 
No loop, no problem.
To be checked

### Randomness 
No randomness

### Reentrancy : 
Some are subject to reentrancy but state is modified before external calls.
To be detailed

### Math 
Solidity 0.8 : no check are necessary for under/overflows
1 rounding issue (low)

### Centralization : 
Upgradable : high risk - give ownership to a DAO or multis wallet.

### msg.sender scope

### Right management
Check rightmanagement.md 

## Protections
Escrow
Add a pause cirxuit breaker
Id : allow to change the wallet if compromised

Release : add a speed bumper to save funds in case of id theft

Circuit breakers
Speed bumps
Rate limiter
Automatic deprecation


## Code base analysis
### Tests 
Working fine
### Coverage:  high : cannot be run.
"CompilerError: Stack too deep."

### Slither 
Runtime report > slither-report.txt







