# Test project

This project aims to test Voting.sol smart contract with truffle.

## Install

Add a .env file with your custom mnemonic:
```
MNEMONIC= //Your mnemonic
```
Then you can play:
```
truffle migrate --reset
truffle test
```
## Tests

### Contract initialization
- Owner is deployer
- Initial workflow status is RegisteringVoters
- Proposals list is empty
- Initial winningProposalID is 0

### Getters
**getVoter**
- Only voter is allowed to get voter
- Get unregistered voter
- Get registered voter

**getOneProposal**
- Only voter is allowed to get proposal

### Workflow transitions
**Transition from RegisteringVoters**
- Only owner is allowed to change status
- Illegal transitions fail
- Transition to ProposalsRegistrationStarted is successful

**Transition from ProposalsRegistrationStarted**
- Only owner is allowed to change status
- Illegal transitions fail
- Transition to ProposalsRegistrationEnded is successful

**Transition from ProposalsRegistrationEnded**
- Only owner is allowed to change status
- Illegal transitions fail
- Transition to VotingSessionStarted is successful

**Transition from VotingSessionStarted**
- Only owner is allowed to change status
- Illegal transitions fail
- Transition to VotingSessionEnded is successful

**Transition from VotingSessionEnded**
- Only owner is allowed to change status
- Illegal transitions fail
- Transition to VotesTallied is successful

__Winner management__
- Genesis proposal wins if no vote
- High score proposal wins
- First registrated proposal wins on tie vote

**Transition from VotesTallied**
- Illegal transitions fail

### Workflow restrictions on operations
- Illegal operations on RegisteringVoters status fail
- Illegal operations on ProposalsRegistrationStarted status fail
- Illegal operations on ProposalsRegistrationEnded status fail
- Illegal operations on VotingSessionStarted status fail
- Illegal operations on VotingSessionEnded status fail
- Illegal operations on VotesTallied status fail

### Operations
**Voter registration**
-  Only owner is allowed to register voter
- Voter registration is successful
- Voter double registration is forbiden

**Proposal registration**
- Only voter is allowed to register proposal
- Proposal is not empty
- Proposal registeration is successful
- Multiple proposal from same voter is allowed

**Voting**
- Only voter is allowed to vote
- Vote fails for proposal doesn't exist
- Vote registration is successful
- Multiple vote from same voter is forbiden
