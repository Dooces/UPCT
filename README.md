Unified Predictive Control Theory (IACE ≈ UPCA)
1. Core Claim

Biological control is a resource-priced arbitration between cheap, habitual policies and costly, generative simulation. The brain escalates to model-based control only when residual error persists and resources permit, with admission to global broadcast gated by identifiable thalamo-basal ganglia circuits. This yields ordered degradation under scarcity, oscillations under conflict, and specific neuromodulator × receptor × circuit effects.

2. Surplus Signal (Ŝ): Inference, Not Homunculus
2.1 Latent Allostatic State

Surplus is not a single “fuel gauge.” Instead, the brain infers a low-dimensional latent state S⃗ from heterogeneous interoceptive/autonomic signals:

𝑥
(
𝑡
)
=
[
𝐸
fast


𝐸
mid


𝐸
slow


𝑂
2


𝐶
𝑂
2


𝐻
sleep


𝐻
hydr
]
,
𝑥
˙
=
𝐴
𝑥
+
𝑢
(
𝑡
)
+
𝜂
(
𝑡
)
x(t)=
	​

E
fast
	​

E
mid
	​

E
slow
	​

O
2
	​

CO
2
	​

H
sleep
	​

H
hydr
	​

	​

	​

,
x
˙
=Ax+u(t)+η(t)

E_fast: ATP/O₂, seconds–minutes

E_mid: glucose/glycogen, minutes–hours

E_slow: fatigue/recovery, hours–days

H_sleep: homeostatic sleep drive (Process-S)

H_hydr: hydration/osmotic balance

O₂, CO₂: gas exchange constraints

Sensors (noisy): HR/HRV, pupil, end-tidal CO₂, SpO₂, glucose, skin conductance, fNIRS/fMRI BOLD.

Discrete inference:

𝑥
^
𝑡
∣
𝑡
−
1
=
𝐹
𝑥
^
𝑡
−
1
+
𝐵
𝑢
𝑡
−
1
,
𝐾
𝑡
=
𝑃
𝑡
∣
𝑡
−
1
𝐶
⊤
(
𝐶
𝑃
𝑡
∣
𝑡
−
1
𝐶
⊤
+
𝑅
)
−
1
x
^
t∣t−1
	​

=F
x
^
t−1
	​

+Bu
t−1
	​

,K
t
	​

=P
t∣t−1
	​

C
⊤
(CP
t∣t−1
	​

C
⊤
+R)
−1
𝑥
^
𝑡
=
𝑥
^
𝑡
∣
𝑡
−
1
+
𝐾
𝑡
(
𝑦
𝑡
−
𝐶
𝑥
^
𝑡
∣
𝑡
−
1
)
,
𝑆
^
𝑡
=
softplus
(
𝑤
⊤
𝑥
^
𝑡
+
𝑏
)
x
^
t
	​

=
x
^
t∣t−1
	​

+K
t
	​

(y
t
	​

−C
x
^
t∣t−1
	​

),
S
^
t
	​

=softplus(w
⊤
x
^
t
	​

+b)

with monotone 
𝑤
≥
0
w≥0.

2.2 Neuroanatomical Substrates

Sensing: NTS, parabrachial nucleus, carotid body, vagal afferents

Integration: posterior/anterior insula, vmPFC (predictive coding hub); hypothalamus (slow drives)

Broadcast: insula/vmPFC → ACC/FPC; hypothalamus → LC/VTA/NBM for global gain

Key property: Ŝ is a fit-and-test latent. If it fails to track behavior under metabolic clamps (hypoxia, ketone infusion, glucose), the model fails.

3. Threshold (θ): Circuit-Level Gating Bound

θ is not a scalar knob but a composite of identifiable gating mechanisms:

