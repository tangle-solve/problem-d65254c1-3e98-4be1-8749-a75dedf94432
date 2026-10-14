<!-- solve-problem:d65254c1-3e98-4be1-8749-a75dedf94432 -->
# Many-miner private-fork equilibria

A [Tangle Solve problem](https://solve.tangle.tools/problems/d65254c1-3e98-4be1-8749-a75dedf94432).

## What is the problem?

In a proof-of-work blockchain, miners extend the longest public chain, but a "selfish" miner can withhold mined blocks and release a private fork strategically, forcing honest miners to waste work on branches that are later orphaned. The census asks: **characterize the (Markov perfect / Nash) equilibria of the mining game when there are arbitrarily many miners, each of whom may maintain a private fork, with heterogeneous hash rates and incomplete information** (a miner cannot observe others' private forks or even whether they are attacking).

Formal setting: discrete time; miner $i$ has hash rate $\alpha_i$, $\sum \alpha_i = 1$; each round one block is found by $i$ with probability $\alpha_i$; the state for $i$ includes the public chain length, $i$'s private fork lead, and (crucially) not others' forks. Each miner chooses per-round a policy: mine on public tip or private fork, and when to release withheld blocks. Payoff is discounted share of blocks in the eventual consensus chain. This is a stochastic game with *private monitoring* (imperfect, asymmetric observation of others' actions). The target is: for which $(\alpha_1,\dots,\alpha_n)$ does a non-trivial withholding equilibrium exist, and what are the equilibrium policies? Ambiguity in the original statement: whether equilibria are required to be symmetric among small miners, whether the reference protocol is plain Nakamoto longest-chain or a modified tie-breaking rule, and whether the solution concept is MPE or Bayesian equilibrium.

## Why does it matter?

Selfish-mining analyses (threshold $\approx 25–33\%$ hash rate under various assumptions) underpin security claims for Bitcoin-style consensus and inform protocol patches (uniform tie-breaking, uncle rewards). These thresholds are single-attacker results; if two 20% pools jointly withholding can extract comparable rents, published security margins are wrong. A multi-miner equilibrium characterization would tell protocol designers when attacks are individually or jointly rational, and whether competition between selfish miners is self-limiting (they orphan each other) or mutually reinforcing.

## What's already known?

- Eyal–Sirer (2014) and follow-ups (Sapirshtein et al. 2016, MDP optimization) fully solve the *single* attacker against honest majority: optimal withholding policies, revenue vs. $\alpha$, and the critical threshold (from $>1/4$ to $>1/3$ depending on tie-breaking and propagation parameters).
- Kiayias et al. (2016) model mining as a stochastic game among miners and study deviations from honest behavior, giving conditions under which honest mining is/not an equilibrium for a large miner; this is the direct theoretical ancestor of the census item. It does not characterize multi-player private-fork equilibria.
- The census cites a 2026 empirical paper (Peresíni et al., arXiv:2601.02984) simulating *multiple simultaneous* selfish miners on Nakamoto, Fruitchains, and Strongchain, comparing specified strategy profiles — but it evaluates fixed strategies, not equilibria.
- The general obstacle: with private forks, each miner's information set excludes others' hidden state, so the game has private monitoring; even two-player stochastic games with private monitoring generally lack tractable equilibrium characterizations (related to the folk-theorem frontier for such games).

## What would count as solving it?

1. **Existence/non-existence:** For given $n$, $(\alpha_i)$, and discount factor, prove whether a Markov perfect equilibrium with positive withholding probability exists (beyond the trivial honest equilibrium).
2. **Characterization:** Give the equilibrium policy structure — e.g., "each miner withholds iff private lead $\le \ell_i(\alpha_i, \text{beliefs})$" — as a function of hash-rate vector and information structure.
3. **Thresholds:** Derive attack thresholds generalizing the single-miner 25–33\% bound: does the set of hash-rate vectors admitting withholding equilibria shrink or grow with $n$?
4. Partial credit: a clean two-player, two-type Bayesian version with closed-form equilibrium would be a publishable step, as would counterexample constructions showing no symmetric equilibrium exists for some configurations.

## How to start

1. Read Eyal–Sirer (2014) for the single-attacker state machine and Sapirshtein et al. (2016) for the MDP formulation — these give the state variables you'll extend.
2. Read Kiayias et al. (2016) §on the stochastic-game formulation; recast their two-player incomplete-information construction with $n$ players and ask what breaks (belief tracking over others' hidden fork lengths).
3. Natural approach: start with **two selfish miners, full information** (both forks visible to each other, hidden only to honest majority) — this is a well-defined stochastic game solvable by value iteration/experiment; then progressively hide information.
4. Main obstacle: the state space of joint private leads grows unboundedly and beliefs become continuous; look for dominant-strategy or threshold-form arguments rather than full MPE computation, or exploit that only the *relative* lead matters.

## Key references

- A. Kiayias, E. Koutsopoulos, A. Tselekounis, *Blockchain Mining Games*, arXiv:1607.02420 (2016). Models mining as a stochastic game and constructs a deviation strategy for a large miner showing when honest mining fails to be an equilibrium. https://arxiv.org/abs/1607.02420
- I. Eyal, E.G. Sirer, *Majority is not Enough: Bitcoin Mining is Vulnerable*, Financial Crypto 2014 (arXiv:1311.0243). Defines selfish mining, computes attacker revenue and the ~25\% threshold under default tie-breaking.
- A. Sapirshtein, Y. Sompolinsky, A. Zohar, *Optimal Selfish Mining Strategies in Bitcoin*, Financial Crypto 2016 (arXiv:1507.06181). Solves the single-attacker problem exactly via MDPs; threshold up to ~33\% with optimal tie-breaking.
- P. Peresíni et al., *Selfish Mining in Multi-Attacker Scenarios: An Empirical Evaluation of Nakamoto, Fruitchain, and Strongchain*, arXiv:2601.02984 (2026). Simulates multiple simultaneous selfish miners and compares fixed strategy profiles across three protocols; per census record (verify — I cannot confirm this paper's content).
- (Background on the game-theoretic obstacle) stochastic games with private monitoring literature, e.g., Mailath & Samuelson, *Repeated Games and Reputations* (2006), chapters on imperfect monitoring.

## Warnings

- The 2026 empirical paper and its arXiv ID come from the census record and may be hallucinated or misdated; verify before citing.
- Don't assume attackers' private forks are visible to each other — the whole difficulty is private monitoring; results with full attacker observability are strictly easier and must be labeled as such.
- Beware "equilibrium" ambiguity: honest mining can coexist with profitable deviations (the game is not necessarily a dilemma), so "equilibrium" must be pinned to a solution concept (MPE vs. sequential vs. Bayesian) in any claim.
- Single-attacker thresholds (25–33\%) do not extrapolate: two competing selfish miners orphan each other's releases, so naive threshold addition is a known dead end.

---
*`BCGT-000226` · flagged for review*

## Acceptance guidance

1. **Existence/non-existence:** For given $n$, $(\alpha_i)$, and discount factor, prove whether a Markov perfect equilibrium with positive withholding probability exists (beyond the trivial honest equilibrium).
2. **Characterization:** Give the equilibrium policy structure — e.g., "each miner withholds iff private lead $\le \ell_i(\alpha_i, \text{beliefs})$" — as a function of hash-rate vector and information structure.
3. **Thresholds:** Derive attack thresholds generalizing the single-miner 25–33\% bound: does the set of hash-rate vectors admitting withholding equilibria shrink or grow with $n$?
4. Partial credit: a clean two-player, two-type Bayesian version with closed-form equilibrium would be a publishable step, as would counterexample constructions showing no symmetric equilibrium exists for some configurations.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/d65254c1-3e98-4be1-8749-a75dedf94432?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000226.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000226)
