# Right management
## TalentLayerArbitrator
createDispute: Anyone can call the function. Valitidity of platformId is unchecked. 

giveRuling: only platform owner can call it.

## TalentLayerEscrow
updateProtocolEscrowFeeRate & updateProtocolWallet: only owner
createTransaction: can only be called by the service owner or a trusted forwarder.

release:  only the buyer, a trusted forwarder or a delegate can call the function.

reimburse: only the service provider, a trustedforwarder or a delegate can call the function 

payArbitrationFeeBySender: only the buyer or a trustedforwarder can call the function. A delegate cannot call this function.

payArbitrationFeeByReceiver: only the service provider or a trustedforwarder can call the function. A delegate cannot call this function.

arbitrationFeeTimeout: anyone can call this function, on purpose !

submitEvidence: only the buyer or the service provider, a trusted forwarder or a delegate can call the function.

appeal: anyone can call the function. Checks are delegated to the arbitrator appeal function

claim: anyone can call the function, but the logic works fine. If called by the owner, access control is checked.

claimAll: empty function

rule: only the arbitrator can call the function

## TalentLayerID
mint: anyone can mint when mint is open

whitelistMint: anyone whitelisted can mint when open

updateProfileData: only owner, a trusted forwarder or delegate of the profile can call the function

addDelegate, removeDelegate: only owner or a trusted forwarder can call the function

updateMintFee, withdraw, freeMint, setWhitelistMerkleRoot, updateMintStatus: only owner

## TalentLayerPlatformID
mint, mintForAddress: only an authorised minter

updateProfileData, updateOriginServiceFeeRate, updateOriginValidatedProposalFeeRate, updateArbitrator, updateArbitrationFeeTimeout, updateServicePostingFee, updateProposalPostingFee: only the owner of the platform

whitelistUser, updateMintStatus: only owner role

updateMintFee, withdraw, addArbitrator, removeArbitrator, updateMinArbitrationFeeTimeout: default admin role

## TalentLayerReview
mint: only a buyer, service provider, a trusted forwarder or a delegate can call the function based on a finished service.

## TalentLayerService
createService, updateServiceData, cancelService: only a buyer, a trusted forwarder of a delegate can create one.

createProposal, updateProposal: only a service provider, a trusted forwarder of a delegate can create one.

afterDeposit, afterFullPayment: only escrow role can call the function
updateAllowedTokenList,  onlyowner
