# Multi-armed_bandit

## Basics

Ref: [Wiki-MAB](https://en.wikipedia.org/wiki/Multi-armed_bandit)

Definition:
	K levers(摇杆, ~game machine)
		The reward distribution of each lever is:
			B={R_1, R_2,…, R_K }
			With mean value of the distribution as:
			μ_1, μ_2, …, μ_K
	T rounds
		Each round the gambler play one machine, get a reward r_t
		
	Then, the regret is defined as the difference between our strategy with the ground truth best strategy:
		ρ=T⋅μ^∗  −∑_(t=1)^T▒r_t 
		Where:
			μ^∗=max⁡(μ_k )
			
	i.e. target is to minimize the regret
	
	Zero-regret strategy: the strategy that make P( ρ/T→0 while T→ inf⁡) →1
i.e. the strategy can