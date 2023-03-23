# Scenarios exploration

## Arbitrator update
What if an arbitrator is updated while a dispute is in progress ?
When a transaction is created in TalentLayerEscrow, the arbitrator is assigned. Afterward, all requests to an arbitrator are related to the one assigned at the creation.
A platform could change the arbitrator without issue: all previous transactions would get routed to the old arbitrator and state would remain coherent.

## Arbitrator's owner's priviledge
What is an arbitrator owner is compromised ?
Arbitrator owner cannot be changed. In case of hack or wallet compromision, the owner could rule all pending disputes.
The platform would have to change the arbitrator but any dispute raised with the compromized arbitrator in older transactions could be ruled by the hacker.
Suggestion: owner of an arbitrator has great priviledge and ownership should be secured.

## Fees calculation
Could fee calculation lead to incoherent state ?
Fees percentages are stored in each transaction. If they are updated, open transaction will still use the old values. This should maintain a coherent state.

## Transaction state machine
Could a transaction remained stuck ?
As long as no dispute is raised, release and reimburse can be used to resolve the transaction.
Once arbitration fees are sent, release and reimburse are locked and a timeout protects from locking the transaction if the other party does not sent arbitration fees.
Once the dispute is created, the only way to resolve the transaction is a ruling to happen. If the owner of the platform never responds, the transaction will get stuck and funds locked forever. This could happen because of key loss or end of project for example.
Suggestion: add a timeout mechanism with a timer that can be updated by the plaform owner to ensure the owner is still alive.

NoDispute -> Reimburse || Release -> NoDispute
NoDispute -> payArbitrationFeeBySender -> WaitingReceiver
NoDispute -> payArbitrationFeeByReceiver -> WaitingSender
WaitingReceiver -> payArbitrationFeeBySender -> WaitingSender
WaitingReceiver -> payArbitrationFeeBySender -> DisputeCreated
WaitingReceiver -> arbitrationFeeTimeout -> Resolved
WaitingSender -> payArbitrationFeeBySender -> WaitingReceiver
WaitingSender -> payArbitrationFeeBySender -> DisputeCreated
WaitingSender -> arbitrationFeeTimeout -> Resolved
DisputeCreated -> rule -> Resolved