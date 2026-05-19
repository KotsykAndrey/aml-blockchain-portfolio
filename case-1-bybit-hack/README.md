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

## Key takeaways from the Bybit hack
 
Supply chain attacks are hard to detect as the theft looked like a normal authorized transaction.
Standard KYC/AML controls do not help in these cases.
 
Speed matters, as the Lazarus Group moved funds within minutes and real-time blockchain monitoring is essential.
 
Blockchain intelligence tools work as $400 million was frozen because Chainalysis and TRM pushed real-time alerts to exchanges.
 
DEX is a regulatory gap. DEX swaps don't trigger Travel Rule. This is a big problem for AML compliance right now.
 
DPRK crypto funds a nuclear program, this is not just a compliance issue, it is a national security issue.
Any DPRK-attributed transaction requires immediate escalation.
 
---

# Part 2: Real on-chain investigation
On-Chain Analysis: Bybit Hack Transaction Tracing
 
Type: Hands-on Blockchain Analysis
 
Tool Used: Etherscan.io (free, public data)
 
Blockchain: Ethereum (ETH)
 
Address Analyzed: `0x1db92e2eebc8e0c075a02bea49a2935bcd2dfcf4` (Bybit Cold Wallet 1)
 
Date of Analysis: May 2026
 
---
 
## 1) What I Did?
 
I opened the real Bybit hack address on Etherscan and manually traced the transactions to
understand how the theft happened and how the money was moved after.
All data below is from public on-chain records. No paid tools were used only Etherscan.io
 
![Bybit Cold Wallet 1 on Etherscan](images/screenshot-1.png)
 
---
 
## 2) What I Found on Etherscan?
 
### Timelines:
 
**~470-500 days ago, Normal Operations**
 
Bybit Cold Wallet 1 received several large transfers:
- Multiple transactions of ~30,000 ETH from other Bybit wallets
- One transfer of ~60,000 ETH to Bybit Hot Wallet
This looks like normal exchange treasury management and moving funds between cold and hot wallets.
 
![Bybit Hot and Cold Wallets normal treasury](images/screenshot-2.png)
 
---
 
**~455 days ago, Staked Assets Moved In**
 
Bybit Hot Wallet sent staked assets to Cold Wallet 1:
- 10,000 stETH
- 15,000 cmETH
 
These are liquid staking tokens (ETH that is staked and earning yield). At this point the
wallet held a large amount of different ETH-based assets, normal treasury.
 
![Bybit Hot and Cold Wallets normal treasury](images/screenshot-3.png)
 
---
 
**~450-460 days ago, Preparation**
 
4 transactions from Hot Wallet to Cold Wallet 1:
- 20,000–25,000 ETH per transaction
Normal treasury consolidation, nothing suspicious at this stage.
 
![Bybit Hot and Cold Wallets normal treasury](images/screenshot-4.png)
 
---

**21.02.2025 14:16:11 UTC ~451 days ago, THE HACK**
 
This is the moment everything changed.
 
Cold Wallet 1 sent to Bybit Exploiter 1:
- 401,346 ETH in one transaction, staked assets and usdt also transferred later
- 15,000 cmETH
- 90,375 stETH
- 8,000 mETH
- 90 USDT

![401000 eth exploit](images/screenshot-5.png)
![stETH mETH cmETH usdt expliot](images/screenshot-6.png)
 
At the same time immediate DEX swaps via Uniswap Paraswap and others to convert staked
tokens to regular ETH and other assets.
 
![immediate DEX swaps](images/screenshot-7.png)
 
---

## 3) Transaction Flow. Visual scheme
 