𝜃
𝑡
=
𝜃
0
+
𝑘
TRN
𝑔
TRN
(
𝑡
)
+
𝑘
STN
𝑟
STN
(
𝑡
)
−
𝑘
𝐷
1
𝑎
𝐷
1
(
𝑡
)
+
𝑘
𝐷
2
𝑎
𝐷
2
(
𝑡
)
+
𝑘
FPC
𝑏
FPC
(
𝑡
)
θ
t
	​

=θ
0
	​

+k
TRN
	​

g
TRN
	​

(t)+k
STN
	​

r
STN
	​

(t)−k
D1
	​

a
D1
	​

(t)+k
D2
	​

a
D2
	​

(t)+k
FPC
	​

b
FPC
	​

(t)

TRN: thalamic reticular nucleus → broadcast gate

STN: hyperdirect pathway → “hold your horses” brake

D1/D2 striatum: Go/No-Go bias

FPC: raises threshold when multiple alternatives active

Operationalization:

θ_str (BG update gate) = striatal Go/NoGo balance; measured via PBWM/response-threshold DDM

θ_brd (broadcast ignition) = TRN/parietal-PFC ignition; measured via P3b amplitude, PLV synchrony, TMS–EEG effective connectivity

𝜃
=
max
⁡
(
𝜃
str
,
𝜃
brd
)
θ=max(θ
str
	​

,θ
brd
	​

)

4. Arbitration: Market of Competing Controllers

Controllers bid for control:

MF habits: dorsolateral striatum, cerebellum

MB planning: dorsomedial striatum + PFC/hippocampus

Pavlovian reflex: amygdala/brainstem

Interoceptive homeostasis: insula/hypothalamus

Exploration/branching: FPC

Auction dynamics:

𝑧
˙
𝑖
=
𝛽
𝑖
(
𝑆
^
)
𝑈
𝑖
−
𝜆
𝑧
𝑖
−
∑
𝑗
≠
𝑖
𝛾
𝑖
𝑗
𝑧
𝑗
+
𝜉
𝑖
z
˙
i
	​

=β
i
	​

(
S
^
)U
i
	​

−λz
i
	​

−
j

=i
∑
	​

γ
ij
	​

z
j
	​

+ξ
i
	​

𝑈
𝑖
=
𝐸
[
Δ
𝐹
𝐸
𝑖
]
time_cost
𝑖
(
DA, 5-HT
)
+
energy_cost
𝑖
(
𝑆
^
)
U
i
	​

=
time_cost
i
	​

(DA, 5-HT)+energy_cost
i
	​

(
S
^
)
E[ΔFE
i
	​

]
	​


Gate opens for controller i when 
𝑧
𝑖
>
𝜃
𝑡
z
i
	​

>θ
t
	​

.

Oscillations

Conflict yields oscillations if cross-inhibition + fatigue/delays induce limit cycles (e.g., all-nighter toggling between “push work” and “fall asleep”). Predictable Hopf regime: 
𝛾
>
𝜆
,
𝜅
>
0
,
𝜏
𝑑
>
0
γ>λ,κ>0,τ
d
	​

>0.

5. Neuromodulator Matrix

Not knobs, but receptor × target × timescale effects:

Tx	Receptor	Target	Effect
NE (LC)	α1/β	PFC gain ↑, TRN ↓, FPC exploration ↑	Lowers θ at moderate levels; too high = reset
NE (LC)	α2	PFC recurrent stabilizer ↑	Raises θ in fatigue
ACh (nAChR)	thalamus/PFC relay	Input gain ↑, encoding ↑	Lowers bound for new evidence
ACh (mAChR, M1/M2)	PFC recurrent, TRN	Stabilization ↑	Raises bound; REM protection
DA (D1)	striatum direct	Go bias ↑, policy precision ↑	Shortens deliberation; effective θ↑ under time pressure
DA (D2)	striatum indirect	No-go bias ↑	Raises θ; cautious
5-HT	1A/2A	PFC integration horizon	Raises θ; patience ↑; 2A state-dependent lowering in psychedelics
6. Learning & Plasticity

