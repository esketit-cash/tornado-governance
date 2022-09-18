# Tornado governance [![build status](https://github.com/esketit-cash/tornado-governance/actions/workflows/build.yml/badge.svg)](https://github.com/esketit-cash/tornado-governance/actions/workflows/build.yml) [![Coverage Status](https://coveralls.io/repos/github/tornadocash/tornado-governance/badge.svg?branch=master)](https://coveralls.io/github/tornadocash/tornado-governance?branch=master)

## Description
This repository holds all the tornado.cash governance upgrades and original governance contracts.

## Documentation
All high-level documentation can be find [here](https://docs.tornado.cash/general/governance).

## Code architecture
Tornado governance infrastructure consists of two types of repository:
1. **Governance repository** (this one) - contains the original governance contracts and parts of proposals that upgrade governance itself via loopback proxy. So here you can compile the actual version of the governance contract.
2. **Proposal repository** - a separate repository for each governance proposal. It contains the full codebase of a proposal.

### Loopback proxy
[Loopback proxy](https://github.com/esketit-cash/tornado-governance/blob/master/contracts/v1/LoopbackProxy.sol) is a special type of proxy contract that is used to add the ability to upgrade the proxy itself. This way governance proposals can upgrade governance implementation.

### Proposal creation manual
To create your custom governance proposal you need to:
1. Create a proposal repository (for [example](https://github.com/Rezan-vm/tornado-relayer-registry)):
  - a proposal is executed from the governance contract using delegatecall of __executeProposal()__ method
  - as a proposal is executed using delegatecall, it should not store any storage variables - use constants and immutable variables instead
2. If your proposal is upgrading governance itself, you need to create a pull request to the governance repository. PR should add folder with governance contract upgrade (separate folder in contracts folder - for [example](https://github.com/esketit-cash/tornado-governance/pull/6/commits/5f36d5744a9f279a58e9ba1f0e0cd9d493af41c7)).
3. Deploy proposal. The proposal must be smart contracts with verified code.
4. Go to Tornado governance [UI](https://tornadocash.eth.limo/governance) to start the proposal voting process.


## Tests/Coverage

Setting up the repository:

```bash
git clone https://github.com/esketit-cash/tornado-governance.git
yarn
cp .env.example .env
```

Please fill out .env according to the template provided in it. Please ensure that all of the example values are set to the correct addresses.

To run test scripts:

```bash
    yarn test
```

To run tests coverage:

```bash
    yarn coverage
```