```mermaid
flowchart TD
    A["Bybit Hot Wallet (treasury operations) 0xf89d7b9c864f589bbF53a82105107622B35EaA40"] -->|"~30,000 ETH × multiple transfers ~450-500 days ago"| B["Bybit Cold Wallet 1 0x1Db92e2EeBC8E0c075a02BeA49a2935BcD2dFCF4"]
    C["Bybit Hot Wallet (treasury operations) 0xf89d7b9c864f589bbF53a82105107622B35EaA40"] -->|"cmETH, stETH ~455 days ago"| B
    B -->|"401,346 ETH + stETH + cmETH + mETH 21.02.2025 14:16:11 UTC - THE HACK"| D["Bybit Exploiter 1 (Lazarus Group)"]
    D -->|"~50 wallets funds split into smaller amounts ~10,000 ETH"| S["Splitting and Distribution"]
    S -->|"Immediate DEX swaps Uniswap / Paraswap / dodo and other"| E["ETH / USDT (converted assets)"]
    S -->|"Thousands of transactions 100+ pages on Etherscan"| F["Wallet 1, Wallet 2, Wallet 3, ...hundreds more"]
    E -->|"Asset conversion complete"| G["Cross-chain bridges BTC / XMR / THORChain and other chains"]
    F -->|"Each wallet → more wallets 2-3 levels deep = 100+ pages"| G
    G -->|"Final cash out"| H["Cash Out OTC brokers China / SE Asia — no KYC, Huione Guarantee marketplace, Russian instant exchangers — no AML, Korean IT workers — crypto salaries"]
    style D fill:#ff4444,color:#fff
    style G fill:#ff8800,color:#fff
    style S fill:#cc0000,color:#fff
    style H fill:#555555,color:#fff
```
---

![Splitting and Distribution to small amounts = 10000 eth](images/screenshot-8.png)

Only 2 wallets from thousands have more than 14000 transactions(manual tracing is near impossible):

![Bybit Exploiter amount transactions](images/screenshot-9.png)
![Bybit Exploit related address amount transactios](images/screenshot-10.png)

---

## 4) What Happened After? Layering Phase:
 
After the theft, Bybit Exploiter 1 address showed 6 pages of transactions going to hundreds of different wallets.
When I tried to follow the money deeper and opening wallets at level 2 and level 3.
Some of those wallets had 100+ pages of transactions and more than 10000 transactions.
This is exactly what layering looks like in practice:
 
```mermaid
flowchart LR
    A["Bybit Exploiter 1\n$1.5B stolen"] --> B["~50 wallets\nLevel 1"]
    A --> C["~50 wallets\nLevel 1"]
    A --> D["~50 wallets\nLevel 1"]
    B --> E["hundreds of wallets\nLevel 2"]
    C --> F["hundreds of wallets\nLevel 2"]
    D --> G["hundreds of wallets\nLevel 2"]
    E --> H["Impossible to\ntrace manually"]
    F --> H
    G --> H
    H --> I["Cross-chain\n(BTC, XMR, L2)"]
    style A fill:#ff4444,color:#fff
    style H fill:#cc0000,color:#fff
    style I fill:#ff8800,color:#fff
```
 
Why this matters for Anti Money Laundering: Manual analysis is completely impossible at this scale.
This is why blockchain intelligence tools like Chainalysis Reactor and TRM Forensics exist.
They can cluster thousands of addresses automatically and follow the money across chains.
 
![TRM graph](images/screenshot-11.png)
![Chainalysis graph](images/screenshot-12.png)

Here was TRM and Chainalysis graphs, I took them only in educational purposes, noted the sources in the end.
 
---

## 5) Swaps and Why They Used Different Assets
 
I noticed things about the tokens used: stETH, cmETH, mETH. These are liquid staking tokens.
They are not Ethereum. Not every exchange or tool tracks them the same way.
By holding these assets the Lazarus group made the initial theft harder to value and freeze quickly.
 
Immediate swaps via Uniswap and Paraswap. Right after the theft they converted everything to regular Ethereum, DAI, USDC, USDT and other cryptocurrencies.
Why did they do this?
 
- It is easier to move across chains. It is liquid. Easier to cash out.
- DEX swaps do not trigger the Travel Rule. No Know Your Customer, no Anti Money Laundering reporting required.
I saw unknown and flagged tokens have been used for laundering (several tokens on Etherscan marked as unverified or suspicious).
The Lazarus group used these to move value across token standards.
 
