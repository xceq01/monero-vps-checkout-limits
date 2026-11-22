# Monero VPS: how long XMR checkout takes, how much disk a node needs, and the limits that only show up after you pay

Monero blocks arrive about every two minutes, and that number sets the pace of everything downstream. Hosts that run their own Monero node tend to deploy small VPS orders on the first confirmation; larger orders wait for the ten confirmations the Monero project recommends, roughly twenty minutes. Hosts that hand crypto off to a payment gateway quote a wider window, and AVA Hosting's payments FAQ puts order confirmation and access details at 20–30 minutes after payment.

The server is usually the cheaper half of the decision. What catches people out is disk: the Monero chain is about 220 GB and grows around 30 GB a year, so the plan you pick today runs out of room well before the CPU does. This guide covers what happens between sending XMR and getting a root prompt, what the merchant can actually see, how to size a box for a node, and the restrictions that live in spec sheets and terms rather than the sales page.

> Most buyers need the smallest KVM box that clears their disk requirement, in a European location. At AVA Hosting that is **VPS One: 1 vCPU, 2 GB ECC RAM, 25 GB NVMe, 1 Gbps port, unlimited transfer, €5.00/month**, with weekly backups and DDoS protection included. It is enough for a VPN endpoint, a Tor relay, a private Git server or a single site. It is not enough for a Monero node.
>
> 👉 [See AVA Hosting's current VPS plans and pricing](https://my.ava.hosting/aff.php?aff=15562)

## AVA Hosting's VPS catalogue, plan by plan

AVA sells two overlapping KVM catalogues on the same €2–€40 ladder, and the specs diverge where it matters for storage-heavy work. The main VPS/VDS line runs from a €11-per-year entry box to a 160 GB machine:

| Plan | vCPU / RAM (ECC) | NVMe | Network | Price | Order |
| --- | --- | --- | --- | --- | --- |
| VPS Nano | 1 / 512 MB | 20 GB | 100 Mbps, unlimited | €11.00 / year | [Order VPS Nano](https://my.ava.hosting/aff.php?aff=15562) |
| VPS One | 1 / 2 GB | 25 GB | 1 Gbps, unlimited | €5.00 / month | [Order VPS One](https://my.ava.hosting/aff.php?aff=15562) |
| VPS Basic | 2 / 4 GB | 50 GB | 1 Gbps, unlimited | €10.00 / month | [Order VPS Basic](https://my.ava.hosting/aff.php?aff=15562) |
| VPS Lucky | 2 / 6 GB | 70 GB | 1 Gbps, unlimited | €15.00 / month | [Order VPS Lucky](https://my.ava.hosting/aff.php?aff=15562) |
| VPS Power | 4 / 8 GB | 90 GB | 1 Gbps, unlimited | €20.00 / month | [Order VPS Power](https://my.ava.hosting/aff.php?aff=15562) |
| VPS Extra | 6 / 12 GB | 120 GB | 1 Gbps, unlimited | €30.00 / month | [Order VPS Extra](https://my.ava.hosting/aff.php?aff=15562) |
| VPS Ultra | 8 / 16 GB | 160 GB | 1 Gbps, unlimited | €40.00 / month | [Order VPS Ultra](https://my.ava.hosting/aff.php?aff=15562) |

Every plan on that line lists Intel Xeon E5 cores, full root, weekly backups, free DDoS protection and a 99.9% uptime SLA. Two things to note before you get attached to the cheapest row: VPS Nano is billed annually only and its own spec sheet shows **0 available**, and the same sheet says **port 25 is closed** on that plan, which rules out running a mail server on it.

The second catalogue carries a "Self-hosted Start" label and different plan names, and the disk allocation per euro is more generous:

| Plan | vCPU / RAM / NVMe | Price | Order |
| --- | --- | --- | --- |
| Start | 1 / 1 GB / 15 GB | €2.00 / month | [Order Start](https://my.ava.hosting/aff.php?aff=15562) |
| Mini | 1 / 2 GB / 25 GB | €5.00 / month | [Order Mini](https://my.ava.hosting/aff.php?aff=15562) |
| Basic | 2 / 4 GB / 50 GB | €10.00 / month | [Order Basic](https://my.ava.hosting/aff.php?aff=15562) |
| Pro | 4 / 8 GB / 100 GB | €20.00 / month | [Order Pro](https://my.ava.hosting/aff.php?aff=15562) |
| Business | 6 / 12 GB / 150 GB | €30.00 / month | [Order Business](https://my.ava.hosting/aff.php?aff=15562) |
| Power | 8 / 16 GB / 200 GB | €40.00 / month | [Order Power](https://my.ava.hosting/aff.php?aff=15562) |

Compare the €20 and €40 rows across the two catalogues and the choice becomes concrete: €20 buys 90 GB on the main line and 100 GB on the self-hosted line; €40 buys 160 GB against 200 GB. If the workload is disk-bound and CPU-idle, which describes a syncing node, the self-hosted line wins on capacity at the same price. The trade is that the self-hosted table lists unlimited transfer only, with no weekly backup, DDoS or SLA wording attached to those plans.

Payment is the part AVA documents separately. Its payments FAQ lists **XMR among the coins accepted through Cryptomus**, alongside BTC, ETH, LTC, USDT, USDC and others, and states that once you pick "All Crypto Payments" at checkout you are redirected to Cryptomus's payment page. That FAQ carries a March 2024 date, so confirm the coin list still includes XMR at the invoice step rather than assuming it from a two-year-old page.

## What other XMR-friendly hosts charge, for calibration

Prices across this market are quoted on four different bases, so a straight "cheapest" ranking is meaningless. Use the table to locate the band your budget sits in, not to pick a winner.

| Provider | Entry price and billing basis | Locations | One limit worth knowing |
| --- | --- | --- | --- |
| CrestVPS | €5.90/mo monthly, €4.13/mo on annual billing (−30%); entry box 2 vCPU / 4 GB / 60 GB | 36 cities in 33 countries | Crypto only, no cards accepted; 72-hour first-order guarantee pays out as account credit, not cash |
| Mynymbox | €4.00/mo; entry box 512 MB / 10 GB | Netherlands, Finland | Outbound port 25 blocked by default; mining prohibited, running a Monero node allowed |
| Skhron | €2.58/mo listed, but most rows showed out of stock at the time of checking | Stockholm, Warsaw | No dedicated IPv4 included, €0.65/mo each; no managed backups or snapshots |
| NoctHost | $10/mo, billed hourly from $0.014; entry box 1 vCPU / 1 GB / 25 GB | 33 cities, 6 continents | Balance top-ups are final and non-refundable |
| BitVPS | Five tiers priced in USD and converted to XMR at invoice time; no surcharge for paying in XMR | Multiple regions via its deploy page | Overpayments are not refunded automatically; you supply a return address or take account credit |

The €4.13 figure is an annual-billing equivalent, the €4 is a monthly starting price for one location, the $10 is a monthly rate that drops to $0.014/hour if you destroy the box, and the €2.58 row was unbuyable when checked. Anyone presenting those four as a single ranking is comparing billing cycles, not servers.

## After you send the XMR: six steps, and where the wait actually is

BitVPS documents the flow that hosts running their own node tend to follow, and it is worth knowing because it explains why "payment confirmed" and "server ready" are not the same moment.

1. You pick a plan and a region, choose Monero, and the invoice is bound to a fresh subaddress that has never been used and will not be reused.
2. Checkout shows an integrated address (subaddress plus payment ID in one string) with a QR code, which modern wallets such as Feather, Cake, Monerujo, Stack, Edge or the Monero GUI recognise and pre-fill.
3. You broadcast from your wallet. The mempool sees a Bulletproof-protected commitment, not an amount or a set of source outputs.
4. The first confirmation lands in about two minutes. Small VPS-tier invoices get marked confirmed here; larger orders wait for ten confirmations, roughly twenty minutes.
5. While the transaction is still in the mempool, the provisioning system pre-stages the disk image and network rules, so the VM is already up when the block confirms.
6. IPv4, IPv6 prefix, root password or your SSH key, and a panel link arrive by email. BitVPS quotes a median of under three minutes from payment to root login.

Gateway-based checkouts work differently. AVA's flow sends you to Cryptomus, and the confirmation email with access details is quoted at 20–30 minutes. NoctHost describes the same shape from the buyer's side: top up a balance, wait for standard confirmations (typically within about 20 minutes), then deploy, with the exact confirmation requirement displayed before you send.

Which model suits you depends on how impatient you are and how much you care about who sits in the middle. Direct-to-host payment with a self-run node is faster and keeps the invoice inside one company. A hosted gateway is fine and usually still KYC-free, but a third party processes the payment, so the privacy boundary moves to that party's policy rather than the host's.

## What the host can and cannot see

Monero hides sender, receiver and amount by default. RingCT combines Pedersen commitments with zero-knowledge range proofs so amounts stay encrypted on-chain; Bulletproofs and Bulletproofs+ keep those proofs small enough to be practical; stealth addresses mean the address you publish never appears on-chain as the destination.

For a hosting invoice, the merchant holds the viewkey for the subaddress it generated. That lets it decrypt the amount received and match the transaction ID to your invoice. It does not let it see which wallet you sent from, what else that wallet holds, or the rest of your transaction graph. That is a property of the protocol, not a promise in a terms page.

One practical consequence: a merchant running its own node sees one subaddress, one amount and one txid. A merchant using a gateway sees whatever the invoice page showed, but the matching happens inside the gateway. If payment-level unlinkability against the processor matters to you, the direct-to-node model is the one to prefer, and the KYC question is separate from it. Several hosts in this market will happily take a card or a SEPA transfer alongside XMR, so anonymity depends on the payment path you select, not on the brand.

## How much VPS a Monero node actually needs

Full node storage is the number that decides your plan. As of mid-2026 the chain is roughly **220 GB and growing about 30 GB per year**, which makes 250 GB the working minimum and 400 GB comfortable for anything long-lived. A cold sync takes **12–24 hours on Gen4 NVMe**, and steady-state traffic runs about 3–5 GB in and out per day, mostly transaction relay and block propagation.

A pruned node keeps only the last eighth of the chain and fits in **roughly 80 GB**. That changes the buying decision completely.

- **Pruned node only:** 80 GB is the floor, and you want headroom for the next year or two of pruning, so budget 100–120 GB. On AVA's main line that is VPS Extra (120 GB, €30/mo); the 90 GB VPS Power (€20/mo) clears the minimum with less slack, and the self-hosted Pro plan gives 100 GB at the same €20.
- **Full node:** 250 GB minimum, 400 GB comfortable. Nothing in AVA's catalogue reaches 240 GB, so a permanent full node means either pruned mode, offloading the chain to a block device, or a provider with larger tiers.
- **Full node plus public RPC:** open 18089 for restricted RPC so wallets can connect, keep 18080 open for peer relay, and put a TLS-terminating proxy in front if you want rate limits. Budget 16 GB of RAM for comfort.
- **Full node plus wallet-RPC:** add a few months of growth on top of 400 GB. This is the case where a 200 GB plan runs out inside two years of chain growth no matter how little you use it.

👉 [Check the 90 GB VPS Power and 120 GB VPS Extra plans](https://my.ava.hosting/aff.php?aff=15562)

Tor and I2P are supported by Monero natively, and some hosts state explicitly that Tor relays, including exits, and I2P routers are allowed. If you plan to run the node behind Tor, confirm the host's policy before you pay, because a relay is a different workload from a node and not every provider allows it.

## The restrictions that live in the spec sheet, not the sales page

None of these appear in a hero section, and all of them cost money or break a workload after you have paid.

- **IPv4 may be an extra.** Skhron includes a routed /64 IPv6 prefix and no IPv4 at all, with addresses at €0.65/month each. If you need a dedicated IPv4 for inbound services, read the include list before comparing prices.
- **Outbound port 25 is commonly blocked.** AVA flags it on VPS Nano directly in the spec sheet; Mynymbox blocks outbound 25 by default and takes exceptions on request; Skhron allows inbound 25 without restriction but restricts outbound. Any mail-sending plan needs this confirmed in writing.
- **Backups are not universal.** AVA includes weekly backups on its main VPS line, which is unusual at €5/month. Skhron states plainly that it provides no managed backups or snapshots, so the copy is your job.
- **Topping up is not the same as paying an invoice.** NoctHost credits a balance and states that top-ups are final, with no refund path; BitVPS credits overpayments to your account or returns them to an address you supply, but does not refund automatically. Send the exact amount.
- **Mining is usually banned while nodes are fine.** Mynymbox prohibits mining and allows a Monero node in the same breath. The distinction is CPU abuse versus relay traffic.
- **DDoS protection is not always per-customer.** Skhron protects its own network and says sustained attacks on one service may be filtered or null-routed. AVA lists free DDoS protection on every VPS plan without qualifying it.
- **Refund terms are the hardest thing to find.** Neither AVA catalogue page states a money-back window. If you are about to pay for a year, ask support first and keep the answer.

👉 [See which AVA Hosting VPS plans are in stock](https://my.ava.hosting/aff.php?aff=15562)

## Who should not buy the bigger plan

A personal VPN endpoint, a Tor bridge, a private Git or password server, a single low-traffic site, or a WireGuard hop between two networks all fit in 1–2 GB of RAM and 25 GB of disk. VPS One at €5/month covers that, and the €2 Start plan covers a pure VPN if 15 GB is enough for the OS and nothing else.

Move up only for three reasons: the disk requirement of a node, the RAM requirement of a public RPC, or a workload that genuinely needs more than two cores. VPS Lucky at €15 sits in an awkward spot for node work, since 70 GB is below the 80 GB pruned threshold. VPS Ultra's 8 vCPU and 16 GB are wasted on a relay that spends its day moving 3–5 GB of block data.

Fleet discounts and annual pricing change the arithmetic elsewhere: CrestVPS takes 30% off for annual billing and stacks 3–12% for 3, 5 or 10 servers, and neither AVA catalogue page shows a comparable multi-server discount. If you are buying five boxes, that gap is worth more than the per-plan differences above.

## Picking a plan in one line

Pruned node, cheapest viable: 90–100 GB, which means AVA VPS Power at €20/month or self-hosted Pro at €20 for 100 GB. Pruned node with growth room: 120 GB, VPS Extra at €30. Public RPC with 16 GB of RAM: self-hosted Power at €40 for 200 GB. Everything else: VPS One at €5.

Two things to confirm before you send XMR. First, whether the plan includes a dedicated IPv4, because it changes what you can host and it is the line item most often sold separately. Second, what happens to your money if the server does not work out, since neither AVA catalogue page states a refund window and a gateway payment is not the same as a card chargeback.

👉 [Start from AVA Hosting's VPS catalogue](https://my.ava.hosting/aff.php?aff=15562)
