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

Genesis transactions will be collected on Github in this repository and checked for validity.
Genesis file collection will terminate on 12 June 2021 23:00 GMT. The final recommended genesis file will be published shortly after that time.

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

Anyone who intends to participate in the genesis ceremony must submit a pull request
containing a valid `gen-tx` to this repository in the `/gentx` folder with a file name like `<moniker>.json`.
and transfer their staked INJ to Peggy contract address from same validator Eth address.

## Instructions

Generally the steps to create a genesis validator are as follows:

1. Install injectived from source
    ```
    git clone https://github.com/InjectiveLabs/injective-core/
    cd injective-core
    make install
    ```

2. [Setup your validator keys]
    ```
    injectived init injective-protocol --chain-id injective-1
    injectived keys add $VALIDATOR_KEY_NAME
    ```

3. Download the [penultimate genesis json file](https://github.com/InjectiveLabs/mainnet-genesis) to `~/.injectived/config/genesis.json`

4. Add initial balance. We will only accept self delegation transactions up to 1000 inj for genesis.
    ```bash
    injectived add-genesis-account $VALIDATOR_KEY_NAME <amount_of_inj>
    
    where amount_of_inj = amount_of_delegation_inj + extra_inj_balance(useful to pay for tx fee) 

    Example (add 1000 inj)
    injectived add-genesis-account $VALIDATOR_KEY_NAME 1000000000000000000000inj
    ```

5. Sign a genesis transaction:
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

    Example `genTx` command staking 1000inj
    ```bash
    injectived gentx external-val-key-name   1000000000000000000000inj  --chain-id="injective-1"  --ip=18.222.213.61 --output-document=external-val.json    --moniker="external-validator"  --commission-max-change-rate=0.01     --commission-max-rate=1.0     --commission-rate=0.07
    ```

    This will produce a file `external-val.json` in current folder.  The content of the file should have a structure as follows:

    ```json
    {
      "body" : {
        "messages" : [
          {
            "@type" : "/cosmos.staking.v1beta1.MsgCreateValidator",
            "description" : {
              "moniker" : "external-validator",
              "identity" : "",
              "website" : "",
              "security_contact" : "",
              "details" : ""
            },
            "commission" : {
              "rate" : "0.070000000000000000",
              "max_rate" : "1.000000000000000000",
              "max_change_rate" : "0.010000000000000000"
            },
            "min_self_delegation" : "1",
            "delegator_address" : "inj1793zghz5hu4mu6tm72e0gjfedv28my2d5dq0mx",
            "validator_address" : "injvaloper1793zghz5hu4mu6tm72e0gjfedv28my2drt426z",
            "pubkey" : {
              "@type" : "/cosmos.crypto.ed25519.PubKey",
              "key" : "YL2kLcyq+bRe760dfIeSW/4pxvs3myyfKq/wu5r/iVE="
            },
            "value" : {
              "denom" : "inj",
              "amount" : "1000000000000000000000"
            }
          }
        ],
        "memo" : "4e306f75d61b80d73f58e175821a10c585a6bafe@18.222.213.62:26656",
        "timeout_height" : "0",
        "extension_options" : [],
        "non_critical_extension_options" : []
      },
      "auth_info" : {
        "signer_infos" : [
          {
            "public_key" : {
              "@type" : "/injective.crypto.v1beta1.ethsecp256k1.PubKey",
              "key" : "AhT1p+NFxZIzMdd4mTNyndw1wo8UcAQGw8/uhwSU2Nsa"
            },
            "mode_info" : {
              "single" : {
                "mode" : "SIGN_MODE_DIRECT"
              }
            },
            "sequence" : "0"
          }
        ],
        "fee" : {
          "amount" : [],
          "gas_limit" : "200000",
          "payer" : "",
          "granter" : ""
        }
      },
      "signatures" : [
        "82LBR+XkC+2fXT6nWL2CCSz/v8nHYTf0S2+gylpggEhFNSPknYmi67xx2cjvOPrp3Me194PX5EnbX36naiGncwA="
      ]
    }
    ```

    __**NOTE**__: If you would like to override the memo field use the `--ip` and `--node-id` flags for the `injectived gentx` command above.

6. copy this file to the `gentx` folder in this repo and submit a pull request:
    ```
    cp external-val.json ./gentx/<moniker>.json
    ```
7. Finally, to participate in this ceremony, transfer your INJ genesis stake amount to Peggy proxy contract address and send a message with Ethereum tx hash in discord.


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
outcomes and many more are outside the Injective foundation and completely in the hands
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
