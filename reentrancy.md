# Reentrancy
## Overview
Multiple reentrancy points have been detected via ether transfers or token transfers.
They are handled correctly by modifing sate before the reentrancy point.

## TalentLayerEscrow

arbitrationFeeTimeout:
loc:614,621
Could allow reentrance from a buyer or a service provider.

\_raiseDispute:
loc:747,755
Could allow reentrance from a buyer or a service provider.
Called by: payArbitrationFeeBySender & payArbitrationFeeByReceiver

\_executeRuling:
loc:785,788,798,799
Could allow reentrance from a buyer or a service provider.
Called by: arbitrationFeeTimeout & rule

\_safeTransferBalance:
loc:970,972
Could allow reentrance from \_reciepient or token owner.
Called by: claim, \_release, \_reimburse

\_release:
loc:857
Could allow reentrance via \_safeTransferBalance.
Called by: \_executeRuling and release

\_reimburse:
loc:875
Could allow reentrance via \_safeTransferBalance.
Called by: \_executeRuling and reimburse

createTransaction:
loc:478
transferFrom allows reentrancy. But the action is done last and state modified before.

## TalentLayerID
withdraw:
loc:275
Could allow reentrance for the owner. But it would make no sense.

mint:
loc:202
\_safeMint can be used as reentrancy point by recipient but it would not be useful

whitelistMint:
loc:221
\_safeMint can be used as reentrancy point by recipient but it would not be useful

freeMint:
loc:290
\_safeMint can be used as reentrancy point by recipient but it would not be useful

## TalentLayerPlatformID
withdraw:
loc:236
Could allow reentrance for the owner. But it would make no sense.

mint:
loc:236
\_safeMint can be used as reentrancy point by recipient but it would not be useful

mintForAddress:
loc:250
\_safeMint can be used as reentrancy point by recipient but it would not be useful

## TalentLayerArbitrator
loc:127
\_giveRuling: calls an external contract, but the gas limit prevents from reentrancy.

## TalentLayerReview
loc:146 
\_safeMint can be used as reentrancy point by recipient. State is protected.
