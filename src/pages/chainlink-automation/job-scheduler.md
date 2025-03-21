---
layout: ../../layouts/MainLayout.astro
section: ethereum
date: Last Modified
title: "Chainlink Automation Job Scheduler"
whatsnext: { "Register a Custom Logic Upkeep": "/chainlink-automation/register-upkeep/" }
---

This guide explains how to register a time-based Upkeep that executes according to a time schedule that you provide.

![Job Scheduler animation](/images/contract-devs/automation/auto-job-scheduler.gif)

## Register a new Upkeep

To use the job scheduler, you must register a new upkeep on the Automation network. In the [Chainlink Automation App](https://automation.chain.link/), click the blue **Register new Upkeep** button.

![Chainlink Automation App](/images/contract-devs/automation/auto-ui-home.png)

### Connecting your Wallet

If you do not already have a wallet connected with the Chainlink Automation network, the interface will prompt you to do so. Click the **Connect Wallet** button and follow the remaining prompts to connect your wallet to the network.

![Automation Connect Wallet](/images/contract-devs/automation/auto-ui-wallet.png)

## Trigger Selection

After you have successfully connected your wallet, please select time-based trigger.

![Automation Trigger Selection](/images/contract-devs/automation/auto-ui-pick.png)

## Using Time-Based Triggers

When you select the time-based trigger, you are prompted to enter a _contract address_. Provide the address of the contract you want to execute. If you did not verify the contract on chain, you will need to paste the [Application Binary Interface](https://docs.soliditylang.org/en/develop/abi-spec.html) (ABI) of the deployed contract into the corresponding text box. Select the function name that you want to execute and provide any static inputs. If you want to use dynamic inputs please see [Custom logic Upkeeps](/chainlink-automation/register-upkeep/)

![Automation Time Based Trigger](/images/contract-devs/automation/automation-time-based-trigger.png)

### Specifying the Time Schedule

After you successfully entered your contract address and ABI, specify your time schedule in the form of a CRON expression. CRON expressions are a shorthand way to create a time schedule. Use the provided example buttons to experiment with different schedules and then create your own.

```
Cron jobs are interpreted according to this format:

  ┌───────────── minute (0 - 59)
  │ ┌───────────── hour (0 - 23)
  │ │ ┌───────────── day of the month (1 - 31)
  │ │ │ ┌───────────── month (1 - 12)
  │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday)
  │ │ │ │ │
  │ │ │ │ │
  │ │ │ │ │
  * * * * *

All times are in UTC

- can be used for range e.g. "0 8-16 * * *"
/ can be used for interval e.g. "0 */2 * * *"
, can be used for list e.g. "0 17 * * 0,2,4"

  Special limitations:
    * there is no year field
    * no special characters: ? L W #
    * lists can have a max length of 26
    * no words like JAN / FEB or MON / TUES
```

After you enter your CRON expression, click **Next**.

![Automation Cron Expression](/images/contract-devs/automation/automation-cron-expression.png)

### Entering Upkeep Details

To complete the upkeep registration process, you must enter some information about your upkeep including its name, gas limit, starting balance LINK, and contact information.

:::note[Job Scheduler Gas requirements]
When you create an upkeep through the Job Scheduler, Chainlink Automation deploys a new `CronUpkeep` contract from the [CronUpkeepFactory](https://github.com/smartcontractkit/chainlink/blob/develop/contracts/src/v0.8/factories/CronUpkeepFactory.sol) to manage your time schedule and ensure that it is compatible. This contract uses roughly 110K gas per call, so it is recommended to add 150K additional gas to the gas limit of the function you are automating.
:::

![Automation Upkeep Details](/images/contract-devs/automation/automation-upkeep-details.png)

:::tip[ERC677 Link]
For registration you must use ERC-677 LINK. Read our [LINK](/resources/link-token-contracts/) page to determine where to acquire mainnet LINK, or visit our [faucets.chain.link](https://faucets.chain.link/) for testnet LINK.
:::
