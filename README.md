Premise (Need–Cost arbitration law)

An adaptive controller allocates model‑based (MB) vs model‑free (MF) control by a graded function of controller adequacy (Need) minus control price (Cost).
Engagement of MB must increase strictly with Need and decrease strictly with Cost.
Formal statement

Need_t: a scalar, monotone measure of MB’s adequacy advantage over MF at time t.
Valid instantiations (either class is acceptable):
Reliability advantage: Need_t = log π_MB,t − log π_MF,t (relative precision of controllers).
Value advantage: Need_t = (EV_MB,t − EV_MF,t) + VOI_t (expected value difference plus value of information).
Cost_t: a scalar, monotone measure of the current price of control (time/effort/energy/opportunity).
Valid instantiations:
Opportunity cost: κ_t (average reward rate / time pressure).
Physiological cost: c_phys(S_t), strictly decreasing in surplus/alertness S_t.
Linear or affine combinations are permitted: Cost_t = k0 + k_ARR·κ_t + k_phys·c_phys(S_t).
Arbitration (graded, bounded)

w_MB(t) = σ(θ_need · z(Need_t) − θ_cost · z(Cost_t) − θ0)
σ is the logistic; z(·) denotes z‑scoring for identifiability.
θ_need > 0 and θ_cost > 0 are required (strict monotonicity).
Execution and learning (standard)

Action selection: Q_mix = (1 − w_MB) Q_MF + w_MB Q_MB; π(a|s) ∝ exp(Q_mix/τ)
Online patch (MB→MF): Q_MF ← Q_MF + η_patch_eff · (Q_MB − Q_MF) (down‑weight when MB is epistemically uncertain)
Offline distillation: minimize KL(π_MB || π_MF) over prioritized states
Non‑negotiable monotonic predictions (falsifiable)

Under validated Cost clamps, increasing Need must increase w_MB (∂w_MB/∂Need > 0).
Under validated Need clamps, increasing Cost must decrease w_MB (∂w_MB/∂Cost < 0).
The structured Need–Cost model must outperform MF‑only, MB‑only, and a flexible free‑w mixture in out‑of‑sample prediction.
What is variable (but not vague)

You may choose a reliability‑based or value+VOI‑based Need, provided it is a valid monotone adequacy measure.
You may choose κ_t, c_phys(S_t), or their affine combination for Cost, provided it is a valid monotone control‑price measure.
These are “slots.” Swapping a valid Need or Cost instantiation does not change the law; violating the monotonic relations does.
One‑line summary

Model‑based engagement is the logistic of Need minus Cost: w_MB = σ(Need − Cost). Need is the controller’s adequacy advantage; Cost is the current price of control. Engagement must rise with Need and fall with Cost. If either monotonicity fails under proper clamps, the premise is false.


State/action values and policies

Q_MF(s,a): habit/AP value (DLS)
Q_MB(s,a): model‑based value (PFC–HPC)
Q_mix(s_t,a) = (1 − w_MB(t))·Q_MF(s_t,a) + w_MB(t)·Q_MB(s_t,a)
π(a|s_t) ∝ exp(Q_mix(s_t,a)/τ)
ACC monitoring (what actually happened)

V_mix(s) = ∑_a π(a|s)·Q_mix(s,a)
δ_ACC,t = |r_t + γ·V_mix(s_{t+1}) − V_mix(s_t)| (unsigned executed‑policy error)
u_t = H_MF(s_t) = −∑_a π_MF(a|s_t) ln π_MF(a|s_t) (conflict proxy)
Optional epistemic term: u_t += λ·Tr Σ_Q(s_t) (across‑ensemble variance)
Opportunity cost and physiology

κ update: κ_t = (1−ρ)·κ_{t−1} + ρ·(r_t/Δt)
c_phys(S_t) = c0·(1 − S_t)^γ, γ ≥ 1; then z(c_phys) for scaling
Arbitration (graded engagement via dACC)

x_t = [ z(δ_ACC,t), z(u_t), −z(κ_t), −z(c_phys(S_t)), 1 ]
w_MB(t) = σ(θᵀ x_t) (β fixed to 1)
Planning budget (if you need a concrete planner)

