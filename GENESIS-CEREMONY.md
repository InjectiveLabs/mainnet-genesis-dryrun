# Genesis Validator Ceremony

Welcome to the Injective Genesis Validator Ceremony!

## What is it?

This *is not* the launch of the Injective chain. Before a blockchain like the
Injective chain can launch, it needs to determine an initial validator set.

This is a ceremony to establish a decentralized initial validator set
that can be recommended for the Genesis State of the Injective Network.
This validator set is computed from the set of signed `gentx` transactions with non-zero INJ submitted during this genesis ceremony.

Before you consider participating in this ceremony, please read the entire
document.

Genesis transactions will be collected on Github in this repository and checked for validity by an automated script.
Genesis file collection will terminate on 7 June 2019 23:00 GMT. The final recommended genesis file will be published shortly after that time.

By participating in this ceremony and submitting a gen-tx, you are making a commitment to your fellow validators
that you will be around to bring your validator online by the recommended genesis time of 7 June 2019 23:00 GMT to launch the network. Note that you can start `injectived` 
with the recommended genesis file before that time and, assuming you configure it successfully, it will automatically start the peer-to-peer and consensus processes once the genesis timestamp is reached.

Please keep the following things in mind.

1. This process is intended for technically inclined people who have participated in Injective staking testnets. If you aren't already familiar with this process, you are advised against participating due to the risks involved. There is no need for you to participate if you feel unprepared - 
 you can create a validator or stake INJ any time after launch.
