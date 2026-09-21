# MASTER PROMPT — BO CDA 201: Statistics for Data Science → Complete Exam-Prep DOCX

> Paste this whole file into a fresh Claude Code session whose working directory is
> `/Users/aditya/STUDY/IIT_Patna_NOTES/SEMESTER_3`.

---

## 0. ROLE AND GOAL

You are an expert professor of mathematical statistics and an exam coach for Indian CBT (computer-based, MCQ-only) examinations.
Your student is **Aditya**, a 3rd-semester **B.S. Computer Science & Data Analytics (CSDA)** student at **IIT Patna**.

Produce **one complete, self-sufficient Word document** (`CDA201_Statistics_Exam_Guide.docx`) for **BO CDA 201 — Statistics for Data Science (L-T-P-C 3-1-2-5)**.
After studying this guide, Aditya should not need to open any other book, slide or note to score top marks.
The exam is **CBT, MCQ-only**, so the guide must teach every concept from first principles **and** train speed, accuracy and trap-detection with a very large bank of MCQs.

**Non-negotiables**
1. **Follow the official syllabus order exactly (Section 2).** Do not reorder, merge away or skip any topic. Every syllabus phrase must appear as a heading or sub-heading.
2. **Be mathematically correct.** Verify every derivation, and check every numeric answer and table value **with Python code** before writing it (Section 8).
3. **Use the professor's notation and examples first** (Section 3), then extend with textbook depth.
4. **Correct, don't copy, the errors** listed in Section 4.
5. **Read the source files yourself** (Section 1) before writing. The digest in Section 3 is a guide, not a substitute; if you find something in the files that the digest missed, include it.
6. **Build in stages:** one syllabus unit per turn, appended to the same .docx. After each unit, report its page count and MCQ count, then wait for the user to say "continue".

---

## 1. SOURCE FILES (read these first, in this order)

**Official syllabus:** `s-2.png` (left half, in the SEMESTER_3 root) and `/Users/aditya/Downloads/CDA 201 (1).pdf` and the text below. Curriculum table: `s-main.png`.

**Professor's lecture notes (handwritten, scanned — render the pages to images and read every page):**
`201/Lecture Notes-20260921/Lectures/Lecture-1.pdf … Lecture-7.pdf` (27 Aug → 19 Sep 2026)

**Lab / tutorial material:**
- `201/Lecture Notes-20260921/Lab Tutorial/Lab Assignment 1.pdf` (Python recap)
- `CDA201_Lab1.ipynb` (dice, guess-the-number, rock-paper-scissors, password strength)
- `CDA201_Lab2.ipynb` (Binomial, Poisson and Geometric PMF/CDF by hand and with scipy; mean/variance)
- `CDA201_Lab3.ipynb` (Normal PDF/CDF via erf, Exponential, Uniform; plotting)
- `Lab Tutorial/CDF_Normal_Dist.pdf`: the erf derivation, CDF(y) = 0.5 + 0.5·erf((y−μ)/(√2σ)), CDF(−y) = 0.5 − 0.5·erf(·)

**Statistical tables given by the professor:** `T-table.pdf`, `Chi square table.pdf`, `F-table.pdf`. The exam will likely expect you to read these tables.

**Textbooks (syllabus Learning Resources):**
1. `201/Kandethody M.Ramachandran, Chris P.Tsokos - Mathematical Statistics with Applications (2009…).pdf` — **primary**
   - Ch 4 Sampling Distributions · Ch 5 Point Estimation · Ch 6 Interval Estimation · Ch 7 Hypothesis Testing · Ch 11 Bayesian Estimation and Inference · App. IV Tables
2. `201/William W. Hines, Douglas C. Montgomery, … Probability and Statistics in Engineering (2003, Wiley).pdf` (scanned)
   - Ch 9 Random Samples and Sampling Distributions · Ch 10 Parameter Estimation · Ch 11 Tests of Hypotheses · Appendix tables