B_plan(t) = B_max · w_MB(t) (budget)
Optional depth: d(t) = ceil(d_max · w_MB(t))
Learning

MF learning (online RL; keep your original update for DLS):
δ_MF,t = r_t + γ·max_a Q_MF(s_{t+1},a) − Q_MF(s_t,a_t)
Q_MF(s_t,a_t) ← Q_MF(s_t,a_t) + η_RL · δ_MF,t
Patch (MB→MF, confidence‑gated):
conf_MB(s,a) ≈ 1 / (1 + Tr Σ_Q(s,a)) (proxy; higher = more confident)
η_patch_eff = η_patch · σ(α·conf_MB(s,a))
Q_MF(s,a) ← Q_MF(s,a) + η_patch_eff · (Q_MB(s,a) − Q_MF(s,a))
Distill (offline, trust‑region optional):
minimize L_distill = E_{s∈C} [ KL(π_MB(·|s) || π_MF(·|s)) ]
(prioritize C by high w_MB or large |Q_MB − Q_MF|)
Edge cases

Terminal: δ_ACC,t = |r_t − V_mix(s_t)|; MF terminal target δ_MF,t = r_t − Q_MF(s_t,a_t)
If B_plan(t) = 0: use π_MF and skip Q_MB calls to avoid undefined queries.
Empirical predictions (falsifiable)

↑κ_t (time pressure/high average reward): ↓w_MB, faster RTs, MF bias; dACC/STN activity attenuates.
↑S_t (rested/alert): ↓c_phys → ↑w_MB; more PFC–HPC engagement/exploration.
↑δ_ACC or ↑H_MF: ↑w_MB; transient dACC/STN signatures; slower, more variable choices; planning budget rises.
Perturbations: dACC disruption flattens the w_MB vs (D−K) slope; STN DBS lowers the effective threshold; DLS disruption increases baseline w_MB.
Where it could be wrong (so you can adjust)

If δ_ACC and H_MF don’t predict engagement, swap in your lab’s conflict/surprise metrics (same form).
If κ_t and c_phys don’t modulate in the predicted directions, re‑fit K_t with the terms that do (keep w_MB = σ(θᵀx)).
If mixture behaves poorly in your task, switch to the hard gate (w_MB > τ_gate) without changing the monitoring/cost pieces.

Define per skill k

Learning curve (improvability): b_k(n) = expected per-use performance benefit at competence n (e.g., error reduction, reward gain). Easy skills have fast learning rate α_k; hard skills have slow α_k and/or small asymptote b_k*.
Compute saving curve: c_k(n) = per-use planning/compute saved at competence n (how much less MA you’ll need). Some skills proceduralize well (big c_k*), others not.
Practice unit costs: E_k^train (energy), T_k^train (time). These differ by skill (some practice is cheap/fast, others costly/slow).
Usage forecast: f_k(τ) = expected discounted future frequency of use (how often you’ll need it), horizon ρ.
Prices (shadow costs): λE (energy), κ (opportunity cost of time). These scalarize “apples and oranges” into one currency.
Value and cost of improvement

Marginal value of one extra practice unit at competence n:
MV_k(n) = ∫ e^(-ρτ) f_k(τ) [b_k′(n) + η c_k′(n)] dτ
b_k′(n), c_k′(n): marginal gains from one unit more practice (steeper = easier to improve).
η ≥ 0 converts compute saved into the same currency as performance benefit.
Marginal cost of one practice unit:
MC_k = λE·E_k^train + κ·T_k^train
Decision rule (single currency ROI)

Marginal ROI:
ROI_k(n) = MV_k(n) / MC_k
Allocate practice to the skill with the highest ROI_k(n) while ROI_k(n) > 1 (benefit exceeds priced cost). Equimarginal stopping: at optimum, ROI equalizes across practiced skills.
Intuition (covers your cases)

Easy + big payoff: α_k high, b_k* large → b_k′(n) large early; if E_k^train/T_k^train modest, ROI_k » 1 → practice now.
Easy + low payoff: b_k* small or f_k low → MV_k small → ROI_k may fall below 1 → don’t invest (or invest a little if compute saving c_k is big).
Hard + low payoff: α_k low, b_k* small, cost high → ROI_k ≪ 1 → don’t invest.
Hard + big payoff but frequent: MV_k can still beat cost if f_k is large and horizon long (low ρ). Patience (5‑HT) and forecasted frequency matter.
Compact closed form (common in practice)