- Exit to Layer 2 networks. It is cheaper faster and less monitored.
- Create additional noise in the transaction graph.

![THORchain bridge](images/screenshot-13.png)

---

## 6) Fake Token. "Justice for Bybit"
 
I also noticed a token called "Justice for Bybit" after the hack was sent to the Cold Wallet address with 500,000,000 tokens.
 
This is a classic airdrop scam. It is not related to the hack. Scammers see that a hacked address gets attention create a token with an emotional name 
and airdrop it hoping someone will click a phishing link to "claim" something. 

From an Anti Money Laundering perspective this is noise. It shows how public high-profile hacks attract secondary scams.

![fake token airdrop, justice for bybit](images/screenshot-14.png)

---
 
## 7) Red Flags I Identified
 
| Red Flag | Pattern | Risk Level |
|---|---|---|
| 401,346 ETH sent in single transaction to new address | Massive unusual outflow | 🔴 CRITICAL |
| Destination address (Exploiter 1) had zero prior history | New wallet receives huge amount | 🔴 HIGH |
| Immediate DEX swaps after receipt | Rapid asset conversion | 🔴 HIGH |
| Many pages of outgoing transactions within hours | Fan-out layering pattern | 🔴 HIGH |
| Staked tokens converted immediately | Avoiding asset-specific freezes | 🟡  MEDIUM |
| Transactions to L2 bridges | Cross-chain obfuscation attempt | 🟡 MEDIUM |
| Fake token airdrop | Secondary scam — noise | ⚪️ LOW |
 
---

## 8) What this analysis shows?
 
```mermaid
flowchart LR
    A[" Open address\non Etherscan"] --> B[" Read transaction\nhistory"]
    B --> C[" Build timeline\nof events"]
    C --> D[" Identify the\nexact theft moment"]
    D --> E[" Follow money\nto next wallets"]
    E --> F[" Document\nred flags"]
    F --> G[" Write SAR\n and conclusions"]
```
 
Key conclusions from this real analysis:
 
1. The theft moment is clearly visible one massive transaction to a new address with zero history is an obvious red flag.
2. Layering makes manual tracing impossible (6 pages → 100 pages per wallet). You need tools.
3. DEX is the biggest AML gap, all swaps happened on DEX with no KYC and no Travel Rule. This is where regulation needs to catch up.
4. Speed is the defence and the only way to stop this is real-time alerts, not manual review. By the time a human analyst sees it, the money is already 10 layers deep
   
---

## 9) Key AML Limitation — Why Most Funds Were Not Frozen?

The fundamental challenge in this case: blockchain wallets cannot be frozen directly. AML controls only work at 
the on/off ramp — when funds touch a centralized exchange or a centralised stablecoin like USDT or USDC.

This is why Lazarus immediately:
- Converted USDT/USDC to ETH (decentralized, unfreezable)
- Moved to XMR (untraceable)
- Used DEX instead of centralized exchanges (no KYC)
- Used OTC brokers without AML controls

The ~$400M that WAS frozen — was caught precisely at these on/off ramp points where centralized entities could act.

---

## 10) Conclusions
 
Working through this case on Etherscan surprised me in two ways. The first surprise was how fast it all happened. I thought I'd see a setup before the theft.
Instead 401,346 ETH disappeared in one transaction at 14:16:11 UTC. Within the block range the hackers were already swapping staked assets on Uniswap and other DEX.
There was no delay. This was clearly a planned operation. The second surprise was how the hackers split the stolen assets.
When I opened Level 2 and Level 3 wallets on Etherscan I saw over 100 pages of transactions per address. That's when I realized. Not in theory but in practice. 
Why manually tracing the assets is impossible. It's not a matter of having resources. It's a data volume problem that only automated tools can solve.

**Could AML have prevented this?**