3. `201/[What's New in Statistics] Robert V. Hogg, … Introduction to Mathematical Statistics (2018, Pearson).pdf`
   (this file is the **8th ed. 2018**; the syllabus lists the 7th ed., and the content is equivalent)
   - 3.3 Γ, χ², β · 3.6 t and F · 4.1 Sampling & statistics · 4.2 Confidence intervals · 4.4 **Order statistics** · 4.5–4.7 Testing & χ² tests · 5.1 Convergence in probability · 5.3 CLT · 6.1 MLE · 6.2 **Rao–Cramér bound & efficiency** · 6.3 ML (likelihood-ratio) tests · 7.1 Quality of estimators (MSE) · 7.2 **Sufficient statistic / factorization** · 7.3 Properties (**Rao–Blackwell**) · 7.4 Completeness (**Lehmann–Scheffé**) · 7.5 Exponential class · 8.1 **Neyman–Pearson** most powerful tests · 8.2 UMP · 8.3 Likelihood-ratio tests · 11.1–11.2 **Bayesian** · App. A.1 Regularity conditions
- Extra books the professor supplied (`Lecture Notes-20260921/Books/`):
  - `Book-1.pdf` = S.C. Gupta & V.K. Kapoor, *Fundamentals of Mathematical Statistics*: Ch 13 χ², Ch 14 t, F, Z, Ch 15 Theory of Estimation, Ch 16 Testing of Hypothesis. **This is a great source of objective-type questions.**
  - `Book-2.pdf` = Prasanna Sahoo, *Probability and Mathematical Statistics*: Ch 13 Order statistics, Ch 14 Sampling distributions (normal), Ch 15 Point estimators, Ch 16 Criteria for evaluating estimators, Ch 17 Interval estimators, Ch 18 Tests of hypotheses. **Many solved problems are in actuarial-exam MCQ style, so mine them.**
  - `Book-3.pdf` = Casella & Berger, *Statistical Inference* (scanned, no text layer): 5.4 Order statistics, 6.2 Sufficiency, 7 Point estimation, 8 Hypothesis testing, 9 Interval estimation.

---

## 2. OFFICIAL SYLLABUS — THE ORDER OF THE DOCUMENT (do not change)

**Unit 0 (Foundations, prerequisite; taught in Lecture 1):** probability recap (random variable as a function S→ℝ, CDF properties, PDF/PMF, expectation, variance, MGF, iid) plus the standard distributions used later (Bernoulli, Binomial, Poisson, Geometric, Uniform, Exponential, Normal, Gamma, Beta), with mean, variance and MGF in one master table. Include the Python lab content here.

**Unit 1: Ordered Statistics**
1.1 Definition and notation X₍₁₎ ≤ … ≤ X₍ₙ₎ · 1.2 Distribution of the maximum X₍ₙ₎ · 1.3 Distribution of the minimum X₍₁₎ · 1.4 Distribution of the k-th order statistic (CDF via Binomial and the PDF) · 1.5 Joint distribution of X₍ᵢ₎, X₍ⱼ₎ and of all order statistics · 1.6 **Probability distribution of the sample range** R = X₍ₙ₎ − X₍₁₎ (and midrange) · 1.7 Uniform-case results (X₍ⱼ₎ ~ Beta(j, n−j+1), mean j/(n+1), variance) · 1.8 Applications (median, "top-10 students", reliability of series/parallel systems)

**Unit 2: Random Sampling and Sampling Distributions**
2.1 Population, sample, random sample, statistic vs. parameter, sampling distribution · 2.2 Distribution of X̄ (normal case; CLT) · 2.3 Gamma function and Gamma distribution · 2.4 **Chi-square distribution** (pdf, mean, variance, MGF, additivity, Z² ~ χ²₁, Σ Zᵢ² ~ χ²ₙ, (n−1)S²/σ² ~ χ²ₙ₋₁, independence of X̄ and S²) · 2.5 **Student's t** (definition, pdf, symmetry, moments, √n(X̄−μ)/S ~ tₙ₋₁, tₙ → N(0,1), t₁ = Cauchy) · 2.6 **F distribution** (definition, pdf, mean, variance, 1/F, T² ~ F₁,ₙ, variance-ratio result, F₍ₘ,ₙ,₁₋α₎ = 1/F₍ₙ,ₘ,α₎) · 2.7 **Reading χ², t and F tables** (upper-α percentiles; left-tail conversions), plus sample-size problems via the CLT