2. INJ staked during genesis will be at risk of 5% slashing if your validator double signs. If you accidentally misconfigure your validator setup, this can easily happen, and slashed INJ are not expected to be recoverable by any means. Additionally, if you double-sign, your validator will be [tombstoned](https://github.com/cosmos/cosmos-sdk/blob/master/docs/spec/slashing/07_tombstone.md) and you will be required to change operator and signing keys.
3. INJ staked during genesis or after will be locked up as part of the defense against long range attacks for 3 weeks. They can be re-delegated or undelegated, but will not be transferrable until transfers are enabled through governance.

## Genesis File

**WARNING: THIS IS NOT THE FINAL RECOMMENDATION FOR THE GENESIS FILE**

This repository contains a work-in-progress recommendation for the genesis file called [`penultimate_genesis.json`](./penultimate_genesis.json).
It **IS NOT** the final recommended genesis file.
If you find an error in this genesis file, please contact us
immediately at "injectiveprotocol.com".

To understand how this file was compiled, please see [GENESIS.md](GENESIS.md).

A final recommendation will be available shortly, including a justification for
all components of the genesis file and scripts to recompute it.

Anyone with an INJ allocation in the [`penultimate_genesis.json`](./penultimate_genesis.json) who intends to participate in the genesis ceremony must submit a pull request
containing a valid `gen-tx` to this repository in the `/gentx` folder with a file name like `<moniker>.json`.

## Instructions

Generally the steps to create a genesis validator are as follows:

1. [Install injectived](https://github.com/InjectiveLabs/injective-core/)

2. [Setup your validator keys]
```
injectived init injective-protocol
injectived keys add $VALIDATOR_KEY_NAME
```

3. Download the [genesis file](https://github.com/InjectiveLabs/mainnet-genesis) to `~/.injectived/config/genesis.json`

4. Sign a genesis transaction:

```bash
injectived gentx \
  <$VALIDATOR_KEY_NAME> \
  --amount <amount_of_delegation_inj> \
  --commission-rate <commission_rate> \
  --commission-max-rate <commission_max_rate> \
  --commission-max-change-rate <commission_max_change_rate> \
  --pubkey <consensus_pubkey> \
  --ip=<ip_address> \
  --moniker=<name> \
  --output-document=external-val.json 
```

Example
```bash
injectived gentx external-val-key-name  --chain-id=888 --amount 1000000000000000000000inj --ip=18.222.213.62 --output-document=external-val.json    --moniker="external-validator"     --commission-max-change-rate=0.01     --commission-max-rate=1.0     --commission-rate=0.07
```

This will produce a file in the ~/.injectived/config/gentx/ folder that has a name with the format `gentx-<node_id>.json`. The content of the file should have a structure as follows:

```json
{
  "type": "auth/StdTx",
  "value": {
    "msg": [
      {
        "type": "cosmos-sdk/MsgCreateValidator",
        "value": {
          "description": {
            "moniker": "<moniker>",
            "identity": "",
            "website": "",
            "details": ""
          },
          "commission": {
            "rate": "<commission_rate>",
            "max_rate": "<commission_max_rate>",
            "max_change_rate": "<commission_max_change_rate>"
          },
          "min_self_delegation": "1",
          "delegator_address": "inj1msz843gguwhqx804cdc97n22c4lllfkk39qlnc",
          "validator_address": "injvaloper1msz843gguwhqx804cdc97n22c4lllfkk5352lt",
          "pubkey": "<consensus_pubkey>",
          "value": {
            "denom": "inj",
            "amount": "100000000000"
          }
        }
      }
    ],
    "fee": {
      "amount": null,
      "gas": "200000"
    },
    "signatures": [
      {
        "pub_key": {
          "type": "tendermint/PubKeySecp256k1",
          "value": "AlT62zuYGlZGUG3Yv0RtIFoPTzVY4N+WEFmBvz1syjws"
        },
        "signature": ""
      }
    ],
    "memo": ""
  }
}
```

__**NOTE**__: If you would like to override the memo field use the `--ip` and `--node-id` flags for the `injectived gentx` command above.

Finally, to participate in this ceremony, copy this file to the `gentx` folder in this repo
and submit a pull request:

```
cp ~/.injectived/config/gentx/gentx-<node_id>.json ./gentx/<moniker>.json
```

We will only accept self delegation transactions up to 1000 inj for genesis.

## A Note about your Validator Signing Key

Your validator signing private key lives at `~/.injectived/config/priv_validator_key.json`. If this key is stolen, an attacker would be able to make
your validator double sign, causing a slash of 5% of your inj and the [tombstoning](https://github.com/cosmos/cosmos-sdk/blob/master/docs/spec/slashing/07_tombstone.md) of your validator. If you are interested in how to better protect this key please see the [`tendermint/kms`](https://github.com/tendermint/kms) (_*use at your own risk*_) repo. We will have a complete guide for how to secure this file soon after launch.

## Next Steps

Wait for the Injective Foundation to publish a final recommendation for the
Genesis Block Release Software and be ready to come online at the recommended
time.

The Injective foundation will recommend a particular genesis file and software version, but there
is no guarantee a network will ever start from it - nodes and validators may
never come online, the community may disregard the recommendation and choose
different genesis files, and/or they may modify the software in arbitrary ways. Such
outcomes and many more are outside the ICF's control and completely in the hands
of the community

On initialization of the software, the Injective chain Bonded Proof-of-Stake system will kick in to
determine the initial validator set (max 100 validators) from the set of `gentx` transactions.
More than 2/3 of the voting power of this set must be online and participating in consensus
in order to create the first block and start the Injective chain.

We expect and hope that INJ holders will exercise discretion in initial staking to ensure the network
does not ever become excessively centralized as we move steadily to the target of 66% INJ staked.

# Disclaimer


The Injective chain is *highly* experimental software. In these early days, we can
expect to have issues, updates, and bugs. The existing tools require advanced
technical skills and involve risks which are outside of the control of the
Injective Foundation. Any use of this open source Apache
2.0 licensed software is done at your *own risk and on a “AS IS” basis, without
warranties or conditions of any kind*, and any and all liability of the
Injective foundation for damages arising in
connection to the software is excluded. **Please exercise extreme caution!**

Furthermore, it must be noted that it remains in the community's discretion to adopt or not
to adopt the Genesis State that Injective foundation recommends within the Genesis Block
Software. Therefore, Injective foundation *cannot* guarantee that (i) INJ will be created and
(ii) the recommended allocation as set forth herein will actually take place.