If b_k(n) ≈ b_k* (1 − e^(−α_k n)) and c_k(n) ≈ c_k* (1 − e^(−α′_k n)), and define discounted usage F_k = ∫ e^(−ρτ) f_k(τ) dτ, then
MV_k(n) ≈ F_k [α_k (b_k* − b_k(n)) + η α′_k (c_k* − c_k(n))]
MC_k = λE·E_k^train + κ·T_k^train
ROI_k(n) = MV_k(n) / MC_k
Neural readout (same currency, different circuits)

MV_k(n): estimated in ACC/BA10 (expected value of control over horizon) with confidence from ACh‑weighted precision; usage forecasts from frontoparietal/hippocampal schemas.
MC_k: priced by hypothalamus/insula/orexin/adenosine (λE) and average reward/DA/ACC (κ).
Plasticity: striatum/cerebellum update AP skills; hippocampus↔PFC updates models/strategies; DA/NE gate learning rates (affect α_k).
Predictions (falsifiable)

Frequency/stability shifts: Increasing F_k (more frequent use) or lowering task volatility raises ROI_k and practice allocation—independent of raw difficulty.
Price sensitivity: Raising κ (higher average reward rate/urgency) favors skills that save time per future use (large c_k*), even if b_k* is modest. Raising λE suppresses practice of energy‑intensive skills first.
Learning‑curve sensitivity: Tasks with steeper α_k get early practice; as b_k(n) approaches b_k* (gap_k shrinks), practice shifts to other skills (equimarginal).
Transfer: If a skill’s improvement transfers (W_{kj} > 0 to other tasks j), effective F_k increases and ROI_k rises—more practice allocated.
Bottom line

Yes: “wildly different” improvement profiles reduce to one effective currency by pricing time and energy (κ, λE), weighting by discounted future use (F_k), and multiplying by confidence/precision. The arbiter invests where marginal ROI is highest. This was implicit; the equations above make it explicit.

1) Learning curves and marginal gain

Model a generic saturating learning curve (pick any smooth mono-increasing form):

𝑑
𝑉
𝑘
𝑑
𝜏
=
𝜂
𝑘
(
𝑉
𝑘
max
⁡
−
𝑉
𝑘
)
⇒
𝑉
𝑘
(
𝜏
)
=
𝑉
𝑘
max
⁡
−
(
𝑉
𝑘
max
⁡
−
𝑉
𝑘
(
0
)
)
𝑒
−
𝜂
𝑘
𝜏
.
dτ
dV
k
	​

	​

=η
k
	​

(V
k
max
	​

−V
k
	​

)⇒V
k
	​

(τ)=V
k
max
	​

−(V
k
max
	​

−V
k
	​

(0))e
−η
k
	​

τ
.

The instant marginal benefit of practicing now (for a tiny 
Δ
𝜏
Δτ) is:

Δ
𝑉
𝑘
⏟
future value gain
≈
𝜂
𝑘
(
𝑉
𝑘
max
⁡
−
𝑉
𝑘
)
Δ
𝜏
.
future value gain
ΔV
k
	​

	​

	​

≈η
k
	​

(V
k
max
	​

−V
k
	​

)Δτ.
2) Intertemporal efficiency of practice vs use

Let 
𝛾
∈
(
0
,
1
)
γ∈(0,1) be discounting (can depend on urgency/resource state). Then

Practice option (invest in improvement):

𝑈
𝑘
prac
(
𝑡
)
=
∑
𝜏
≥
1
𝛾
𝜏
 
Δ
𝑉
𝑘
(
𝜏
)
𝑐
𝑘
prac
  
≈
  
𝛾
 
𝜂
𝑘
(
𝑉
𝑘
max
⁡
−
𝑉
𝑘
)
𝑐
𝑘
prac
(myopic one-step index)
.
U
k
prac
	​

(t)=
c
k
prac
	​