**Unit 3: Point Estimation**
3.1 Estimator vs. estimate, parameter space · 3.2 **Sufficiency** · 3.3 **Factorization theorem** (Neyman–Fisher) · 3.4 **Consistency** (convergence in probability, Chebyshev, the sufficient condition E→θ and Var→0) · 3.5 **Method of moments** · 3.6 **Unbiased estimation** (bias; existence and non-existence; unreasonable unbiased estimators) · 3.7 **Minimum-Variance Unbiased Estimator (MVUE/UMVUE)** and its properties (uniqueness) · 3.8 **Fisher information** (both forms, additivity nI(θ)) · 3.9 **Rao–Cramér lower bound** (regularity conditions, efficiency, the 4-step recipe) · 3.10 **Rao–Blackwellization** (plus completeness and Lehmann–Scheffé) · 3.11 **Maximum Likelihood Estimator** and its properties (invariance, consistency, asymptotic normality/efficiency; boundary cases like U(0, θ)) · 3.12 **Criteria for evaluating estimators: Mean Squared Error** (MSE = Var + Bias², relative efficiency)

**Unit 4: Interval Estimation**
4.1 Coverage probability and confidence level (frequentist interpretation) · 4.2 **Pivotal quantities** · 4.3 Interval estimators for various distributions: normal mean (σ known → z; unknown → t), normal variance (χ²), difference of means (known σ, pooled t, Welch), ratio of variances (F), proportion (Wald, and Wilson for awareness), difference of proportions, exponential/Poisson parameter via χ², one-sided bounds · 4.4 **Sample-size determination** (mean and proportion; the p = 0.5 worst case) · 4.5 **Shortest-length interval** (symmetric for symmetric pivots; the unequal-tail χ² case)

**Unit 5: Testing of Hypotheses**
5.1 Null vs. alternative; **simple vs. composite** · 5.2 Test statistic, **critical region**, acceptance region · 5.3 **Error probabilities** (Type I α, Type II β), **power function**, **level vs. size of a test**, p-value · 5.4 **Neyman–Pearson lemma** (MP tests, worked derivations for Normal, Exponential, Poisson and Bernoulli) plus UMP via monotone likelihood ratio · 5.5 **One- and two-sided tests for the mean, variance and proportions** · 5.6 **One-sample and two-sample t-test**, **pooled t-test**, **paired t-test** · 5.7 **Chi-square test** (variance test; goodness of fit) · 5.8 **Contingency-table test** (independence; df = (r−1)(c−1); expected counts ≥ 5 rule) · 5.9 **Maximum likelihood (likelihood-ratio) test** (Λ, −2 log Λ ~ χ², Wilks) · 5.10 **Duality between confidence intervals and tests**

**Unit 6: Bayesian Estimation**
6.1 Prior, likelihood, **posterior** (∝ likelihood × prior) · 6.2 Conjugate priors · 6.3 Loss functions; **quadratic (squared-error) loss** ⇒ Bayes estimate = **posterior mean** (absolute loss → posterior median; 0-1 loss → posterior mode, for comparison) · 6.4 **Bayes estimates for well-known distributions:** Binomial–Beta, Poisson–Gamma, Exponential–Gamma, Normal–Normal (σ² known), Gamma rate–Gamma, Geometric–Beta; show each posterior mean as a weighted average of the prior mean and the MLE · 6.5 Credible intervals vs. confidence intervals

**Practical component (R/Python):** fold into every unit a "🖥 Lab Corner" with short, runnable Python (numpy/scipy) snippets that compute that unit's quantities: order-statistic simulation, `scipy.stats.chi2/t/f.ppf`, MLE by `scipy.optimize`, CIs, `ttest_ind/ttest_rel`, `chi2_contingency`, Beta–Binomial posterior updates.

---

## 3. WHAT THE PROFESSOR ACTUALLY TAUGHT (digest of Lectures 1–7). Reproduce these examples as "Professor's Example" boxes.

