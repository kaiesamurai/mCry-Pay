# mCry-Pay

## About Us
We are a team of four tech enthusiasts from France, increasingly passionate about the blockchain revolution, where we combined our skills and knowledge to create this innovative project. While some team members had prior experience with Ethereum development, others were new to it, but together we learned a lot throughout the process.

## Short Description
mCry-Pay is a trustless messaging bot that enables users to interact seamlessly with the Ethereum blockchain. The bot generates a unique smart-wallet for each user, allowing them to receive and spend Ethereum and ERC-20 tokens, as well as interact with Ethereum DeFi.

## Long Description
mCry-Pay is a trustless messaging bot designed for easy interaction with the Ethereum blockchain. Upon initiating a conversation with the mCry-Pay software, each user gets a smart-wallet created and deployed on the Ethereum blockchain. The smart-wallet allows users to:

- Receive ETH and ERC-20 tokens.
- Receive BTC through the pTokens protocol, converting it into wrapped Bitcoin (pBTC).
- Send ETH and DAI tokens.
- Lend DAI through the Aave protocol to earn interest.
- View their balance.
- View the transaction history of their smart-wallet.

For the hackathon, we built the mCry-Pay bot on Telegram due to its flexibility and widespread use within crypto communities. The user interface features a main menu workflow with message buttons for direct command execution, minimizing the need for manual input.

## How It's Made

### Tech Stack
- **Languages**: TypeScript, Solidity
- **Production Environment**: Firebase
- **CI**: GitHub Actions
- **API**: Blocknative
- **Libraries**: Buidler, ethers.js, Telegraf
- **DeFi**: Aave, pTokens

We opted for TypeScript to allow room for future improvements and potential contributions, despite the team's initial unfamiliarity with it. Solidity was chosen for smart contract development due to its prevalence in the Ethereum ecosystem. 

We utilized the Buidler starter kit provided at the hackathon and continued using it for future projects due to its positive impact. Ethers.js was integrated into the bot code for consistency with the smart contract stack and for its effective performance with TypeScript.

Firebase was selected for its serverless convenience and affordability. We upgraded to the Blaze plan to accommodate external queries required for notifications, using Cloud Functions and Cloud Firestore.

For the Telegram bot, we used Telegraf, the most active and well-documented JavaScript library for this purpose.

### Software Architecture
Initially, we aimed for a non-custodial smart wallet with encrypted message handling, but the lack of a standard URI scheme for calling smart contract methods led us to prioritize ease of use. We created a contract for each onboarding account, with a manager acting as a router, which unfortunately made the solution less non-custodial.

On the backend, there are two functions:
- One for the Telegram webhook.
- One for the Blocknative webhook.

Blocknative was chosen for transaction notifications due to its effectiveness and active development. We store data in Cloud Firestore for transaction tracking and user-wallet mapping. The user is identified with a UID stored in the smart wallet manager, facilitating future expansions to other bots.

### Security Considerations
The main vulnerabilities are in the serverless functions and the database. To prevent fake requests, we hide our webhook behind a secret path and regularly update it, verifying the origin of each request. Proper configuration of the Cloud Firestore database is crucial to avoid accidental public exposure of data.

### Smart-contract Architecture
The project uses a SmartWalletManager smart-contract, managed by the Ethereum private key of the Telegram bot, which acts as a smart-wallet factory. When a new user joins, the bot calls the `createWallet(UID)` function on the SmartWalletManager to deploy a new UserSmartWallet smart-contract, registered with the user's UID.

The SmartWalletManager maintains a mapping of all UIDs to their smart-wallet addresses. To simplify event listening, the Manager contract serves as the central event emitter. Blocknative notifies our serverless project via webhook when an event is detected, allowing the bot to notify the user.

The UserSmartWallet integrates multiple interfaces from other smart-contracts, including:
- ERC-20 for receiving and spending tokens.
- Aave's aToken, LendingPool, and LendingPoolAddressesProvider interfaces for DAI lending.
- pTokens interface for converting pBTC to BTC.

The smart-wallet also manages a list of owners for interaction, with the Telegram bot granted default owner access. Users can add external Ethereum addresses or remove the bot's access for full control.

## Next Steps
We plan to optimize the smart-wallet contracts to reduce gas consumption and enhance viability on the Ethereum main-net.

## Prizes Applicability
- pTokens
- Aave