The theft itself. No. This was a supply chain attack on Safe{Wallet}. No KYC process, no CDD checklist no transaction monitoring rule would have flagged
a transaction that looked fully authorized from the blockchains perspective. Standard AML was blind here. The response is a different story.
Alerts from Chainalysis and TRM Labs resulted in around $400M being frozen across exchanges in the weeks after the hack. That outcome was only possible because of 
automated blockchain monitoring, not review. By the time a human analyst finished tracing one wallet level the funds were already three chains deep.
This tells me that the value of AML in crypto is not about preventing attacks. It's about detecting and responding quickly after the fact.

**What I want to show with this case:**

- I can work with on-chain data using free public tools and extract meaningful conclusions.
- I understand how DPRK-attributed hackers think and operate. Not the compliance checklist version, but the operational pattern
- Why they used liquid staking tokens.
- Why DEX was chosen over CEX, why THORChain and XMR appear at specific stages.
- I can see the gaps. DEX has no Travel Rule obligation, no KYC, no SAR requirement and understand why that matters for the future of crypto AML.

This is not just theory. Every transaction, every timestamp, every wallet in this case is publicly verifiable, on Etherscan now.

---

## Part 3: Mock SAR (Suspicious Activity Report)

**Inbound deposit from DPRK Lazarus Group address**
 
**Disclaimer:** This is a simulated SAR written for educational and portfolio purposes. Based on publicly available on-chain data and industry reporting.
No confidential information was used. All institution details are fictional.

**Note on format:** In a real FinCEN BSA E-Filing System, this document would be submitted as plain text with no tables or markdown formatting. 
Red flags would be listed as numbered plain text paragraphs. Tables and formatting are used here for portfolio readability only. Filing Details 
would be entered in separate form fields, not in the narrative itself.
 
---
 
## Filing Institution
 
| Field | Detail |
|---|---|
| Institution Name | Clear Exchange Ltd. |
| Institution Type | Virtual Asset Service Provider (VASP) / Money Services Business (MSB) |
| FinCEN Registration | MSB Registration No. XXXXXXX |
| Registered Address | 123 Compliance Street, Wilmington, DE 19801, USA |
| Compliance Contact | AML Compliance Department |
| Contact Email | compliance@clear-exchange.com |
| Contact Phone | +1 (XXX) XXX XXX |
 
---
 
## Report Details
 
| Field | Detail |
|---|---|
| Date of Report | Febuary 26, 2025 |
| SAR Type | Suspected proceeds of cybercrime / DPRK sanctions evasion / WMD proliferation financing |
| Filing Deadline | Within 30 days of detection per BSA requirements met |
| Prior SARs on Subject | None on file |
 
---
 
## Subject of Report
 
| Field | Detail |
|---|---|
| Name | Unknown, unhosted wallet, no account at this institution |
| Account Number at Clear Exchange | N/A — no account held by sending address |
| Receiving Account | XXX-XXXXXX (John Smith — account holder at Clear Exchange) |
| Sending Wallet Address | 0x47666Fab8bd0Ac7003bce3f5C3585383F09486E2 |
| Attribution | Lazarus Group (DPRK) — Reconnaissance General Bureau (RGB) |
| OFAC Status | SDN List — active designation |
| Blockchain | Ethereum (ETH) |
 
Attribution is based on publicly available cluster data from Chainalysis and TRM Labs, consistent with OFAC SDN designation of Lazarus Group addresses.
 
---
 
## Description of Suspicious Activity
 
On February 24, 2025 at 09:41 UTC, Clear Exchange Ltd. received a real-time alert from our blockchain analytics system (Chainalysis KYT)
flagging an inbound deposit of **14,200 ETH (~$37.8M USD)** to customer account XXX-XXXXXX (John Smith).
 
The sending address — 0x47666Fab8bd0Ac7003bce3f5C3585383F09486E2 — was identified as **Bybit Exploiter 1**, directly attributed to 
Lazarus Group (DPRK) per Chainalysis and TRM Labs cluster data and consistent with OFAC SDN designation.
 
### Background — Source of Funds
 
The sending address received 401,346 ETH (~$1.5B USD) on February 21, 2025 at 14:16:11 UTC in a single transaction from Bybit Cold Wallet 1
(0x1Db92e2EeBC8E0c075a02BeA49a2935BcD2dFCF4) - the address compromised in the Bybit exchange hack of February 21, 2025. This event has been publicly 
attributed to Lazarus Group by Chainalysis, TRM Labs, and is consistent with OFAC SDN designation of the involved addresses.
 
