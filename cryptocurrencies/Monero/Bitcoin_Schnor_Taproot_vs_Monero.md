https://www.reddit.com/r/Monero/comments/n75jhc/a_post_about_schnorr_taproot/
https://libredd.it/r/Monero/comments/n75jhc/a_post_about_schnorr_taproot/

~Hope I'm not violating any rules; but~ I know people here have questions re Taproot and how it compares against Monero. I've been unsure about the implications, but I think I understand it well enough to form some ideas now. That said, I'm not a cryptographer, I'm not a dev. I'm an educated layman with a background in EE, semicon, sysadmin, and a hobbyist in open source security software. So I might get some details wrong, and if I do please let me know.

Schnorr is a signature scheme, and Taproot leverages Schnorr, which enables signature aggregation for Bitcoin scripts. Kinda similar to Monero ring signatures (a single sig can prove that one member signed, without knowing which member); however in Taproot, it's broader than that. Bitcoin has scripts, mostly timelocks and multi-sig (which Monero does not). Specifically:

In Taproot, a single signature can prove the satisfaction of a complex script, with multiple timelocks and multiple signing parties. Onchain, this appears to have just a single signature, and a simple send-to-self has the same appearance as a complex function, (like what might occur in a coinjoin or coinswap).

This is cool, and saves space onchain. Instead of having to provide M of N signatures to prove the satisfaction of the script (with each sig taking up precious bytes in the txn message); you can just provide one signature. This reduces the cost of coinjoins. It will reduce the ability to determine if a transaction was a LN opening/closing operation.

Or put differently: It won't be readily apparent whether or not a Taproot output is a multisig or singlesig, or has a timelock associated with it.

### Stuff Taproot Does Not Do
It doesn't break the UTXO trasaction graph or linkability. In other words, known UTXOs are still known and tracked. In Monero, you don't know which output was actually selected for spending, which makes the transaction graph probablistic, rather than deterministic. In Taproot, specific UTXOs are still being selected for the inputs to any transaction, and new UTXOs being created from those. The transaction graph is still deterministic.

It doesn't hide amounts. This enables a wealth of correlations and linkages. One small example, coinjoins. It will be pretty obvious that a Wasabi style coinjoin took place, when the final result is 50 new UTXOs each with exactly 0.1 BTC.

It doesn't eliminate heuristics and metadata. Wallets will still have the fingerprint of how their specific wallet constructs transactions. I think it's likely that there will still be evidence of the types of transactions you're making. You still leak your IP address, not only with every transaction, but with every lookup of your address balances. The exception would be the very small number of people running their own node over Tor.

It isn't default and doesn't enforce Taproot migration at the network level. Wallets still have to integrate it. People still have to adopt it. There will be introduction of significant friction, just like we saw (and still see) wallet incompatibilities with the new address formats in Segwit. We're talking years to get people to switch.

You might even be tempted to say that it's optional privacy, but it's not even that. It obscures the type of transaction being conducted. But the transaction graph is still known with certainty, and the linkages to your UTXO set will largely remain intact, especially since it's likely that most UTXOs are already known by chain analysis. You could try to coinjoin with somewhat reduced costs, but I'm willing to bet that heuristics and metadata will still give away the fact that you did so. Besides, unnecessary extra transactions will continue to be priced out of the static 1MB blocksize limit.

**TLDR: Taproot is cool and is definitely an improvement and innovation for Bitcoin. It saves space, and obscures the type of transaction being conducted. But it's a marginal privacy improvement, an optional improvement, which does nothing to break the certainty of the transaction graph, doesn't mask amounts, and does very little to prevent UTXO linkability to real identities, particularly in the face of heuristics, metadata, and the reality that vast majority of Bitcoin UTXOs are already known by chain analysis.**

## Comments

- **Thank you for taking the time to write this out. Extremely informative! So why doesn’t bitcoin protocol just adopt Monero’s privacy method? Is it that they are just incompatible with each other?**
  - Because it would require massive overhaul of large portions of the code, multiple hard forks, and would remove the ability to do scripts. There's just no realistic way to add even a portion of what Monero does, without significant existential risks to the entire Bitcoin project.
    - Why can't you do scripts in Monero?
      - Because the cryptography implemented in Monero doesn't have this capability. I don't know if there is any cryptography out there capable of doing ring signatures for transaction graph obfuscation while also doing script evaluation. But really, if you want a real nuts and bolts answer, a dev would have to respond, because I'm giving an educated guess on that one. 
 ----
