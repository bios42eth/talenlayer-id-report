## Findings
### Calls to sending ether are unchecked
Severity
medium
Location
TalentLayerEscrow:613,620,746,754,784,787,797,798,969

Description
Calls used for sending ether are unchecked. If the reciepient is a contract with no payable fallback, the call will silently fail.
The behavior of the contract won't be blocked, but the funds will remain locked in the contract. This issue is mitigated by the possibility of upgrading the contract.

Remediation
There are different options to fix this :
- Allow to collect the lost funds on the contract. This would likely give way more power to a centralized account.
- Check transfer has been done correctly or revert. This could lead to blocking situations.
- Fallbak to wrapped ether if call failed. This could lead to situation where the recipient contract does not know how to handle ERC20.

### Math rounding error
Severity
medium
Location 
TalentLayerEscrow::\_executeRuling 791-792

Description
In case the amount is odd, 1 wei will remain in the contract.

Remediation
Send first half of the amount and then subsctract the amount sent for the second tranfer.

### Duplicate event can be fired
Severity
Depends on the event's usage

Location
TalentLayerEscrow::\_executeRuling 794-795

Description
Since the amount is set to 0 before calling \_reimburse and \_release, emit PaymentCompleted(transaction.serviceId); will be called twice.

Remediation
Avoid calling it twice in that edge case.

### Delegate cannot call a function
Severity 
low
Location
TalentLayerEscrow::payArbitrationFeeBySender
TalentLayerEscrow::payArbitrationFeeByReceiver

Description 
payArbitrationFeeBySender and payArbitrationFeeByReceiver cannot be called by delegates whereas other function can.
It seems logic they could do so.

Remediation
Authorize delegates to call those functions

### Create a modifier to improve readability
TalentLayerPlatformID::
updateProfileData, updateOriginServiceFeeRate, updateOriginValidatedProposalFeeRate
updateArbitrator
updateArbitrationFeeTimeout
updateServicePostingFee
updateProposalPostingFee
 
Description 
The same requirement is copied multiple time.

Suggestion
Create a modifier to improve readability and maintenability.


### Anyone can create a dispute
Severity 
notice
Location
TalentLayerArbitrator::createDispute

Description
Anyone can call the function. Valitidity of platformId is unchecked. 
Anyone could create disputes for only the gas cost.
But there is no reason for doing so.

Suggestion  
Check platformId validity