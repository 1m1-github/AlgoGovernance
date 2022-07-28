# AlgoGovernance
An economically efficient on-chain governance dApp based on Algorand

Voting, result tallying and querying, all on-chain

## How it works

During the voting period, voters send their `governance tokens` to the governance dApp together with their vote.
The governance tokens are locked in the dApp and the voter receives `receipt tokens` in exchange.
The dApp updates the vote count in its global state.
Once the voting period ends, receipt tokens can be exchanged back for `governance tokens`.
Also, any dApp can query the voting result from the governance dApp.

## Details

Since `receipt tokens` can be exchanged for `governance tokens` 1-to-1 after voting ends, the economic value of the 2 is almost the same. This allows other services that accept the same `governance tokens` to also accept `receipt tokens` instead and exchange them afterwards. This means the lock of the `governance tokens` is not a real lock.
The purpose of locking `governance tokens` is to avoid double voting. Local state cannot be used for this purpose, as voters could simply clear local state. Global state is (currently) too small to note every voted accounts id.

The governance dApp could implement different functions to evaluate the result from the vote counts. E.g. a threshold could be applied to give an 'undetermined' result if not enough votes were cast. More complex methods such as rank voting or STAR voting could be implemented using the same framework.

Using the ABI (https://developer.algorand.org/docs/get-details/dapps/smart-contracts/ABI/), any dApp can call the governance dApp and know the result of the vote.
A dApp could e.g. allow its own updating only if the governance dApp has a positive voting result. This gives updating power to the DAO governance token holders.

## Open Questions

### Allow clearing?
Should it be possible to *clear* the votes in the governance dApp after the voting ended? Or should each voting session be on a separate dApp which keeps the results forever.
On the one hand, not clearing the results seems cleaner. For a different voting session, just create a new dApp.
On the other hand, imagine a dApp that only allows updating if the governance dApp oks it. In this case, the dApp would have to keep changing the governance dApp id everytime there is a new voting session. And this would have to happen with the previous update, meaning project owners would need to plan the next 2 voting sessions in advance.
Allowing the clearing of governance dApp would allow dApps to keep using the same governance dApp to gateway its update. But then the question becomes, where to get the result of the previous voting session? Of course, it is always possible off-chain, but we are purely on-chain here.
Perhaps the results after clearing could be stored in the global state. As the size of the global state increases, more end results can be stored.