Between February 21 and February 24, 2025, the following laundering activity was observed from the sending address prior to the deposit attempt at Clear Exchange:
 
- Immediate conversion of staked assets (stETH, cmETH, mETH) to ETH via Uniswap and Paraswap within 30 minutes of initial receipt, bypassing any asset-specific freeze mechanisms
- Fan-out distribution to approximately 50 intermediate wallets (Level 1), approximately 10,000 ETH each
- Further splitting across hundreds of Level 2–3 wallets generating 80–100+ pages of outgoing transactions per wallet on Etherscan
- Cross-chain bridge activity via THORChain converting ETH to BTC
- Partial conversion to Monero (XMR) to break transaction traceability

A full list of approximately 50 Level 1 wallet addresses directly funded from Bybit Exploiter 1 is attached to this report as **Attachment 1 (layer1-addresses.csv)**.
Hundreds of Level 2–3 addresses are maintained in our internal blocklist and are available to law enforcement upon request.
 
The 14,200 ETH deposited at ClearPath Exchange represents a portion of the original stolen funds that passed through multiple layering stages before reaching our platform.
 
### Activity at Clear Exchange
 
The deposit was directed to account XXX-XXXXXX registered under the name **John Smith**, opened on January 15, 2025 — 37 days prior to this deposit.
The account had no prior transaction history consistent with a deposit of this size or origin.
 
Upon receipt of the Chainalysis KYT alert:
 
- The deposit of 14,200 ETH was **automatically held** by our compliance system
- The account was **immediately restricted** from all activity
- Compliance Officer was notified within 5 minutes of alert
- Funds remain frozen pending law enforcement instruction
  
The account holder was not notified of the reason for restriction in accordance with tipping off prohibition 31 U.S.C. § 5318(g)(2).
 
---
 
## Red Flags Identified
 
> Note: In a real FinCEN filing these would be submitted as numbered plain text.
> Table format is used here for portfolio readability only.
 
| Indicator | Risk Level |
|---|---|
| Inbound deposit received from OFAC SDN-listed address attributed to Lazarus Group (DPRK) — funds frozen upon receipt | 🔴 CRITICAL |
| Sending address directly linked to $1.5B Bybit hack publicly documented and OFAC designated | 🔴 CRITICAL |
| 14,200 ETH (~$37.8M) received in single transaction | 🔴 CRITICAL |
| Funds passed through 3 layers of intermediate wallets before reaching platform | 🔴 HIGH |
| Sending address funded via DEX swaps, no Travel Rule triggered at source | 🔴 HIGH |
| Cross-chain bridge activity (THORChain) observed prior to deposit | 🔴 HIGH |
| Deposit inconsistent with account holder's established transaction profile | 🔴 HIGH |
| Receiving account opened 37 days prior, no prior high-volume activity | 🟡 MEDIUM |
 
---
 
## Actions Taken
 
| Date / Time | Action |
|---|---|
| Feb 24, 2025 — 09:41 UTC | Chainalysis KYT alert received, sending address flagged as Lazarus Group / OFAC SDN |
| Feb 24, 2025 — 09:43 UTC | Deposit automatically held by compliance system |
| Feb 24, 2025 — 09:46 UTC | Compliance Officer notified, case escalated to senior management |
| Feb 24, 2025 — 10:15 UTC | Account XXX-XXXXXX restricted,  all activity suspended |
| Feb 24, 2025 — 10:30 UTC | OFAC notification filed separately - mandatory for SDN match, independent of SAR filing |
| Feb 24, 2025 — 14:00 UTC | All identified cluster addresses (~50 Level 1 + hundreds Level 2–3) added to internal blocklist |
| Feb 25, 2025 | FBI Cyber Division notified — DPRK state-sponsored cyber threat |
| Feb 26, 2025 | SAR filed with FinCEN within 30-day BSA requirement |
 