**L1 (27 Aug 2026):** "Statistics is the science of collecting, organising and analysing data." RV X: S→ℝ with the example "2 coins, X = #heads, S = {HH, HT, TH, TT}, X(S) = {0, 1, 2}". CDF properties: (1) 0 ≤ F ≤ 1, (2) non-decreasing, (3) F(−∞) = 0 and F(∞) = 1, (4) right-continuous F(x+h) → F(x) as h → 0⁺, (5) P(X > x) = 1 − F(x). PDF f = dF/dx ≥ 0 with ∫f = 1. IID = independent (joint = product) + identical. Order statistics = sample values in ascending order. Heights example: X₁ = 65, X₂ = 74, X₃ = 59, X₄ = 72 → X₍₁₎ = 59, X₍₂₎ = 65, X₍₃₎ = 72, X₍₄₎ = 74. "X₍ₖ₎ can differ from Xₖ": 10, 9, 7, 11 gives X₃ = 7 but X₍₃₎ = 10. Top-10 students = X₍ₙ₎, …, X₍ₙ₋₉₎.
- Max: F_{X(n)}(y) = [F(y)]ⁿ, f = n[F(y)]ⁿ⁻¹ f(y). **Q1:** five U[0,1] RVs → X₍₅₎ has CDF x⁵ and pdf 5x⁴.
- Min: F_{X(1)} = 1 − [1−F]ⁿ, f = n[1−F]ⁿ⁻¹ f. **Q2:** CDF 1 − (1−x)⁵, pdf 5(1−x)⁴.
- k-th: F_{X(k)}(y) = Σ_{j=k}^{n} C(n,j) Fʲ(1−F)ⁿ⁻ʲ ("at least k of the Xᵢ ≤ y", which is binomial). pdf as written by the professor: C(n,k)·k·F^{k−1}(1−F)^{n−k} f, which equals n!/((k−1)!(n−k)!) F^{k−1}(1−F)^{n−k} f. Show both forms and prove they are equal. **Q3:** X₍₃₎ of five U[0,1] RVs: F = 10x³(1−x)² + 5x⁴(1−x) + x⁵ = 5x³(1−x)(2−x) + x⁵.

**L2 (29 Aug):** Joint CDF of (X₍₁₎, X₍ₙ₎): [F(y)]ⁿ if x ≥ y; [F(y)]ⁿ − [F(y) − F(x)]ⁿ if x < y. Joint pdf n(n−1)[F(y) − F(x)]ⁿ⁻² f(x)f(y). Joint pdf of X₍ᵢ₎, X₍ⱼ₎: n!/((i−1)!(j−i−1)!(n−j)!) F(x)^{i−1}[F(y)−F(x)]^{j−i−1}[1−F(y)]^{n−j} f(x)f(y). Joint pdf of all order statistics: n! Π f(xᵢ).
**Q1:** n = 4, f = 2x on (0,1): joint pdf of X₍₃₎, X₍₄₎ = 48 x₃⁵ x₄ (verified). **Q2:** n = 15 U[0,1], joint pdf of X₍₁₎, X₍₁₅₎ (left unsolved: solve it). **Q3:** U[0,1] ⇒ X₍ⱼ₎ ~ Beta(j, n−j+1); find the mean and variance (solve it). Sampling basics; statistic examples X̄, S² (with n−1), range X₍ₙ₎ − X₍₁₎. CLT. Gamma(α, λ) = λe^{−λx}(λx)^{α−1}/Γ(α); Gamma(1, λ) = Exp(λ); Γ(1) = 1, Γ(n+1) = nΓ(n), Γ(½) = √π. χ²ₙ = Gamma(n/2, ½). Additivity example χ²₃ + χ²₄ + χ²₂ = χ²₉.

