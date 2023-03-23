# Audit Talent Layer

## Scope
url
https://github.com/TalentLayer/talentlayer-id-contracts/

commit 
9e720f3986bae4bb97c10b0dd9d2e1a3075fc81a

files
contracts/TalentLayerArbitrator.sol
contracts/TalentLayerEscrow.sol
contracts/TalentLayerID.sol
contracts/TalentLayerPlatformID.sol
contracts/TalentLayerReview.sol
contracts/TalentLayerService.sol

## Risks analysis
See risks.md

## Vulnerabilities
### Block overflow 
No loop is used to process datas. This issue should not raise.

### Randomness 
No randomness is used in the project.

### Reentrancy
Some are subject to reentrancy but state is modified before external calls.
See reentrancy.md

### Math 
Solidity 0.8 is used, then no check are necessary for under/overflows.
1 math issue found. See findings.md

### msg.sender scope
Scope for msg.sender and \_msgSender() has been checked and seems correctly used : it's only called in constructors, modifiers or external functions.
The only usage in an internal function is in TalentLayerArbitrator::loc:127.
But the function is acutally called only once.

### Time stamp
block.timestamp is only used for real timed event comparison.
It's very unlikely that the miners could take advantage of manipulating it.

### Right management
See rightmanagement.md 

## Centralization
Owner has a very limited role and can only access maintenance functions, except regarding to the upgrade of the contracts.
Thus, owner can potentially do whatever it wants.
It's recommended to use a mechanism involving at least 2 people to validate an owner transaction.

## Protections
Escrow
The Escrow contract holds all the funds and can be a target. In case of issue, the only way to fix a problem is to update the contract. It might take some time to do it in a reliable way. 
To gain time, I'd recommend adding a pause circuit breaker that could block all transfers for investigation in case of trouble.

I would consider adding a delay between a release and the actual funds transfer to allow identifing issues before releasing funds for real. 

## Code base analysis
### Tests 
Tests have been run and work fine. They seem well written with expectations.
It's difficult to evaluate the scope covered by the tests because of missing coverage.

### Coverage
Test coverage cannot be run. I get this error: "CompilerError: Stack too deep."

### Slither 
Runtime report > slither-report.txt



