## Findings

### Coverage report cannot be run
Severity
High

Location
Build process

Description
When running the coverage, the compilation fails with this error: "CompilerError: Stack too deep."
Without coverage, it becomes more difficult to identify parts of the code that are not covered by tests. This could lead to uncovered code.
Even if coverage is not a garantee of code validity, it's a very useful tool during the development process to improve code security.

Remediation
Fix the compilation issue.

### Calls to sending ether are unchecked
Severity
Medium

Location
TalentLayerEscrow#613,620,746,754,784,787,797,798,969

Description
Calls used for sending ether are unchecked. If the reciepient is a contract with no payable fallback, the call will silently fail.
The behavior of the contract won't be blocked, but the funds will remain locked in the contract. This issue is mitigated by the possibility of upgrading the contract.

Remediation
There are different options to fix this:
- Allow to collect the lost funds on the contract. This would likely give way more power to a centralized account.
- Check transfer has been done correctly or revert. This could lead to blocking situations.
- Fallbak to wrapped ether if call failed. This could lead to situation where the recipient contract does not know how to handle ERC20.

### Unchecked transfered amount
Severity
Medium

Location
TalentLayerEscrow::createTransaction#478

Description
ERC20 transfered value is not checked. The call could work without the correct amount to be transfered. If less tokens are transfered, this would lead to an incoherent state and prevent the transaction from being finalized. This issue is mitigated by the fact that the allowed tokens are white listed. 
In the current state, taxed tokens are not supported by the platform.

Remediation
Check the amount actually transfered to the escrow.

### Math rounding error
Severity
Medium
Location 
TalentLayerEscrow::\_executeRuling#791-792

Description
In case the amount is odd, 1 wei will remain in the contract.

Remediation
Send first half of the amount and then subsctract the amount sent for the second tranfer.


### Duplicate event can be fired
Severity
Depends on the event's usage

Location
TalentLayerEscrow::\_executeRuling#794-795

Description
Since the amount is set to 0 before calling \_reimburse and \_release, emit PaymentCompleted(transaction.serviceId); will be called twice.

Remediation
Avoid calling it twice in that edge case.

### Delegate cannot call a function
Severity 
Low

Location
TalentLayerEscrow::payArbitrationFeeBySender
TalentLayerEscrow::payArbitrationFeeByReceiver

Description 
payArbitrationFeeBySender and payArbitrationFeeByReceiver cannot be called by delegates whereas other function can.
It seems logic they could do so.

Remediation
Authorize delegates to call those functions

### Outdated comment
Severity
Low

Location
TalentLayerService#40

Description
The described param seems no longer used.

Remediatino
Remove the line

### Invalid error message
Severity
Low

Location
TalentLayerService#336

Description
The error message seems not correct.

Remediatino
"This proposal is already updated" -> "This proposal is already validated"

### msg.sender scope
Severity
Low

Location
TalentLayerArbitrator#127.

Description
msg.sender is called in an internal function
Suggestion
Pass the recipient as param of the function or inline the function since it's used only once.

### Tautology
Severity
Low

Location
TalentLayerReview.mint#132

Description
\_rating >= 0 is always true
https://github.com/crytic/slither/wiki/Detector-Documentation#tautology-or-contradiction

Suggestion
Remove the check from the require statement

### Dead code
Severity
Low

Location
TalentLayerReview::\_burn#216
TalentLayerID.sol::\_burn#391

Description
Code is never used and should be removed

Remediation
Remove useless code

### Missing inheritance
Severity
Low

Location
TalentLayerEscrow#15
TalentLayerID#19
TalentLayerPlatformID#16
TalentLayerReview#25
TalentLayerService#16

Description
Class does not inherit from interface. This can lead to discrepencies between the interface and implementation.

Remediation
Inherit from interface.


### Missing zero address validation
Severity 
Low

Location 
TalentLayerArbitrator#52
TalentLayerEscrow#326
TalentLayerEscrow::updateProtocolWallet#396
TalentLayerID#108
TalentLayerReview#81
TalentLayerService#187

Decription 
addresses passed as params are not checked for nullity.
https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation

Remediation
Require a valid address in the initializer

### Boolean comparison
Severity
Notice

Location
TalentLayerService.updateAllowedTokenList#377

Description
if (\_tokenAddress == address(0) && \_status == false)
https://github.com/crytic/slither/wiki/Detector-Documentation#boolean-equality

Remediation 
Remove boolean comparison 
if (\_tokenAddress == address(0) && !\_status)

### Transaction state machine
Severity
Suggestion

Location
TalentLayerEscrow

Description
Once the dispute is created, the only way to resolve the transaction is a ruling to happen. If the owner of the platform never responds, the transaction will get stuck and funds locked forever. This could happen because of key loss or end of project for example.

Suggestion
Add a timeout mechanism with a timer that can be updated by the plaform owner to ensure the owner is still alive.

### Create a modifier to improve readability
Severity 
Suggestion

Location
TalentLayerPlatformID::updateProfileData
TalentLayerPlatformID::updateOriginServiceFeeRate
TalentLayerPlatformID::updateOriginValidatedProposalFeeRate
TalentLayerPlatformID::updateArbitrator
TalentLayerPlatformID::updateArbitrationFeeTimeout
TalentLayerPlatformID::updateServicePostingFee
TalentLayerPlatformID::updateProposalPostingFee
 
Description 
The same requirement is copied multiple times.
require(ownerOf(\_platformId) == msg.sender, "You're not the owner of this platform");

Suggestion
Create a modifier to improve readability and maintenability.

### Anyone can create a dispute
Severity 
Notice

Location
TalentLayerArbitrator::createDispute

Description
Anyone can call the function. Valitidity of platformId is unchecked. 
Anyone could create disputes for only the gas cost.
But there is no reason for doing so.

Suggestion  
Check platformId validity