**L3 (4 Sep):** "Why is χ² a sampling distribution?" X ~ N(0,1), Y = X² ⇒ F_Y = 2Φ(√y) − 1 ⇒ Y ~ χ²₁ (uses the Leibniz rule). χ² mean n (using ∫₀^∞ e^{−ax}x^{n−1}dx = Γ(n)/aⁿ), variance 2n, MGF (1−2t)^{−n/2}. X̄ ~ N(μ, σ²/n) via MGF. Thm 3: X̄ is independent of (Xᵢ − X̄) (jointly normal plus zero covariance). Thm 4: X̄ and S² are independent (n = 2 illustration: S² = (X₁−X₂)²/2 and Cov(X₁+X₂, X₁−X₂) = 0). Thm 5: (n−1)S²/σ² ~ χ²ₙ₋₁ via Σ(Xᵢ−μ)² = Σ(Xᵢ−X̄)² + n(X̄−μ)². Hence E(S²) = σ².
**Ex:** Xᵢ ~ N(50, 16), n = 64: P(49 < X₈ < 51) = P(−¼ < Z < ¼) = 0.1974; P(49 < X̄ < 51) = P(−2 < Z < 2) = 0.9544.

**L4 (5 Sep):** Var(S²) = 2σ⁴/(n−1). Chi-square tables stop at n = 30 (beyond that, use the normal approximation). Upper-α percentile χ²_{α,n}: P(X > χ²_{α,n}) = α, which is the (1−α)-quantile. χ²_{0.05,10} = 18.307. Drill answers: χ²_{.025,10} = 20.483, χ²_{.025,15} = 27.488, χ²_{.01,7} = 18.475, χ²_{.05,24} = 36.415, χ²_{.005,10} = 25.188, χ²_{.005,5} = 16.750. **Q:** P(32.007 < X < χ²_{α,23}) = 0.09 → α = 0.01, value 41.638. **Q:** P(χ²_{α,10} < X < 23.209) = 0.015 → α = 0.025, value 20.483. t-distribution: T = Z/√(Y/n); pdf; t₁ = Cauchy 1/(π(1+t²)); symmetric; odd moments 0; E(Tᵏ) formula; Var = n/(n−2) for n > 2; √n(X̄−μ)/S ~ tₙ₋₁; tₙ → N(0,1) by the WLLN (χ²ₙ/n →ᵖ 1).

**L5 (11 Sep):** −t_{α,n} = t_{1−α,n}. t_{0.05,15} = 1.753, t_{.025,15} = 2.131, t_{.025,14} = 2.145, −t_{.10,15} = −1.341, −t_{.10,14} = −1.345, t_{0.995,7} = −3.499/−3.500. P(T₁₀ < 1.812) = 0.95; P(−1.345 < T₁₄ < 2.624) ≈ 0.89; P(−1.356 < T₁₂ < 2.179) ≈ 0.875; P(1.725 < T₂₀ < 2.845) ≈ 0.045; for T ~ t₁₉, P(|T| ≤ c) = 0.95 ⇒ c = 2.093. F distribution: pdf, positively skewed, E = n/(n−2), Var = 2n²(m+n−2)/(m(n−2)²(n−4)), T² ~ F₁,ₙ, (S₁²/σ₁²)/(S₂²/σ₂²) ~ F_{m−1,n−1}, pooled two-sample t_{m+n−2}, 1/U ~ F_{n,m}. Table drill: F_{7,15,.05} = 2.71, F_{15,15,.05} = 2.40, F_{10,6,.05} = 4.06, F_{4,9,.05} = 3.63, F_{15,7,.05} = 3.51, plus 0.01-level values for (24,19), (15,24), (9,9), (10,15), (15,10): compute and verify them.
**Q:** m = 21, n = 8, σ₁² = 8, σ₂² = 12: P(S₁²/S₂² > 2.3) = P(F₂₀,₇ > 3.45) = 0.05. **Q:** two samples of size 5, equal variances, P(larger/smaller > 3) = 1 − P(⅓ ≤ F₄,₄ ≤ 3) with pdf 6u/(1+u)⁴ gives **0.3125** (verified). **Q (bulb):** σ = 100 h, P(|X̄ − μ| ≤ 50) ≥ 0.95 ⇒ √n/2 ≥ 1.96 ⇒ n ≥ 15.37 ⇒ **n = 16**.