> Note: OFAC notification was filed separately and immediately upon SDN match on February 24, 2025.
> This is a separate legal obligation from SAR filing and does not replace it.
 
> Note: Account holder was not informed of SAR filing or reason for account restriction per tipping off prohibition 31 U.S.C. § 5318(g)(2).
 
---
 
## WMD Proliferation Financing Flag
 
This SAR requires escalation beyond standard AML/CTF procedure.
 
Per publicly available attribution by Chainalysis, TRM Labs, and consistent with OFAC SDN designation, Lazarus Group operates under North Korea's 
Reconnaissance General Bureau (RGB). UN Security Council Resolutions 1718 (2006) and 2270 (2016) prohibit all member states from facilitating transactions 
that could contribute to DPRK's nuclear or ballistic missile programs.
 
This filing includes a WMD proliferation financing flag and has been escalated to national security channels in addition to standard FinCEN SAR filing.
 
---
 
## Analyst Note
 
This activity is consistent with post-theft laundering methodology publicly attributed to Lazarus Group by Chainalysis and TRM Labs following the
February 21, 2025 Bybit hack.
 
**Ongoing monitoring required:** Any future transaction originating from identified cluster addresses must trigger an immediate freeze and re-escalation.
This case should not be treated as resolved — Lazarus Group clusters remain active and continue to move funds.
 
**Regulatory gap observed:** DEX protocols used in this case (Uniswap, Paraswap, THORChain) do not currently fall under Travel Rule obligations per FATF
Recommendation 16. This allowed layering to proceed without triggering inter-VASP reporting at the most critical stage. This gap is noted for law enforcement context.
 
**Frozen funds status:** 14,200 ETH remains frozen at Clear Exchange Ltd. pending law enforcement instruction. This institution will cooperate fully with
any investigation and will provide all requested documentation including Attachment 1 and internal blocklist data.
 
---
 
**END OF THE MOCK SAR**
 
---
 
> **Filing Details Note:** In a real FinCEN BSA E-Filing System, the following
> information would be entered in separate form fields, not in the narrative itself.
>
> Filed with: FinCEN — BSA E-Filing System
> Filing date: February 26, 2025
> OFAC notification: Filed separately — February 24, 2025
> Records retention: Minimum 5 years
 
---

## Sources:
 
- Chainalysis article
  https://www.chainalysis.com/blog/bybit-exchange-hack-february-2025-crypto-security-dprk/
- Chainalysis Crypto Crime Reports 2025 on youtube
  https://www.youtube.com/watch?v=EZJW4PWoEjM&t=33s&pp=ygUkY2hhaW5hbHlzaXMgY3J5cHRvIGNyaW1lIHJlcG9ydCAyMDI1
  https://www.youtube.com/watch?v=XlTTeZbz_34&pp=ygUkY2hhaW5hbHlzaXMgY3J5cHRvIGNyaW1lIHJlcG9ydCAyMDI10gcJCQQLAYcqIYzv
- TRM Labs articles
  https://www.trmlabs.com/resources/blog/the-bybit-hack-following-north-koreas-largest-exploit
  https://www.trmlabs.com/resources/blog/bybit-hack-update-north-korea-moves-to-next-stage-of-laundering
- TRM Labs youtube video
  https://www.youtube.com/watch?v=MJjP5ZIyu0k&pp=ygUTdHJtIGxhYnMgYnliaXQgaGFjaw%3D%3D
- 31 U.S.C. § 5318(g)(2) https://www.law.cornell.edu/uscode/text/31/5318
- UN Security Council Resolutions 1718 (2006) https://unscr.com/en/resolutions/1718/
- UN Security Council Resolutions 2270 (2016) https://unscr.com/en/resolutions/2270/\

---

*Prepared by Andrey Kotsyk — AML/Blockchain Forensics Portfolio*

*This is a fictional scenario for educational and portfolio purposes only. All institution details are fictional. Wallet addresses are publicly available on-chain.*

*www/linkedin.com/in/andrey-kotsyk*
