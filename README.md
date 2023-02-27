# Test project

This project aims to test Voting.sol smart contract with truffle.

## Install

Initialization:
```
git clone https://github.com/asaintvanne/alyra-voting-test.git
cd alyra-voting-test
npm install
```

Configure your network in `truffle-config.js` or use one already registered. If necessary, create a `.env` file with your parameters:
```
touch .env
MNEMONIC= //your mnemonic
```

Then you can play:
```
truffle test --network yourNetwork
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
  * Unknown address is not allowed
  * Owner address is not allowed
- Unregistered voter is retrieved
- Registered voter is retrieved

**getOneProposal**
- Only voter is allowed to get proposal
  * Unknown address is not allowed
  * Owner address is not allowed

### Workflow transitions
**Transition from RegisteringVoters**
- Only owner is allowed to change status
  * Unknown address is not allowed
  * Voter address is not allowed
- Illegal transitions fail
  * Transition to ProposalsRegistrationEnded is not allowed
  * Transition to VotingSessionStarted is not allowed
  * Transition to VotingSessionEnded is not allowed
  * Transition to VotesTallied is not allowed
- Transition to ProposalsRegistrationStarted is successful
  * Genesis proposal is not created before status transition
  * Event WorkflowStatusChange is emitted with good parameters
  * Workflow status is correctly set
  * Genesis proposal is created

**Transition from ProposalsRegistrationStarted**
- Only owner is allowed to change status
  * Unknown address is not allowed
  * Voter address is not allowed
- Illegal transitions fail
  * Transition to ProposalsRegistrationStarted is not allowed
  * Transition to VotingSessionStarted is not allowed
  * Transition to VotingSessionEnded is not allowed
  * Transition to VotesTallied is not allowed
- Transition to ProposalsRegistrationEnded is successful
  * Event WorkflowStatusChange is emitted with good parameters
  * Workflow status is correctly set

**Transition from ProposalsRegistrationEnded**
- Only owner is allowed to change status
  * Unknown address is not allowed
  * Voter address is not allowed
- Illegal transitions fail
  * Transition to ProposalsRegistrationStarted is not allowed
  * Transition to ProposalsRegistrationEnded is not allowed
  * Transition to VotingSessionEnded is not allowed
  * Transition to VotesTallied is not allowed
- Transition to VotingSessionStarted is successful
  * Event WorkflowStatusChange is emitted with good parameters
  * Workflow status is correctly set

**Transition from VotingSessionStarted**
- Only owner is allowed to change status
  * Unknown address is not allowed
  * Voter address is not allowed
- Illegal transitions fail
  * Transition to ProposalsRegistrationStarted is not allowed
  * Transition to ProposalsRegistrationEnded is not allowed
  * Transition to VotingSessionStarted is not allowed
  * Transition to VotesTallied is not allowed
- Transition to VotingSessionEnded is successful
  * Event WorkflowStatusChange is emitted with good parameters
  * Workflow status is correctly set

**Transition from VotingSessionEnded**
- Only owner is allowed to change status
  * Unknown address is not allowed
  * Voter address is not allowed
- Illegal transitions fail
  * Transition to ProposalsRegistrationStarted is not allowed
  * Transition to ProposalsRegistrationEnded is not allowed
  * Transition to VotingSessionStarted is not allowed
  * Transition to VotingSessionEnded is not allowed
- Transition to VotesTallied is successful
  * Event WorkflowStatusChange is emitted with good parameters
  * Workflow status is correctly set

**Winner management**
- Genesis proposal wins if no vote
- High score proposal wins
- First registrated proposal wins on tie vote

**Transition from VotesTallied**
- Illegal transitions fail
  * Transition to ProposalsRegistrationStarted is not allowed
  * Transition to ProposalsRegistrationEnded is not allowed
  * Transition to VotingSessionStarted is not allowed
  * Transition to VotingSessionEnded is not allowed
  * Transition to VotesTallied is not allowed

### Workflow restrictions on operations
- Illegal operations on RegisteringVoters status fail
  * addProposal is not executable
  * setVote is not executable
- Illegal operations on ProposalsRegistrationStarted status fail
  * addVoter is not executable
  * setVote is not executable
- Illegal operations on ProposalsRegistrationEnded status fail
  * addProposal is not executable
  * addVoter is not executable
  * setVote is not executable
- Illegal operations on VotingSessionStarted status fail
  * addProposal is not executable
  * addVoter is not executable
- Illegal operations on VotingSessionEnded status fail
  * addProposal is not executable
  * addVoter is not executable
  * setVote is not executable
- Illegal operations on VotesTallied status fail
  * addProposal is not executable
  * addVoter is not executable
  * setVote is not executable

### Operations
**Voter registration**
- Legal voter registration is successful
  * Event VoterRegistered is emitted with good parameters
  * Voter is registered
  * Voter hasVoted flag is set to false
  * Voter voted proposal ID is set to genesis proposal
- Only owner is allowed to register voter
  * Unknown address is not allowed to register voter
  * Registered voter address is not allowed to register voter
- Voter double registration is forbiden

**Proposal registration**
- Only voter is allowed to register proposal
  * Unknown address is not allowed to register proposal
  * Owner address is not allowed to register proposal
- Empty proposal is not allowed
- Legal proposal registration is successful
  * Event ProposalRegistered is emitted with good parameters
  * Description is equal to sent value
  * Number of vote is initialized to 0
- Multiple proposal from same voter is allowed

**Voting**
- Only voter is allowed to vote
  * Unknown address is not allowed to vote
  * Owner address is not allowed to vote
- Vote fails for proposal doesn't exist
- Legal vote registration is successful
  * Event Voted is emitted with good parameters
  * Voter hasVoted flag is set to true
- Multiple vote from same voter is forbiden