Weights update by:

Δ
𝑤
∝
Hebb
×
𝑔
ctrl
×
prediction error
Σ
×
salience
−
downscale
Δw∝Hebb×g
ctrl
	​

×
Σ
prediction error
	​

×salience−downscale

Controller tag: plasticity proportional to which controller won

Sleep:

NREM: hippocampal replay → MB credit assignment

REM: muscarinic stabilization + recombination; pruning of unused traces

7. Predictions & Falsifiability

Operational readouts

Ŝ: latent filter fit to physiological channels; validated against clamps

θ: separate θ_str (BG gating indices) and θ_brd (ignition markers)

Directional tests

Hypoxia/CO₂: lowers O₂ latent → Ŝ↓ → reduced MB weight despite constant ε_res

Insula TMS: noisy Ŝ (posterior variance ↑) → more LC-NE resets; less stable MB engagement

nAChR vs mAChR: nicotine lowers θ, scopolamine raises θ; if both same, reject receptor-specificity

STN-DBS: reduces θ_str (faster commit) but not MB admission; if MB admission ↑, θ mislocalized

DA/urgency: raises opportunity cost; paradoxically reduces MB even with high Ŝ

Sleep pressure: MB fails first, then executive, then habit → reproduces clinical hypoglycemia/hypoxia ordering

Ketone infusion: increases MB if Ŝ tracks true energy, not just glucose

Pre-registered nulls: if ≥2 fail, theory is falsified (not “saved” post hoc).

8. Integration with IACE / UPCA

Mapping:

IACE	UPCA	Circuit	Observable
Speculative Engine (MA)	MB planner	PFC–hipp	replay, ΔFE estimate
Automatic Pathways (APs)	SI/habits	DLS, cerebellum	MF weight
Subconscious Allocator (SC)	AMC	ACC/FPC ↔ BG gate	θ_str, θ_brd
Surplus	Ŝ filter	Insula/hypothalamus/brainstem	S latents
Global broadcast	Ignition gate	TRN + parietal–PFC	P3b/PLV/TMS-EEG

Thus IACE and UPCA are the same control principle at different descriptions.

9. Compact Formulas

MB engagement:

𝑔
MB
=
𝜎
(
𝛼
𝜀
res
+
𝛽
𝑆
^
−
𝜃
𝑡
)
g
MB
	​

=σ(αε
res
	​

+β
S
^
−θ
t
	​

)

Gate bound:

𝜃
𝑡
=
𝜃
(
𝑇
𝑅
𝑁
,
𝑆
𝑇
𝑁
,
𝐷
1
/
𝐷
2
,
𝐹
𝑃
𝐶
;
 
𝑁
𝐸
,
𝐴
𝐶
ℎ
𝑛
/
𝑚
,
𝐷
𝐴
,
5
−
𝐻
𝑇
)
θ
t
	​

=θ(TRN,STN,D1/D2,FPC; NE,ACh
n/m
	​

,DA,5−HT)

Auction dynamics:

𝑧
˙
𝑖
=
𝛽
𝑖
(
𝑆
^
)
𝑈
𝑖
−
𝜆
𝑧
𝑖
−
∑
𝑗
≠
𝑖
𝛾
𝑖
𝑗
𝑧
𝑗
+
𝜉
𝑖
z
˙
i
	​

=β
i
	​

(
S
^
)U
i
	​

−λz
i
	​

−
j

=i
∑
	​

γ
ij
	​

z
j
	​

+ξ
i
	​

10. Why It Matters

Explains: imagery biases, intuition, sleep-dependent pruning, hypoglycemia/hypoxia collapse ordering, scarcity-graded control degradation

Novel predictions: ACh double dissociation (nAChR vs mAChR), DA urgency paradox, ketone infusion effect

Implementable: State-space Ŝ filter, auction gate, plasticity update = runnable code

Falsifiable: pre-registered manipulations with declared fail criteria
