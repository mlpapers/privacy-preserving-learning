# Privacy-Preserving Machine Learning

## Protected information and trust models

- **[Differential privacy](https://www.cis.upenn.edu/~aaroth/Papers/privacybook.pdf)** (2014 monograph). Bounds how output distributions change between neighboring datasets. The adjacency relation defines the protected unit; epsilon and delta describe a mechanism’s guarantee, not the probability that an individual record is safe.

- **[Record-level versus user-level privacy](https://www.cis.upenn.edu/~aaroth/Papers/privacybook.pdf)** (2014 monograph). Changing one record and changing all records of one person are different adjacency relations. Group privacy degrades with group size; a record-level guarantee cannot be reported unchanged as protection for a whole user.

- **[Local differential privacy: RAPPOR](https://arxiv.org/abs/1407.6981)** (2014). Randomizes client reports using randomized response and Bloom filters for string statistics. Repeated-report protection needs separate accounting; local privacy trades statistical efficiency for reduced trust in a collector.

- **[Shuffle-model amplification](https://arxiv.org/abs/1811.12469)** (2018). Anonymously shuffling locally randomized reports can strengthen the central DP guarantee. Amplification relies on the shuffling protocol and adversary’s view; removing names from otherwise revealing records is not the same mechanism.

- **[Secure aggregation](https://research.google/pubs/practical-secure-aggregation-for-privacy-preserving-machine-learning/)** (2017). Lets a server recover aggregate client updates without seeing individual updates under stated protocol assumptions. Protects messages during computation; the aggregate or released model may still reveal training information.

- **[Encrypted inference: CryptoNets](https://proceedings.mlr.press/v48/gilad-bachrach16.html)** (2016). Evaluates a transformed neural network on homomorphically encrypted inputs and returns encrypted predictions. Protects the inference input from the evaluator; this is a different target from DP protection for training examples.

## Noise mechanisms and privacy accounting

- **[Sensitivity-calibrated mechanisms](https://www.cis.upenn.edu/~aaroth/Papers/privacybook.pdf)** (2014 monograph). Laplace and Gaussian mechanisms calibrate noise to how much a query can change on neighboring data. Bounding contributions is essential; adding noise without sensitivity analysis does not establish differential privacy.

- **[Composition and post-processing](https://www.cis.upenn.edu/~aaroth/Papers/privacybook.pdf)** (2014 monograph). Repeated private accesses consume privacy budget; computation using only a private release preserves its guarantee. New access to raw private data is not post-processing, even when it occurs during model selection.

- **[Rényi differential privacy](https://arxiv.org/abs/1702.07476)** (2017). Bounds Rényi divergence between outputs on neighboring datasets. Privacy costs add at each order under composition, then convert to an epsilon–delta guarantee; the selected order affects the resulting bound.

- **[Zero-concentrated differential privacy](https://arxiv.org/abs/1605.02065)** (2016). Controls privacy-loss tails through Rényi-divergence bounds across orders. Gives convenient composition and group-privacy analysis; its parameter must be converted before comparison with epsilon–delta DP.

- **[Gaussian differential privacy](https://arxiv.org/abs/1905.02383)** (2019). Describes privacy through hypothesis-testing tradeoffs between neighboring outputs. Gaussian DP provides a one-parameter family and composition approximations; asymptotic approximations and finite-composition bounds should be distinguished.

- **[Subsampling amplification](https://arxiv.org/abs/1808.00087)** (2018). Bounds privacy after sampling a subset before applying a private mechanism. Sampling probability and scheme enter the theorem; an accountant must match how the implementation actually forms training batches.

- **[Privacy-loss random-variable accounting](https://arxiv.org/abs/2106.02848)** (2021). Numerically composes privacy-loss distributions with bounded approximation error. Tightens accounting for repeated mechanisms; evaluates a specified mechanism without auditing whether code implements it.

- **[Private hyperparameter selection](https://arxiv.org/abs/2110.03620)** (2021). Non-private tuning can leak information even when the final training run is private. Papernot and Steinke analyze search procedures under Rényi DP; reported budgets must account for the selection procedure and its assumptions.

## Private optimization and knowledge transfer

- **[Output and objective perturbation](https://arxiv.org/abs/0912.0071)** (2009). Adds randomness to parameters or the optimization objective. Chaudhuri et al. prove privacy for regularized ERM under convexity, smoothness and sensitivity conditions; arbitrary neural objectives need separate analysis.

- **[DP-SGD](https://arxiv.org/abs/1607.00133)** (2016). Clips per-example gradients, adds Gaussian noise and accounts for sampled iterations. Clipping bounds each example’s contribution; noise scale, sample rate and training steps jointly determine the privacy guarantee.

- **[DP-FTRL](https://arxiv.org/abs/2103.00039)** (2021). Uses tree aggregation to perturb cumulative clipped gradients with noise correlated across steps. Avoids reliance on sampling/shuffling amplification; repeated contributions still need accounting. Trades noise structure against optimization and compute.

- **[User-level private federated learning](https://arxiv.org/abs/1710.06963)** (2017). Clips and noises user contributions rather than individual-example gradients. McMahan et al. train recurrent language models with user-level DP; grouping, participation sampling and contribution limits define the protected unit.

- **[Private Aggregation of Teacher Ensembles](https://arxiv.org/abs/1610.05755)** (2016). Trains teachers on disjoint private partitions and labels public inputs through noisy voting. A student learns from these labels; privacy belongs to the aggregation mechanism, not merely to using an ensemble.

- **[Selective teacher queries](https://arxiv.org/abs/1802.08908)** (2018). Adds Gaussian noise to teacher votes and a noisy consensus threshold that can withhold labels. Selective querying reduces disclosure on disagreements; data-dependent privacy accounting includes the threshold test and answered queries.

- **[Private synthetic data: AIM](https://arxiv.org/abs/2201.12677)** (2022). Adaptively selects and privately measures statistical queries, then fits synthetic data to noisy answers. Utility depends on the query workload; synthetic appearance alone is no privacy guarantee, and downstream fidelity needs evaluation.

## Pretraining and empirical utility

- **[Private language-model fine-tuning](https://arxiv.org/abs/2110.05679)** (2021). Li et al. combine public pretraining, matched objectives and DP-specific tuning; ghost clipping avoids materializing full per-example gradients. Privacy covers the private fine-tuning data, not an unaccounted pretraining corpus.

- **[Private vision at scale](https://arxiv.org/abs/2204.13650)** (2022). Averages gradients over multiple augmentations before clipping each image; large batches and normalization improve tested DP-SGD accuracy. Reported privacy budgets exclude hyperparameter-search costs, so they do not cover the full tuning procedure.

## Attacks, memorization and empirical auditing

- **[Membership inference](https://arxiv.org/abs/1610.05820)** (2016). Infers whether a candidate record was used for training from model responses. Shokri et al. train shadow-model attacks; success depends on access and data assumptions, while a failed attack does not prove privacy.

- **[Likelihood Ratio Attack](https://arxiv.org/abs/2112.03570)** (2021). Fits per-example distributions of model confidence with and without membership. Evaluates true positives at low false-positive rates, exposing risks hidden by average attack accuracy; shadow-model assumptions affect the attack.

- **[Canary exposure](https://arxiv.org/abs/1802.08232)** (2018). Inserts rare synthetic sequences and measures how unusually highly the model ranks them. Exposure quantifies tested memorization and extraction risk; it is not a universal certificate for all private information.

- **[Training-text extraction](https://arxiv.org/abs/2012.07805)** (2020). Carlini et al. recover verbatim training sequences from GPT-2, including examples appearing in one document. Generation can reveal memorized data; membership classification and extracting an unknown sequence are distinct attack targets.

- **[Deep Leakage from Gradients](https://arxiv.org/abs/1906.08935)** (2019). Optimizes dummy inputs to match shared gradients and reconstructs training data in studied image/text settings. Keeping raw records on-device does not ensure that transmitted gradients conceal them.

- **[Gradient inversion beyond toy settings](https://arxiv.org/abs/2003.14053)** (2020). Uses magnitude-invariant gradient matching to reconstruct high-resolution images from trained networks. Architecture, batch size and attacker knowledge matter; the experiments refute treating gradient sharing as inherently private.

- **[One-run privacy auditing](https://arxiv.org/abs/2305.08846)** (2023). Uses many independently included or excluded examples to audit a mechanism in one training run. Produces empirical privacy lower bounds under the audit’s assumptions; it can expose a claimed bound’s failure, not certify every possible attack.

## Software and reporting

- **[Opacus](https://opacus.ai/tutorials/guide_to_module_validator)** (documentation). Implements DP-SGD in PyTorch. Its validator rejects BatchNorm, whose batch statistics couple records; alternatives include GroupNorm. Sampling and accounting must still match the executed training procedure.

- **[Comparable privacy–utility results](https://arxiv.org/abs/2110.03620)** (2021). Record protected unit, adjacency, epsilon/delta, sampling, composition, tuning and public data. Compare utility under matched guarantees; equal epsilon with different units or omitted private searches is not an equal privacy budget.

## Related topics

[Federated learning](https://github.com/mlpapers/federated-learning) · [Distributed learning](https://github.com/mlpapers/distributed-learning) · [Machine unlearning](https://github.com/mlpapers/machine-unlearning) · [Evaluation](https://github.com/mlpapers/evaluation) · [Generative models](https://github.com/mlpapers/generative-models)
