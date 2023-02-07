---
layout: ../../layouts/MainLayout.astro
section: automation
date: Last Modified
title: "Chainlink Automation Documentation"
whatsnext:
  {
    "Register a time-based Upkeep": "/chainlink-automation/job-scheduler/",
    "Register a Custom Logic Upkeep": "/chainlink-automation/register-upkeep/",
    "Create an compatible contract for custom logic upkeep": "/chainlink-automation/compatible-contracts/",
    "Automation architecture": "/chainlink-automation/overview/",
    "Automation economics": "/chainlink-automation/automation-economics/",
  }
setup: |
  import { Aside } from "@components"
---

<Aside type="note" title="Talk to an expert">
  <a href="https://chainlinkcommunity.typeform.com/to/OYQO67EF?page=docs-automation">Contact us</a> to talk to an expert about using Chainlink Automation with your applications.
</Aside>

**Chainlink Automation** enables conditional execution of your smart contracts functions through a hyper-reliable and decentralized automation platform that uses the same external network of node operators that secures billions in value. Building on Chainlink Automation will help you get to market faster so you don't have to deal with the setup cost, ongoing maintenance, and risks associated with a centralized automation stack. To take full advantage of the Chainlink Automation infrastructure, read all of the documentation to understand the features of Chainlink Automation.

<YouTube id="https://www.youtube.com/watch?v=dj0impNJdls" />

To learn more about how the Chainlink Automation Network automates your smart contracts, read the [Chainlink Automation Architecture](/chainlink-automation/overview) page. You can also learn more through our [additional Automation resources](https://chain.link/automation#masterclass).

![Chainlink Automation](/images/contract-devs/automation/automation_intro.gif)

## Select a trigger

Chainlink Automation will reliably execute smart contract functions using a variety of triggers.

- [Time-based trigger](#time-based-trigger): Use a [time based trigger](#time-based-trigger) to execute your function according to a time schedule. This feature is also called the Job Scheduler and it is a throwback to the Ethereum Alarm Clock. Time-based trigger contracts do not need to be [compatible](/chainlink-automation/compatible-contracts/#example-contract) with the `AutomationCompatibleInterface` contract.
- [Custom logic trigger](#custom-logic-trigger): Use a [custom logic trigger](#custom-logic-trigger) to provide custom solidity logic that Automation Nodes evaluate (off-chain) to determine when to execute your function on-chain. Your contract must meet the requirements to be [compatible](/chainlink-automation/compatible-contracts/) with the `AutomationCompatibleInterface` contract. Custom logic examples include checking the balance on a contract, only executing limit orders when their levels are met, any one of our [coded examples](/chainlink-automation/util-overview), and many more.

### Time-based Trigger

Before you begin, deploy the contract that you want to automate. You will also need the ABI for your contract if it has not been verified. Your contract does not need to be [compatible](/chainlink-automation/compatible-contracts/) with the `AutomationCompatibleInterface` contract.

1. Open the Chainlink Automation app.

   <!-- prettier-ignore -->
   <div class="remix-callout">
      <a href="https://automation.chain.link" >Open the Chainlink Automation App</a>
   </div>

1. [Register](/chainlink-automation/job-scheduler/) a new Upkeep in the [Chainlink Automation App](https://automation.chain.link) and select **Time-based** trigger. Provide the address of your deployed contract, provide the ABI if it is not verified, and choose the function that you want to automate along with the relevant function inputs, if any.

1. Specify the time schedule using [CRON](/chainlink-automation/job-scheduler/#specifying-the-time-schedule).

1. Complete the remaining details. Your upkeep name will be publicly visible, but your email and project name will not be publicly visible. Your gas limit needs to include an extra [150K](/chainlink-automation/job-scheduler/#entering-upkeep-details) for execution.

1. Fund your Upkeep with ERC-677 LINK. See the [LINK token contracts](/resources/link-token-contracts/) page to determine where to acquire ERC-677 LINK.

1. After your Upkeep is registered, [manage](/chainlink-automation/manage-upkeeps/) it in the Chainlink Automation App.

For more details on time-based automation, click [here](/chainlink-automation/job-scheduler/).

### Custom logic trigger

To use a custom logic trigger, you must make your contract [compatible](/chainlink-automation/compatible-contracts/) with the `AutomationCompatibleInterface` contract.

1. Open the Chainlink Automation app.

    <!-- prettier-ignore -->
   <div class="remix-callout">
      <a href="https://automation.chain.link" >Open the Chainlink Automation App</a>
   </div>

1. [Register](/chainlink-automation/register-upkeep/) a new Upkeep in the [Chainlink Automation App](https://automation.chain.link) and select **Custom logic** trigger. Provide the address of your [compatible](/chainlink-automation/compatible-contracts/) contract and complete the remaining details. Your upkeep name will be publicly visible, but your email and project name will **not** be publicly visible. Ensure you specify the appropriate gas limit for your function to execute on chain.

1. Fund your Upkeep with ERC-677 LINK. See the [LINK token contracts](/resources/link-token-contracts/) page to determine where to acquire ERC-677 LINK.

1. After your Upkeep is registered, [manage](/chainlink-automation/manage-upkeeps/) it in the Chainlink Automation App.

1. For more details on custom logic automation, read the [Creating Compatible Contracts](/chainlink-automation/compatible-contracts/) guide.

:::caution[WARNING]
Do **NOT** attempt to send LINK to your contract. For Chainlink Automation, contracts are funded through the registry rather than within your contract.
:::

:::tip[Funding Upkeep]
Fund your Upkeep with more LINK than you anticipate you will need. The network will not check or perform your upkeep if your balance is too low based on current exchange rates. View the [Automation Economics](/chainlink-automation/automation-economics) page to learn more about the cost of using Chainlink Automation.
:::

:::tip[ERC677 Link]
For registration you must use ERC-677 LINK. Read our [LINK](/resources/link-token-contracts/) page to determine where to acquire mainnet LINK, or visit our [faucets.chain.link](https://faucets.chain.link/) for testnet LINK.
:::

## Supported networks and costs

For a list of blockchains that is supported by Chainlink Automation, see the [Supported Networks](/chainlink-automation/supported-networks) page. To learn more about the cost of using Chainlink Automation, see the [Automation Economics](/chainlink-automation/automation-economics) page.

## Questions and examples

If you have questions, read the [Chainlink Automation Frequently Asked Questions](/chainlink-automation/faqs/) page, ask them in the [#automation channel](https://discord.com/channels/592041321326182401/821350860302581771) in our [Discord server](https://discord.gg/qj9qarT), or [reach out to us](https://forms.gle/WadxnzzjHPtta5Zd9).

To understand different use cases for Chainlink Automation, refer to [Other Tutorials](/getting-started/other-tutorials/).
