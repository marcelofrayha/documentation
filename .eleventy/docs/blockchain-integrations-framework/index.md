---
layout: nodes.liquid
section: bif
date: Last Modified
title: 'Blockchain Integrations Framework'
permalink: 'docs/blockchain-integrations-framework/'
---

Chainlink Labs provides self-service tools that node operators and blockchain projects can use to deploy Data Feed integrations on EVM networks. These tools are called the Blockchain Integrations Framework (BIF). This framework includes a command line interface (CLI) and is available as a binary file. The binary is not publicly available. During the Alpha release, you must receive this binary directly from Chainlink Labs.

The framework simplifies the following tasks:

- Deploy Chainlink nodes into Kubernetes clusters and distribute jobs to them
- Run tests that generate reports detailing compatibility between Chainlink Labs' EVM integration and a given blockchain
- Deploy cluster infrastructure required to test OCR for Data Feeds
- Perform longer running soak tests for OCR for Data Feeds
- Perform smart contract management tasks with the same capabilities as Chainlink Labs' tools

**Contents**

- [Requirements](#requirements)
- [Set up your environment](#set-up-your-environment)
- [Running basic tests](#running-basic-tests)
- [Running integration testing](#running-integration-tests)
- [Troubleshooting integration tests](#troubleshooting-integration-tests)

## Requirements

Most basic tests can be completed on a system with minimal hardware resources. To complete soak testing and use the CLI to create clusters with multiple Chainlink nodes, you must run a Kubernetes cluster on a system with Docker and the following system resources:

- A Linux system
- 6 CPU cores
- 10 GiB of system memory
- At least 50 GiB of disk storage

Additionally, Docker's resource limits must also allow these resources. If you are using Docker Desktop, set your [Docker resource limits](https://docs.docker.com/desktop/settings/linux/#resources) to make sure they meet the system requirements for using the Blockchain Integrations Framework. Docker Engine has [resource limits](https://docs.docker.com/config/containers/resource_constraints/) that might be different from Docker Desktop.

## Set up your environment

Install the framework and create an alias:

1. Obtain the binary file from Chainlink Labs and put it in your local directory.
1. Make the binary available in `/usr/bin` so you can run it without the full path: `sudo mv ./bif /usr/local/bin/`

You can now run the framework CLI. Run `bif integration -h` to see the available commands. See the [Running basic tests](#running-basic-tests) section for examples.

If you plan to run soak tests or other tests that require running Chainlink nodes, set up [Helm](https://helm.sh/docs/intro/install/#through-package-managers), [Kubernetes](https://kubernetes.io/docs/setup/) and other required tools:

1. Install the [Docker Engine](https://docs.docker.com/engine/install/) or [Docker Desktop](https://www.docker.com/products/docker-desktop/).
1. Set your [Docker resource limits](https://docs.docker.com/desktop/settings/linux/#resources) to make sure they meet the [system requirements](#requirements) for using the Blockchain Integrations Framework. Docker Engine has [resource limits](https://docs.docker.com/config/containers/resource_constraints/) that might be different from Docker Desktop.
1. Install [Helm](https://helm.sh/docs/intro/install/#through-package-managers).
1. Add the following repositories:
   - chainlink-qa: `helm repo add chainlink-qa https://raw.githubusercontent.com/smartcontractkit/qa-charts/gh-pages/`
   - bitnami: `helm repo add bitnami https://charts.bitnami.com/bitnami`
1. Update the helm repository: `helm repo update`
1. Install the [kubectl tool](https://kubernetes.io/docs/tasks/tools/).

The [chainlink-env repository](https://github.com/smartcontractkit/chainlink-env) includes several tools for creating clusters of Chainlink nodes for testing. Use this to start a cluster:

1. Clone the [chainlink-env repository](https://github.com/smartcontractkit/chainlink-env) and change directories: `git clone https://github.com/smartcontractkit/chainlink-env.git && cd chainlink-env`
1. Install dependencies: `make install_deps`
1. Install k3d from [k3d.io](https://k3d.io/v5.4.6/#installation).
1. _Optionally_, install `Lens` from [k8slens.dev](https://k8slens.dev/) or use `k9s` as a low resource consumption alternative from [k9scli.io](https://k9scli.io/topics/install/). These tools offer user-friendly cli to interact with kubernetes clusters. They are not necessary for this tutorial.
1. Create a cluster: `make create_cluster`
1. Install monitoring: `make install_monitoring`

The `make install_monitoring` command starts Grafana, which takes control of your terminal. Open Grafana in a browser at `localhost:3000` to confirm that it is running properly. Log in with `admin` as the username and `sdkfh26!@bHasdZ2` as the default password. If you are running testing a remote system, you can use `ssh -i $KEY $USER@$REMOTE-IP -L 3000:localhost:3000 -N` to create an SSH tunnel to the system where Grafana is running and map port `3000` to your local workstation. Change the default Grafana password.

You can now [run integration testing](#running-integration-tests).

When you are done with testing, you can clean up the environment using the following commands:

1. Stop the cluster: `make stop_cluster`
1. Delete the cluster: `make delete_cluster`

## Running basic tests

You can use the framework to run several basic tests on a network. These commands deploy contracts as part of the tests, so you must provide the following parameters:

- The private key to a wallet that is funded and capable of deploying contracts on the network that you want to test
- The chain ID for your network, which usually can be found on the [LINK Token Contracts](/docs/link-token-contracts/) or [chainlist.org](https://chainlist.org/)
- The RPC URL for either a network node that you run or a third-party service like [Alchemy](https://www.alchemy.com/), [Infura](https://infura.io/), and [Ankr](https://www.ankr.com/rpc/). **Note**: Use a websocket URL. Subscriptions require websockets.

### Test compatibility of the network client API

```shell
bif compatibility test-rpc \
--privateKey <PRIVATE_KEY> \
--chainID <CHAIN_ID> \
--rpc <RPC_URL> \
--storageContractAddress <OPTIONAL_CONTRACT_ADDRESS>
```

The `--storageContractAddress` flag is available so you can re-use deployed contracts from previous tests and save wallet funds.

After running the test, you should get a similar output:

```shell
Running Compatibility Tests...
Deploying Storage test contract...
Successfully deployed contract to address 0x7d626A527a639Cb0b7e0A29CC8539E210063f77A

API Test Report:

eth_call                                      - chainlink-compatible ✅
eth_estimateGas                               - chainlink-compatible ✅
eth_estimateGas (with block number)           - chainlink-compatible ✅
eth_getCode                                   - chainlink-compatible ✅
eth_getTransactionReceipt                     - chainlink-compatible ✅
eth_getBlockByHash                            - chainlink-compatible ✅
eth_getBlockByNumber                          - chainlink-compatible ✅
eth_getTransactionCount                       - chainlink-compatible ✅
eth_getBalance                                - chainlink-compatible ✅
eth_maxPriorityFeePerGas                      - chainlink-compatible ✅
eth_gasPrice                                  - chainlink-compatible ✅
eth_sendRawTransaction                        - chainlink-compatible ✅
eth_subscribe                                 - chainlink-compatible ✅
eth_getLogs                                   - chainlink-compatible ✅
InsufficientEth                               - chainlink-compatible ✅
NonceTooLow                                   - chainlink-compatible ✅
TransactionAlreadyInMempool                   - chainlink-compatible ✅

Tests Ran Successfully.
```

### Test the smart contract op codes of a network

```shell
bif compatibility test-opcodes \
--privateKey <PRIVATE_KEY> \
--chainID <CHAIN_ID> \
--rpc <RPC_URL> \
--opcodesContractAddress <OPTIONAL_CONTRACT_ADDRESS>
```

The `--opcodesContractAddress` flag is available so you can re-use deployed contracts from previous tests and save wallet funds.

After running the test, you should get a similar output:

```shell
Deploying Opcodes contract...
Successfully deployed contract to address 0xABc3B3cEC3978d831e5585fc6cBCc11B5fEDA2fc
Running Opcode Tests on contract 0xABc3B3cEC3978d831e5585fc6cBCc11B5fEDA2fc...

OpCodes Test Report:

GeneralCodes                                  - chainlink-compatible ✅
Invalid                                       - chainlink-compatible ✅
Stop                                          - chainlink-compatible ✅
Revert                                        - chainlink-compatible ✅

Tests Ran Successfully.
```

## Running integration tests

Integration testing uses the `bif integration test` command and a TOML file. The TOML file simplifies managing the many variables involved.

The TOML file for the Blockchain Integrations Framework has two settings sections for OCR soak tests on EVM networks:

- _Nodes settings_: Variables that configure chainlink nodes for given networks.
- _Network settings_: Variables that configure the testing client : Which chain you use for testing and the private key for the wallet you want to use for deploying contracts.
- _Test settings_: Test duration, node funding, and round configuration settings to control the behavior of the test.

**Example TOML file**

```toml
# node settings
[nodes]
    [nodes.goerli]

        [nodes.goerli.base]
        eth_chain_id = "5"
        eth_http_url = "https://eth-goerli.g.alchemy.com/v2/<ALCHEMY_KEY>"
        eth_url = "wss://eth-goerli.g.alchemy.com/v2/<ALCHEMY_KEY>"
# network settings
[networks]

    [networks.evm]

        [networks.evm.base]
        name = "EVM"
        evm_keys = ["WALLET_PRIVATE_KEY"]
        ws_urls = []
        http_urls = []
        chain_id = 1337
        simulated = false
        chainlink_transaction_limit = 5000
        transaction_timeout = "2m"
        minimum_confirmations = 1
        gas_estimation_buffer = 1000

        [networks.evm.overrides.goerli]
        name = "Goerli"
        ws_urls = ["wss://eth-goerli.g.alchemy.com/v2/<ALCHEMY_KEY>"]
        chain_id = 5

        [networks.evm.overrides.optimism_goerli]
        name = "Optimism Goerli"
        ws_urls = ["wss://opt-goerli.g.alchemy.com/v2/<ALCHEMY_KEY>"]
        chain_id = 420
        client_implementation = "Optimism"

        [networks.evm.overrides.palm]
        name = "Palm"
        ws_urls = ["wss://palm"]
        chain_id = 11297108099

# test settings
[tests]

    [tests.v1-9-0]

        [tests.v1-9-0.base]
        keep_environments="Never" # Always | OnFail | Never
        # Image repo to pull the Chainlink image from
        chainlink_image="public.ecr.aws/chainlink/chainlink"
        # Version of the Chainlink image to pull
        chainlink_version="1.9.0"
        # Name of the person running the tests (no spaces)
        chainlink_env_user="My-Name"
        test_log_level="info" # info | debug | trace
        node_count=6
        test_duration=15 # minutes
        contract_count=2
        node_funding=0.02 # ETH
        round_timeout=15 # minutes
        expected_round_time=2 # minutes
        time_between_rounds=1 # minutes

        [tests.v1-9-0.overrides.shorter-test]
        test_duration=2 # minutes
```

If you completed the full [steps to set up your environment](#set-up-your-environment), you can run a soak test using the following steps:

1. Create a file called `example.toml` and paste the example TOML file above.
1. Edit the file to add your `evm_keys` under the `networks.evm.base` config.
1. Modify the `ws_urls` under `networks.evm.overrides.goerli` with your specified RPC endpoint URL. Optionally, you can create additional overrides for different networks.
1. Start running a soak test `bif integration test ./example.toml`.
1. The interactive menu opens. Select your desired:
   1. Test suite: Only `OCR` is available for the moment.
   1. Test: Only `EVMSoak` is available for the moment.
   1. Network settings (see your `TOML` config): `evm` .
   1. Network override (see your `TOML` config): `goerli`.
   1. Test settings (see your `TOML` config): v1-9-0.
   1. Test override (see your `TOML` config): shorter-test.
   1. The cluster to run on.
1. While your test is running, view your local Grafana instance at `localhost:3000` to see test metrics.
1. Check node logs: `bif integration logs [--level LOG_LEVEL]`

If the test is successful, you will see output similar to the following example:

```
3:56AM INF Soak test concluded
------------------------------
• [SLOW TEST] [434.148 seconds]
OCR Soak Test @ocr-soak With soak test contracts deployed runs the soak test until error or timeout
------------------------------

Ran 1 of 1 Specs in 434.148 seconds
SUCCESS! -- 1 Passed | 0 Failed | 0 Pending | 0 Skipped
```

As you test additional networks, modify the TOML file and configure it with the details for your network.

### Troubleshooting integration tests

There are some common issues with soak testing that you can troubleshoot.

**Pods remaining from failed tests:**

If a soak test fails, the pods used for testing often remain running. Use `kubectl` to identify and delete these pods:

1. List all pods by their namespaces: `kubectl get pods --all-namespaces`
1. Delete the deployments that you no longer need by namespace: `kubectl delete --all deployments --namespace=ocr-soak-goerli-50b0c`
1. Modify your test configuration and run `bif integration soak ./example.toml` again.

**Insufficient resources:**

Your system must meet the defined [Requirements](#requirements). Running on systems without sufficient resources will cause soak tests to fail because some containers for Chainlink nodes will not be able to start.

- If you are running on a virtual machine, add additional vCPUs, system memory, or storage.
- Run your soak tests with fewer nodes. The default node count is 6 per cluster, which is recommended for a thorough soak test. You can adjust this value if you are unable to obtain the necessary system resources.

**Insufficient wallet funding**

The soak tests send 0.1 ETH to each test node by default, so you must make sure that there are significant funds in the wallet that you use to fund testing. This is the wallet specified by your `evm_keys` parameter in the TOML file. Alternatively, you can adjust the `node_funding` parameter in the TOML test configuration, but make sure that each node has enough funds to complete the soak test.
