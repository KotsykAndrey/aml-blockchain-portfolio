# Part 1: Bybit Hack 2025 by Lazarus Group

Type: Blockchain Forensics / DPRK Attribution
 
Date of hack: February 21 2025
 
Amount Stolen: ~$1.5 billion USD (in ETH)
 
Attributed To: Lazarus Group (North Korea, state-sponsored hackers)
 
Sources: Chainalysis, TRM Labs, Etherscan, on-chain data.
 
---
 
## What happened?
In February 2025 is that hackers from North Korea stole 1.5 billion dollars from the crypto exchange Bybit.
This is the biggest crypto theft in history.
 
The Bybit hack was carried out by the Lazarus Group, a group of hackers from North Korea.
They did not hack Bybit directly. Instead they hacked a third-party software called Safe{Wallet} that Bybit was using to manage their cold wallet.
The hackers changed the transaction data in the interface.
When Bybit employees signed the transaction they thought it was a normal transfer but actually they signed a transaction that gave full control of the wallet to the hackers.
After the theft the money was quickly converted, chain hopped and split through thousands of wallets to confuse the trace.
The Bybit hack is an example of how the Lazarus Group operates.
 
---
 
## How the attack worked? 

Step by step:

**1) The hackers compromised Safe{Wallet}.**
They got access to a developers computer at Safe{Wallet}. Injected malicious code into the interface.
 
**2) The transaction data was replaced.**
When Bybit employees opened the wallet to sign a transaction the malicious code showed them fake data. Everything looked legitimate.
 
**3) The Bybit employees signed.**
All required signatures were collected. From the blockchains perspective it was an authorized transaction.
No AML alert would fire at this moment. The theft looked like a normal transfer.
 
**4) $1.5 Billion moved to hacker wallets.**
The primary receiving address was `0x1db92e2eebc8e0c075a02bea49a2935bcd2dfcf4`.
The Lazarus Group used this address to receive the stolen funds.
 
---
 
## How they laundered the money?

(proofs you will see in part 2 on-chain analysis)
 
**Phase 1) Split through thousands of wallets.**
Automated scripts split the funds across thousands of wallets.
Each wallet held an amount creating a huge transaction graph that is very hard to analyze manually.
The hackers split the funds into smaller amounts making it difficult to track.
One stolen wallet split into hundreds of wallets then each split into thousands wallets into smaller and smaller amounts and after it cash out points.

**Phase 2) Conversion.**
After the theft the hackers started converting ETH to other assets through DEX exchanges to prevent freezing.
They stole 1.5 billion dollars in ETH. Swapped it via DEX, such as Uniswap and Paraswap and converted it to BTC, USDT, DAI and much of no-name tokens.
The reason they did this fast is that Bybit could ask exchanges to freeze specific ETH addresses.
Once converted through DEX the trail splits across assets and chains.
 
**Phase 3) Chain hopping.**
The hackers used the THORChain bridge to move ETH to BTC.
Then used TornadoCash, BTC mixers and P2P exchanges to further launder the funds.
They also used XMR, a privacy coin to break traceability and then moved the funds back to BTC via OTC brokers.
 
**Phase 4) Cash out.**
The hackers used OTC brokers in China and Southeast Asia where there is no KYC to cash out the funds.
They also used Huione Guarantee, a marketplace in Southeast Asia and Russian instant exchangers, which do not have KYC or AML controls.
Additionally Korean IT workers who work at Western companies and receive crypto salaries were also used to cash out the funds.
 
---

## How the industry responded?
 
How the industry responded is an example of why public-private cooperation in AML matters.
Several exchanges received real-time alerts from Chainalysis and TRM Labs when the Lazarus funds arrived.
Around $400 millions was frozen in the weeks after the hack.
This would not have been possible without blockchain intelligence tools.
 
---
 
## Red Flags
 
If you were an AML analyst at an exchange and these funds arrived, you would see:
 
| Red Flag | Risk Level | Action |
|---|---|---|
| Funds from OFAC sanctioned address | 🔴 CRITICAL | Immediate freeze + SAR |
| Large inflow right after known hack | 🔴 HIGH | Immediate escalation |
| Cross-chain bridge origin | 🟡 MEDIUM | Enhanced review |
| New wallet with no prior history | 🟡 MEDIUM | Source of funds request |
| Many rapid small splits | 🔴 HIGH | SAR + escalation |
| Destination, high-risk OTC broker | 🔴 HIGH | Freeze + SAR |
| Fake token airdrop, Secondary scam | ⚪️ LOW | Don't sign any scam contracts |
 
---