**L6 (12 Sep):** Recap. Tree diagram: Statistical inference → Estimation (point/interval) and Hypothesis testing. Least squares min Σ(yᵢ − a − bxᵢ)² ("do yourself": derive the normal equations). Parameter vs. statistic. Estimator T(X) vs. estimate T(x) (heights of 100 people, x̄ = 160 cm). Parameter space (N: ℝ×ℝ⁺; Poisson: ℝ⁺). Three methods: unbiased, MLE, MoM. Unbiasedness and bias (b > 0 overestimates, b < 0 underestimates). Bin(n,p): X/n is unbiased for p; X(X−1)/(n(n−1)) is unbiased for p²; X(n−X)/(n−1) is unbiased for np(1−p). Normal: X̄ and S² are unbiased (full proof of E(S²) = σ²). Poisson: X̄, X₁ and (X₁+X₂)/2 are all unbiased, so there are many. Bin: **no unbiased estimator of p^{n+1}** (polynomial-degree argument).

**L7 (18–19 Sep):** Poisson: T = I(X₁ = 0) is unbiased for e^{−λ}; (−2)^X is unbiased for e^{−3λ} but absurd (takes the values 1, −2, 4, −8…). Consistency and convergence in probability. U(0, θ): X₍ₙ₎ is consistent (P(X₍ₙ₎ < θ−ε) = ((θ−ε)/θ)ⁿ → 0). Thm: E Tₙ → θ and Var Tₙ → 0 ⇒ consistent (Chebyshev). X̄ is consistent for μ; S² is consistent (Var = 2σ⁴/(n−1)). MSE = Var + Bias². Poisson: T₁ = X₁ (var λ), T₂ = (X₁+X₂)/2 (λ/2), T₃ = X̄ (λ/n). UMVUE definition and uniqueness. T₁ = X̄ and T₂ = 2/(n(n+1)) Σ iXᵢ are both unbiased and consistent (compare their variances). Two routes to the UMVUE: the **CRLB route** vs. **Rao–Blackwell–Lehmann–Scheffé** (sufficiency + completeness). CR inequality: Var T ≥ [ψ′(θ)]²/(nI(θ)); I(θ) = E[(∂/∂θ log f)²] = −E[∂²/∂θ² log f]. Regularity conditions (open Θ, derivative exists, **support independent of θ** (fails for U(0, θ)), uniform convergence, 0 < I < ∞). Recipe: (1) compute I, (2) compute the CRLB, (3) guess an unbiased estimator, (4) if its variance equals the CRLB, it is the UMVUE. Bin(m,p): I = m/(p(1−p)), CRLB = p(1−p)/m, so X/m is the UMVUE. Exercises: Poisson → X̄; N(μ, σ²): μ with σ² known → X̄; σ² with μ known → (1/n)Σ(Xᵢ−μ)²; Exp(mean θ): I = 1/θ², CRLB = θ²/n, X̄ is the UMVUE.

**Not yet taught as of 21 Sep 2026 (take these from the textbooks and teach them just as thoroughly):** sufficiency/factorization, Rao–Blackwell, completeness, MLE, MoM, the sample range distribution, all of interval estimation, all of testing, and all of Bayesian estimation.

---

## 4. ERRATA — the correct versions must appear in the guide (add a "⚠ Common Error" box where relevant)

1. L7, Exponential Fisher information: one line of the notes reads "I(θ) = θ²". **Correct: I(θ) = 1/θ²**, so CRLB = θ²/n.
2. The k-th order-statistic pdf written as "C(n,k)·k·…" is correct but non-standard. Present the standard n!/((k−1)!(n−k)!) form and show they are equal.
3. The F variance was written as n²(2m+2n−4)/(m(n−2)²(n−4)). It is equal to the standard 2n²(m+n−2)/(m(n−2)²(n−4)); state the standard form and the condition n > 4.
4. The notes label t_{0.995,7} as −3.500; the tabulated t_{0.005,7} = 3.499, so write −3.499 (≈ −3.50).
5. Make sure the guide says "**(n−1)S²/σ²** ~ χ²ₙ₋₁" (not nS²/σ²) whenever S² uses the n−1 divisor; also define the n-divisor version and its bias.

---