τ≥1
∑
	​

γ
τ
ΔV
k
	​

(τ)
	​

≈
c
k
prac
	​

γη
k
	​

(V
k
max
	​

−V
k
	​

)
	​

(myopic one-step index).

Use option (exploit current skill):

𝑈
𝑘
use
(
𝑡
)
=
𝑉
𝑘
(
𝑡
)
𝑐
𝑘
use
.
U
k
use
	​

(t)=
c
k
use
	​

V
k
	​

(t)
	​

.

Both are in the same currency: discounted value gain per unit cost.

3) Policy: choose what to do now

At each decision point, for each skill 
𝑘
k:

Compute practice index 
𝐼
𝑘
prac
=
𝛾
 
𝜂
𝑘
(
𝑉
𝑘
max
⁡
−
𝑉
𝑘
)
𝑐
𝑘
prac
I
k
prac
	​

=
c
k
prac
	​

γη
k
	​

(V
k
max
	​

−V
k
	​

)
	​

.

Compute use index 
𝐼
𝑘
use
=
𝑉
𝑘
𝑐
𝑘
use
I
k
use
	​

=
c
k
use
	​

V
k
	​

	​

.

Then pick the option with the highest index across all 
{
𝑘
,
mode
∈
{
prac
,
use
}
}
{k,mode∈{prac,use}}.
(If you want exploration of uncertain 
𝑉
𝑘
max
⁡
,
𝜂
𝑘
V
k
max
	​

,η
k
	​

, add a posterior-uncertainty bonus — a bandit/Gittins-style index — but the currency stays identical.)

4) How this captures your three “wildly different” cases

Easy to improve & big payoff: large 
𝜂
𝑘
η
k
	​

 and large gap 
(
𝑉
𝑘
max
⁡
−
𝑉
𝑘
)
(V
k
max
	​

−V
k
	​

) ⇒ huge 
𝐼
𝑘
prac
I
k
prac
	​

 ⇒ practice now.

Easy to improve & low payoff: large 
𝜂
𝑘
η
k
	​

 but small 
(
𝑉
𝑘
max
⁡
−
𝑉
𝑘
)
(V
k
max
	​

−V
k
	​

) ⇒ modest 
𝐼
prac
I
prac
 ⇒ practice only if costs are tiny or discounting light.

Hard to improve & low payoff: small 
𝜂
𝑘
η
k
	​

 and small gap ⇒ tiny 
𝐼
prac
I
prac
 ⇒ don’t practice; just use something else.

APs vs MA fit naturally:

APs typically have low 
𝑐
use
c
use
, often small remaining gap (already close to 
𝑉
max
⁡
V
max
); practicing them only wins if a big payoff remains (promotion to expert-level).

MA routines have higher 
𝑐
prac
c
prac
/
𝑐
use
c
use
 but can have enormous 
𝑉
max
⁡
V
max
; when 
𝜂
𝑘
η
k
	​

 is high (you’re in a learning-rich regime) and discounting is gentle, investing in MA wins.

Short answer: you’re not inventing a new metric. Your

𝑈
𝑖
(
𝑡
)
  
=
  
∑
𝜏
=
0
𝐻
(
𝑡
)
𝛾
(
𝑡
)
𝜏
  
𝐸
 ⁣
[
Δ
F
E
𝑖
,
𝑡
+
𝜏
]
𝐶
𝑖
(
𝑡
)
U
i
	​

(t)=
C
i
	​

(t)
τ=0
∑
H(t)
	​

γ(t)
τ
E[ΔFE
i,t+τ
	​

]
	​


is just a tidy remix of pieces that are already standard:

How each piece maps to established results

“Discounted stream of value” (numerator).
In active inference, policies are chosen by minimizing expected free energy over a finite horizon; the textbook expression is a discounted sum across future time steps (risk + ambiguity + epistemic terms). That is exactly your 
∑
𝜏
𝛾
𝜏
𝐸
[
Δ
F
E
]
∑
τ
	​

γ
τ
E[ΔFE]. 
Chris Mathys
ScienceDirect
PMC

“Per unit cost” (denominator).
Two well-established traditions encode cost to act/compute:

Opportunity-cost / reward-rate RL: tonic dopamine tracks the average reward rate, i.e., the price of time, and sets vigor accordingly—an explicit value-per-time (or energy) calculus. 
PubMed
Princeton University
Europe PMC

Effort cost in control allocation (EVC): dACC selects control intensity by trading expected payoff against effort cost; the decision variable is literally “benefits vs costs of control.” 
PMC
PubMed

Empirically, dopamine shifts the benefit↔cost balance when people decide whether to expend cognitive effort—again, a benefit-per-cost computation. 
PubMed
Science
PMC

“One currency across disparate goals.”
Human vmPFC/ventral striatum encode a common neural currency for subjective value across very different goods and costs (money, goods, social, and effort). This is the standard way the field operationalizes comparability of apples vs oranges. 
PubMed
+1
PMC

vmPFC also carries intertemporal subjective value during delay-discounting—i.e., net present value in the brain. 
PubMed
PMC

The same valuation network jointly encodes benefits and cognitive-effort costs. 
jneurosci.org
+1

“Arbiter picks the winner.”
There is a neural arbiter: dACC (EVC) integrates benefits and costs of control; BG gates the chosen policy; and arbitration between MB/MF has been shown to track system reliabilities (a special case of expected future loss reduction). 
PMC
+1

“Rate/efficiency selection is old biology.”
In ecology, animals maximize gain per unit cost/time (optimal foraging; marginal value theorem). That’s the same ratio form your 
𝑈
𝑖
U
i
	​

 uses. 
paulseabright.com
Gwern

“Bounded rationality as free-energy / information trade-off.”
In control and decision theory, KL-control / linearly-solvable MDPs and information-theoretic bounded rationality pick policies by maximizing expected utility minus a resource cost, i.e., a free-energy-like functional—another articulation of “value per cost.” 
papers.neurips.cc
PNAS
arXiv

“Costs are real/metabolic.”
Neural signaling is expensive; cortical computation is energy-limited. The biophysics supports treating cost 
𝐶
𝑖
C
i
	​

 as something the system must price. 
PubMed
+1

Ratio vs. difference (why your 
𝐵
/
𝐶
B/C matches classic “benefit – λ·cost”)

Many frameworks maximize benefit – λ·cost rather than a ratio. Under a resource (time/energy) constraint, the two are equivalent at the optimum: choosing controller 
𝑖
i by largest 
𝐵
𝑖
/
𝐶
𝑖
B
i
	​

/C
i
	​

 is identical to choosing by 
𝐵
𝑖
−
𝜆
𝐶
𝑖
B
i
	​

−λC
i
	​

 for some shadow price 
𝜆
λ (the current opportunity cost of time/energy). In the brain, tonic DA ≈ 
𝜆
λ (average reward rate), which converts the ratio into the familiar difference form used by dACC/EVC. Empirically, manipulating DA or the opportunity cost of time tilts choices exactly as that duality predicts. 
PubMed
bioRxiv
PMC

Minimal biological wiring diagram

Computation of the numerator (discounted value): vmPFC/VS encode subjective (discounted) value; hippocampus/PFC supply model-based predictions (where applicable); active-inference accounts formalize this as expected free energy over horizon 
𝐻
(
𝑡
)
H(t). 
PubMed
+1
Chris Mathys

Computation of the denominator (cost): dACC/EVC and striatum integrate effort/time costs; DA implements the opportunity-cost price; NE (LC) sets adaptive gain/precision (explore–exploit), changing effective costs/benefits. 
PMC
PubMed
+1

Selection/gating: dACC signals control demand; BG and STN gate the chosen policy; when deliberation time has a cost, behavior follows standard bounded-accumulation with explicit time-penalties. 
PMC
+1
jneurosci.org

Bottom line

The form “discounted future goodness over present cost” is not novel: it’s the intersection of active inference’s expected free energy, reward-rate/effort accounts (DA, EVC), optimal foraging’s rate maximization, and information-theoretic bounded rationality.

What is yours is the specific packaging: using 
Δ
F
E
ΔFE as the benefit signal across controllers and making the ratio explicit. But every “LEGO brick” of that package already exists—and has neural support.

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