## 5. REQUIRED STRUCTURE OF THE DOCUMENT

**Front matter**
1. Title page: "BO CDA 201 — Statistics for Data Science | Complete CBT Exam Guide | IIT Patna · B.S. CSDA · Semester 3 · Prepared for Aditya", date.
2. "How to use this guide": a 1-page study plan (7-day and 3-day revision plans) and a legend of the box icons.
3. Syllabus-coverage matrix: every syllabus phrase → section number → lecture date (or "from textbook") → number of MCQs.
4. Automatic Table of Contents (Heading 1–3).
5. "Exam Pattern & Strategy": CBT MCQ types (single-correct, multiple-correct, numerical-answer, assertion–reason, match-the-following), negative-marking strategy, time per question, calculator/table-reading tips.

**For EVERY unit and sub-topic, use this exact template (in this order):**
1. **🎯 Learning Objectives** (3–6 bullets)
2. **📖 Intuition First**: a plain-language explanation and a real-data-science motivation (A/B tests, ML model evaluation, quality control)
3. **📐 Definitions & Notation** (boxed; the professor's notation)
4. **🧮 Key Formulas / Theorems**: boxed; each with conditions and "when to use"
5. **🔍 Derivation / Proof**: step-by-step, every algebra step shown (the exam can ask about intermediate steps)
6. **👨‍🏫 Professor's Example(s)**: from Section 3, fully solved
7. **✍ Worked Examples**: at least 4 per sub-topic, graded easy → hard, including at least 1 table-reading and 1 "trap" example
8. **📊 Diagrams / Flowcharts / Tables** (see Section 6)
9. **⚠ Common Mistakes & MCQ Traps** (at least 5 per unit)
10. **🧠 Memory Hooks / Shortcuts** (mnemonics, quick-check rules, e.g., "χ² mean = df, variance = 2·df")
11. **🖥 Lab Corner** (short, verified Python)
12. **📝 Practice Exercises** (descriptive/numerical, 8–12 per unit, with full solutions at the end of the unit)
13. **✅ MCQ Practice Set**: at least **40 MCQs per unit** (Unit 0: 25), mixed types: ~60% single-correct, 15% multiple-correct, 15% numerical-answer (NAT), 10% assertion–reason / match. Difficulty tags [E]/[M]/[H]. Include ≥ 30% calculation-based and ≥ 20% table-lookup questions.
14. **🔑 Answer Key with 1–3-line explanations**, including why each wrong option is wrong where that's instructive
15. **📌 One-Page Unit Summary** (to revise the night before)

**Back matter**
- **Master Formula Sheet** (4–6 pages): every distribution (pmf/pdf, support, mean, var, MGF), every pivot, every CI, every test statistic with its df, every Bayes conjugate pair
- **Decision flowchart: "Which test / which interval do I use?"** (by parameter × number of samples × σ known/unknown × paired/independent)
- **Statistical Tables Appendix**: regenerate the z, t, χ² and F tables with scipy (same layout as the professor's PDFs) plus a "How to read each table" guide with 5 worked lookups each
- **3 Full-Length Mock CBT Tests** (60 MCQs each, syllabus-weighted, timed guidance, answer key and explanations)
- **Last-24-hours revision list**, and an index of terms

---

## 6. DIAGRAMS, FLOWCHARTS AND TABLES THAT MUST APPEAR (generate them as PNG images at ≥ 200 dpi and embed them)

- Random variable as a mapping S→ℝ (2-coin example); CDF step-function vs. continuous CDF
- Order statistics: sorting number-line diagram; pdf plots of X₍₁₎, X₍₃₎, X₍₅₎ for n = 5 Uniform; the "trinomial splitting" diagram behind the joint pdf of X₍ᵢ₎, X₍ⱼ₎; the region diagram for the joint CDF of (X₍₁₎, X₍ₙ₎)
- Population → samples → statistic → sampling-distribution flow diagram
- Family-tree diagram: Normal → χ² → t, F; Gamma ↔ Exponential ↔ χ²; Beta ↔ Uniform order statistics
- Overlaid pdfs of χ² (df 1, 2, 5, 10), t (df 1, 5, 30 vs. N(0,1)) and F (several df); shaded upper-α tail diagrams for table reading
- Inference tree (Estimation → Point / Interval; Hypothesis testing), matching the professor's L6 diagram
- Bias–variance "dartboard" diagram; MSE decomposition bar chart
- Estimator-properties map: unbiased / consistent / efficient / sufficient / complete → UMVUE (CRLB route vs. RBLS route flowchart)
- Likelihood-function plots with the MLE marked (Bernoulli, Poisson, and the U(0, θ) boundary case)
- CI coverage simulation plot (100 intervals, some missing μ)
- Hypothesis-testing diagram: H₀ and H₁ distributions with α, β and power shaded; a power-function curve; one-tailed vs. two-tailed critical regions
- Neyman–Pearson flowchart (likelihood ratio → critical region → size α)
- Test-selection decision flowchart (z / t / pooled t / paired t / Welch / χ² / F / proportion / contingency)
- CI–test duality diagram
- Bayesian update diagrams: prior × likelihood → posterior (Beta–Binomial and Normal–Normal)
- Tables: distribution master table; test summary table (H₀, statistic, distribution, df, rejection rule); CI summary table; conjugate prior table; estimator comparison table

Make diagrams with **graphviz (`dot` is installed)** for flowcharts and trees, and with **matplotlib** for plots. Install packages if missing:
`pip install matplotlib scipy numpy python-docx`

---

## 7. DOCX BUILD SPECIFICATIONS

- Build with **python-docx** from a Python script (keep the script as `build_cda201_guide.py` next to the output so it can be re-run). Save to `/Users/aditya/STUDY/IIT_Patna_NOTES/SEMESTER_3/201/CDA201_Statistics_Exam_Guide.docx`.
- Page: A4, 2 cm margins, page numbers in the footer, running header "CDA 201 · Statistics for Data Science".
- Styles: Title; Heading 1 (Unit), Heading 2 (sub-topic), Heading 3; Normal 11 pt (Calibri or Cambria); code in Consolas 9.5 pt with a light-grey shading.
- **Coloured call-out boxes** (1-cell tables with shading): Definition (blue), Theorem/Formula (green), Professor's Example (purple), Warning/Trap (red), Memory Hook (yellow), Lab (grey).
- **Equations:** write them as native Word equations (OMML), for example by converting LaTeX → MathML → OMML (`latex2mathml` + Microsoft's `MML2OMML.XSL` if available). If that fails, render the LaTeX with matplotlib mathtext as a high-dpi PNG inline image. Never leave raw LaTeX in the document.
- Insert a real **TOC field** (tell the user to press F9 / "Update field" once when they open the file).
- MCQs: number them continuously per unit (e.g., Q3.27); put options (A)–(D) on separate lines; keep the answer keys at the end of each unit, not next to the questions.
- Target length: roughly **250–350 pages** in total, and **≥ 280 MCQs** plus 3 × 60 mock MCQs.

---

## 8. QUALITY-ASSURANCE CHECKLIST (run before declaring a unit done)

- [ ] Every syllabus phrase of the unit is present as a heading (tick it in the coverage matrix).
- [ ] Every professor example from Section 3 for that unit is included and solved.
- [ ] Every numeric answer (in examples, exercises, MCQs and mocks) was **recomputed in Python** (scipy.stats for table values; symbolic checks with sympy for derivations where feasible). Keep a `qa_log_unitX.txt`.
- [ ] Each MCQ has exactly one correct option (or the stated number for multiple-correct questions), the distractors are plausible (typical student errors: n vs. n−1, one- vs. two-tailed, upper vs. lower percentile, forgetting to square σ), and the answer-key letters are balanced (no long runs of the same letter).
- [ ] No errata from Section 4 is repeated.
- [ ] All images render; the TOC field exists; the document opens without errors (re-open with python-docx to confirm).
- [ ] Report back: pages added, MCQs added, figures added, and any source ambiguity you resolved.

**Start now:** list the files you are reading, read them, and confirm the unit plan in one short message. Then build **Front matter + Unit 0**, and stop and wait for "continue".
