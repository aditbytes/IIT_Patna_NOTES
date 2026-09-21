# MASTER PROMPT — BO CDA 205: Machine Learning Techniques → Complete Exam-Prep DOCX

> Paste this whole file into a fresh Claude Code session whose working directory is
> `/Users/aditya/STUDY/IIT_Patna_NOTES/SEMESTER_3`.

---

## 0. ROLE AND GOAL

You are an expert machine-learning professor (at the level of Bishop, Hastie–Tibshirani–Friedman and Mitchell) and an exam coach for Indian CBT (MCQ-only) exams.
Your student is **Aditya**, a 3rd-semester **B.S. Computer Science & Data Analytics (CSDA)** student at **IIT Patna**.

Produce **one complete, self-sufficient Word document** (`CDA205_ML_Exam_Guide.docx`) for **BO CDA 205 — Machine Learning Techniques (L-T-P-C 3-1-2-5)**.
After studying this guide, Aditya should not need any other source. The exam is **CBT, MCQ-only**, so the guide must teach each technique from scratch
(intuition → maths → algorithm → hand-computed numeric example → Python) **and** drill it with a very large bank of MCQs,
especially **hand-calculation MCQs** (distances, entropy/Gini, posteriors, PCA eigen-steps, k-means iterations, Viterbi paths, silhouette values…).

**Non-negotiables**
1. **Follow the official syllabus order exactly (Section 2)**; every syllabus phrase must appear as a heading. Tag each section 🏫 *Taught in class* or 📚 *From textbook*.
2. **Be correct:** every numeric example and MCQ answer must be recomputed in Python (numpy/scikit-learn) and match the hand calculation (Section 8).
3. **Use the professor's datasets, examples and step lists first** (Section 3). **Reuse the lab-assignment datasets as fully solved examples.**
4. **Correct, don't copy, the errors** in Section 4.
5. **Read every source file yourself** (Section 1) first; include anything the digest missed.
6. **Build in stages:** one unit per turn, appended to the same .docx; after each unit, report pages and MCQs, then wait for "continue".

---

## 1. SOURCE FILES (read these first)

- **Official syllabus:** `205/CDA 205.pdf`, `S-1.png` (left half).
- **Professor's slides:** `205/Lecture Notes-20260921 (2)/1 Intro kNN- Naive Bayes.pdf` (26 slides; slides 7–14 are image-only, so render and view them).
- **Assignments (docx):** `Lab-Assignment-1.docx` (kNN), `Lab-Assignment-2.docx` (Naive Bayes), `Assignment -DecisionTreeClassifier.docx` (Gini).
- **Supplementary text given by the professor:** `statquest-illustrated-guide-to-machine-learning.pdf` (Josh Starmer; image-based, so render pages). Chapters: 01 Fundamentals, 02 Cross-validation, 03 Stats fundamentals, 04 Linear regression, 05 Gradient descent, 06 Logistic regression, 07 **Naive Bayes**, 08 **Assessing model performance** (confusion matrix, sensitivity/specificity, ROC/AUC), 09 Regularization, 10 **Decision trees**, 11 **SVC & SVM**, 12 Neural networks. Borrow its intuitive, picture-first explanation style.
- **Textbooks:**
  1. `205/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf`: 1.5 Decision theory · 1.6 Information theory (entropy, KL) · 2.5.2 Nearest-neighbour methods · 4.1.4–4.1.6 **Fisher's linear discriminant** · 4.2 Probabilistic generative models (LDA/QDA with Gaussian class-conditionals) · 7.1 **Maximum-margin classifiers (SVM)**, 7.1.1 overlapping classes (soft margin) · 6.1–6.2 kernels · 8 Graphical models (8.1 Bayesian networks, 8.3 Markov random fields: background for CRF) · 9.1 **K-means** · 9.2 **Mixtures of Gaussians** · 9.3–9.4 **EM** · 12.1 **PCA** · 13.1–13.2 **Markov models & HMM** (forward–backward, Viterbi/max-sum, Baum–Welch) · 14.4 tree-based models
  2. `205/Hastie, Tibshirani, Friedman, The Elements of Statistical Learning, 2009.pdf`: 2.3 Least squares vs. nearest neighbours · 3.3 **Subset selection (best subset, forward/backward stepwise)** · 4.3 **Linear Discriminant Analysis** (4.3.3 reduced-rank LDA = LDA as dimensionality reduction) · 4.5 separating hyperplanes · 6.6.3 Naive Bayes · 7 Model assessment (bias–variance, CV) · 9.2 **Tree-based methods (CART)** · 12 **SVMs** · 13 Prototype methods & **k-nearest neighbours** · 14.3 **Cluster analysis** (14.3.6 K-means, 14.3.7 Gaussian mixtures as soft K-means, 14.3.10 **K-medoids**, 14.3.12 **Hierarchical clustering**) · 14.5 **Principal components** · 14.5.3 spectral clustering · 18.6 feature selection in high dimensions
  3. `205/T. Mitchell., Machine Learning, McGraw-Hill, 1997.pdf`: Ch 3 **Decision-tree learning (ID3, entropy, information gain, overfitting, pruning, continuous attributes, gain ratio)** · Ch 5 Evaluating hypotheses · Ch 6 **Bayesian learning (MAP/ML, Bayes optimal, Naive Bayes, the text-classification example, EM 6.12)** · Ch 8 **Instance-based learning (k-NN, distance-weighted, curse of dimensionality)**
- **Topics not in these books** (CRF, MEMM, active learning, semi-supervised learning, topic modelling, DBSCAN, cluster validity indices): cover them from your expert knowledge, following the standard references: Lafferty–McCallum–Pereira 2001 (CRF), McCallum–Freitag–Pereira 2000 (MEMM), Ester et al. 1996 (DBSCAN), Blei–Ng–Jordan 2003 (LDA topic model), Settles 2009 (active-learning survey), Zhu 2005 / Chapelle et al. 2006 (semi-supervised), Rousseeuw 1987 (silhouette), Davies–Bouldin 1979, Dunn 1973.
- **Practical component (R/Python):** every unit gets a "🖥 Lab Corner" with from-scratch numpy code **and** the scikit-learn equivalent (the professor's labs require "without using scikit-learn" versions).

---

## 2. OFFICIAL SYLLABUS — THE ORDER OF THE DOCUMENT (do not change)

**Unit 0: Introduction & ML Foundations** (🏫 slides 1–19): AI vs. DS vs. DA vs. ML vs. DL (the professor's definitions and the Venn diagram); scope of DS/AI; algorithm families; digital data types (structured / semi-structured / unstructured); big-data units (KB→YB) and the 4 V's; supervised vs. unsupervised (the movie Action/Comedy table vs. "how many groups?"); train/validation/test splits (80/20, the split-size table); overfitting vs. underfitting, bias–variance; **evaluation metrics** (confusion matrix, accuracy, precision, recall, F1, specificity, ROC/AUC) and **cross-validation** (k-fold, LOOCV); feature scaling (min-max, z-score); distance and similarity refresher (Euclidean, Manhattan, Minkowski, cosine). *These are needed by every later unit.*

**Unit 1: Supervised Learning**
- 1.1 **Decision trees**: entropy, information gain (ID3), gain ratio (C4.5), **Gini impurity & Gini gain (CART)**, the tree-building algorithm, continuous attributes (threshold search), overfitting, pre/post-pruning, handling missing values; regression trees (variance reduction)
- 1.2 **Nearest-neighbour classifiers**: the kNN algorithm (the professor's 3-step procedure), choosing k (validation, odd k for binary), distance metrics, weighted kNN, kNN regression, feature scaling, curse of dimensionality, complexity
- 1.3 **Generative classifiers like Naive Bayes**: Bayes' theorem, prior/likelihood/evidence/posterior, the conditional-independence assumption, MAP decision rule argmax P(c)ΠP(xᵢ|c), categorical NB, **Laplace smoothing** (the zero-frequency problem), Gaussian NB, multinomial NB for text, log-probabilities (underflow); generative vs. discriminative
- 1.4 **Linear Discriminant Analysis** (as a classifier): Gaussian class-conditionals with a shared covariance ⇒ linear boundary, discriminant function δₖ(x), QDA contrast, a 2-class numeric example
- 1.5 **Support Vector Machines**: hyperplane, functional/geometric margin, hard-margin primal, Lagrangian and dual, support vectors, soft margin with C and hinge loss, the kernel trick (linear, polynomial, RBF), a worked 2-D numeric example (find w, b and the margin width 2/‖w‖), multiclass (OvR/OvO)
- 1.6 **Feature selection techniques: wrapper and filter approaches** (filter: variance threshold, correlation, χ², mutual information, ANOVA-F; wrapper: search plus model evaluation; embedded: Lasso, for comparison)
- 1.7 **Backward selection algorithm** and 1.8 **Forward selection algorithm** (step-by-step pseudocode and a worked trace; stepwise hybrid; complexity vs. best-subset 2ᵖ)
- 1.9 **PCA**: covariance matrix, eigen-decomposition, explained-variance ratio, scree plot, projection/reconstruction, a full hand-worked 2-D example (mean-centre → covariance → eigenvalues → eigenvectors → projected scores), relation to SVD, standardisation
- 1.10 **LDA (Linear Discriminant Analysis for dimensionality reduction)**: Fisher criterion J(w) = wᵀS_Bw / wᵀS_Ww, the solution w ∝ S_W⁻¹(μ₁−μ₂), at most C−1 components, a worked 2-class example, **PCA vs. LDA comparison table** (unsupervised vs. supervised)

**Unit 2: Unsupervised Learning**
- 2.1 **K-means**: Lloyd's algorithm, the objective (WCSS/inertia), a hand-iterated numeric example to convergence, initialisation sensitivity, k-means++, choosing k (elbow, silhouette), limitations
- 2.2 **Hierarchical clustering**: agglomerative vs. divisive; linkage (single, complete, average, Ward); a full distance-matrix update example with the **dendrogram**; cutting the dendrogram
- 2.3 **EM**: latent variables; Gaussian Mixture Models; E-step (responsibilities) and M-step (update π, μ, Σ) formulas; a 1-D two-component worked iteration; log-likelihood monotonicity; K-means as hard-EM
- 2.4 **K-medoids** (PAM: BUILD and SWAP), robustness to outliers vs. K-means, a worked example, CLARA for awareness
- 2.5 **DBSCAN**: ε, MinPts; core/border/noise points; density-reachable/connected; the algorithm; a worked 2-D example; strengths (arbitrary shapes, noise) and weaknesses (varying density); OPTICS/HDBSCAN for awareness
- 2.6 **Cluster validity indices**: internal (**silhouette** coefficient with a worked computation, **Davies–Bouldin**, **Dunn**, Calinski–Harabasz, WCSS/elbow); external (purity, Rand index, adjusted Rand, NMI, Jaccard); relative
- 2.7 **Similarity measures** (Euclidean, Manhattan, Minkowski, Chebyshev, cosine, Jaccard, Hamming, Mahalanobis, correlation; Gower for mixed data; similarity ↔ distance conversion; which measure suits which clustering method). The basic distances are introduced briefly in Unit 0 so that K-means can be taught first, in syllabus order.
- 2.8 **Some modern techniques of clustering**: spectral clustering, mean-shift, Gaussian-mixture/BIRCH, HDBSCAN, affinity propagation, deep clustering (awareness level, with one intuition paragraph and one MCQ-able fact each)

**Unit 3: Graphical Models**
- 3.1 Probabilistic graphical-models primer (Bayesian networks vs. Markov random fields; generative vs. discriminative sequence models)
- 3.2 **HMM**: states, observations, parameters λ = (A, B, π); the three problems: **evaluation (Forward algorithm)**, **decoding (Viterbi)**, **learning (Baum–Welch/EM)**; a full numeric forward and Viterbi trellis example (weather/ice-cream or healthy/fever); assumptions (Markov, output independence)
- 3.3 **MEMM**: P(sᵢ | sᵢ₋₁, oᵢ) with per-state max-ent (logistic) models, feature functions, the **label-bias problem**
- 3.4 **CRF (linear-chain)**: globally normalised P(y|x) = (1/Z(x)) exp(Σ λₖ fₖ(yₜ₋₁, yₜ, x, t)), feature functions, inference (forward–backward / Viterbi), how it fixes label bias; applications (POS tagging, NER)
- 3.5 **HMM vs. MEMM vs. CRF comparison table** (generative/discriminative, local/global normalisation, label bias, features)

**Unit 4: Semi-supervised Learning, Active Learning, Topic Modelling**
- 4.1 **Semi-supervised learning**: assumptions (smoothness, cluster, manifold); self-training, co-training, label propagation, semi-supervised EM, transductive SVM
- 4.2 **Active learning**: pool-based / stream-based / membership query synthesis; query strategies (uncertainty sampling: least-confidence, margin, entropy; query-by-committee with vote entropy; expected model change; expected error reduction), a worked uncertainty-sampling example
- 4.3 **Topic modelling: LDA (Latent Dirichlet Allocation)**: the generative story (α → θ_d, β/η → φ_k, z, w), the plate diagram, Dirichlet intuition, inference (collapsed Gibbs sampling update, variational Bayes overview), outputs (doc-topic and topic-word distributions), perplexity/coherence; **the ⚠ "three LDAs" disambiguation box** (Linear Discriminant Analysis classifier / LDA dimensionality reduction / Latent Dirichlet Allocation)

---

## 3. WHAT THE PROFESSOR ACTUALLY TAUGHT (reproduce as "Professor's Example" boxes)

- **Definitions slide:** AI: "the study of [intelligent] agents that receive percepts from the environment and take action" (Russell & Norvig); Data Science: "the study of data to extract meaningful insights for business" (AWS); ML: "the ability to solve a problem without being explicitly programmed" (Arthur Samuel); DL: "…learn from experience and understand the world in terms of a hierarchy of concepts" (Goodfellow); Data Analytics: "inspecting, cleansing, transforming and modelling data…" (Meta S. Brown). Algorithm lists: DS: K-means, PCA, SVD, Apriori, ICA; AI: neural networks, search & optimisation. Data types: structured (RDBMS, Excel), semi-structured (HTML, XML), unstructured (media, sensors). Big-data units: 1024 KB = 1 MB … 1024 ZB = 1 YB. 4 V's: Volume, Velocity, Variety, Veracity.
- **Movie example:** training T1 (100 action, 15 comedy) Action; T2 (20, 95) Comedy; T3 (90, 5) Action; T4 (10, 85) Comedy; test T5 (50, 50). **kNN steps:** (1) compute the Euclidean distance from the test point to every training point, (2) sort ascending, (3) choose k using validation data (majority vote; odd k for binary; pick the k with the highest validation accuracy, then report test accuracy). Distances: d(T5, T1) = 61.03, d(T5, T2) = 54.08, d(T5, T3) = 60.21, d(T5, T4) = 53.15 → sorted T4, T2, T3, T1 → k = 1: Comedy; k = 2: Comedy, Comedy; k = 3: Comedy, Comedy, Action → **Comedy**. Train/val/test 80/20 schemes; the split-size table (50/50 … 99.5/0.5 for N = 100 … 10⁷).
- **Naive Bayes:** conditional probability → Bayes' theorem → the independence assumption → posterior ∝ likelihood × prior, drop the evidence term, predicted class = argmax P(cᵢ)ΠP(θⱼ|cᵢ). Terms: posterior P(c|θ), prior P(c), likelihood P(θ|c), predictor prior P(θ).
- **The 14-row "Play Golf" dataset** (slide 23; **use it exactly**; note that Rainy and Sunny are swapped relative to the classic Quinlan table):
  1 Rainy Hot High Weak No · 2 Rainy Hot High Strong No · 3 Overcast Hot High Weak Yes · 4 Sunny Mild High Weak Yes · 5 Sunny Cool Normal Weak Yes · 6 Sunny Cool Normal Strong No · 7 Overcast Cool Normal Strong Yes · 8 Rainy Mild High Weak No · 9 Rainy Cool Normal Weak Yes · 10 Sunny Mild Normal Weak Yes · 11 Rainy Mild Normal Strong Yes · 12 Overcast Mild High Strong Yes · 13 Overcast Hot Normal Weak Yes · 14 Sunny Mild High Strong No.
  Frequency tables (slide 24, **correct**): Outlook Sunny 3/2, Overcast 4/0, Rainy 2/3; Temp Hot 2/2, Mild 4/2, Cool 3/1; Humidity High 3/4, Normal 6/1; Wind Weak 6/2, Strong 3/3; P(Yes) = 9/14, P(No) = 5/14.
  Query x = (Sunny, Cool, Normal, Weak): P(Yes)·Π = 3/9·3/9·6/9·6/9·9/14 = **0.03175**; P(No)·Π = 2/5·1/5·1/5·2/5·5/14 = **0.002286** ⇒ **P(Yes|x) = 0.933, P(No|x) = 0.067 → Play**. Also use this dataset for decision trees (slide title "Decision Tree: Information Gain"): H(S) = 0.940; IG(Outlook) = 0.2467, IG(Humidity) = 0.1518, IG(Wind) = 0.0481, IG(Temp) = 0.0292 → **root = Outlook** (build the full ID3 tree and the CART/Gini tree).
- **Assignment: Decision tree (Gini):** (1) Play Tennis, 7 rows: Sunny Hot High No→No; Sunny Hot High Yes→No; Overcast Hot High No→Yes; Rain Mild High No→Yes; Rain Cool Normal No→Yes; Rain Cool Normal Yes→No; Overcast Cool Normal Yes→Yes. Tasks: Gini of the target, Gini of every split, the best feature by Gini gain, build the tree, predict (Sunny, Cool, Normal, Windy = Yes), accuracy. (2) Buy Computer, 6 rows: Young Low No; Young High Yes; Middle Low Yes; Middle High Yes; Old Low No; Old High Yes. Tasks: count Yes/No, Gini of the target, Gini for Age, Gini gain for Age, build the tree, predict (Young, Low). **Solve both completely by hand and verify with sklearn.**
- **Lab 1 (kNN):** (a) students (hours, attendance%, result): S1 (2, 60) Fail, S2 (3, 65) Fail, S3 (4, 70) Pass, S4 (5, 75) Pass, S5 (6, 80) Pass → predict (4.5, 72) with k = 3; (b) points (1,2)A (2,3)A (3,4)A (6,7)B (7,8)B (8,9)B → predict (5,6) with k = 3; (c) kNN regression: houses H1–H8 (area, beds, age, price): (1000, 2, 10, 45), (1200, 2, 8, 50), (1500, 3, 5, 65), (1800, 3, 4, 75), (2000, 4, 3, 90), (2200, 4, 2, 100), (2500, 4, 1, 120), (2800, 5, 2, 135) → predict (1700, 3, 5) with k = 3 (show the result **with and without feature scaling** and explain the difference); (d) the 11-house, 5-feature table (area, beds, baths, age, distance, price) H1 (1000, 2, 1, 15, 12, 42) … H11 (3000, 5, 5, 1, 1, 170) → predict (1900, 3, 3, 5, 6). Solve them all.
- **Lab 2 (Naive Bayes):** (a) Weather-only 10 rows (Sunny No, Sunny No, Overcast Yes, Rain Yes, Rain Yes, Rain No, Overcast Yes, Sunny No, Sunny Yes, Rain Yes) → classify Sunny (priors, likelihoods, posteriors); (b) students (10 rows; hours, attendance → Pass/Fail) discretised Hours Low ≤ 3 / High > 3, Attendance Low < 70 / High ≥ 70; new (5, 75) = (High, High) → predict. Solve both, and show where Laplace smoothing changes things (zero counts).
- **Not yet taught as of 21 Sep 2026 (take from the textbooks, full depth):** everything from LDA and SVM onward (feature selection, PCA, LDA, all of unsupervised learning, graphical models, semi-supervised learning, active learning, topic modelling). The decision-tree assignment exists, but the lecture slides only show the dataset.

---

## 4. ERRATA — the correct versions must appear in the guide

1. **Naive Bayes slides 25–26:** slide 25 multiplies the correct likelihoods (3/9 and 2/5 for Sunny) but reports P(Yes|today) = 0.86; the correct normalised value is **0.0317/(0.0317 + 0.00229) = 0.933**. Slide 26 silently switches to P(Sunny|Yes) = 2/9 and P(Sunny|No) = 3/5 (inconsistent with the dataset and its own frequency table), which is what produces 0.86. **The correct answer from the dataset is 0.933 (Play = Yes).** Add a "⚠ Common Error" box explaining that you must read the counts from the table, and that normalising by the sum is valid because P(today) cancels.
2. The "Big Data Numbers" slide spells Yottabyte and Zettabyte as "Vottabyte"/"Zetabyte"; use the correct spellings.
3. Be explicit that the "Assignment 1: Play Tennis" dataset (7 rows) is a **different, smaller** dataset from the 14-row golf data; don't mix them.
4. Keep the "three LDAs" clearly separated wherever the acronym appears.

---

## 5. REQUIRED STRUCTURE OF THE DOCUMENT

**Front matter:** title page ("BO CDA 205 — Machine Learning Techniques | Complete CBT Exam Guide | IIT Patna · B.S. CSDA · Semester 3 · Prepared for Aditya"); how to use + 7-day/3-day plans; **syllabus-coverage matrix**; automatic TOC; exam pattern & strategy (fast hand-calculation tricks: log-sum for NB, squared distances for ranking, the entropy table for common fractions, Gini shortcuts, 2×2 eigenvalue formulas).

**For EVERY technique, use this template (in order):**
1. 🎯 Learning objectives
2. 📖 Intuition (StatQuest-style picture-first explanation) and where it is used
3. 📐 Notation & mathematical formulation (objective / probability model)
4. 🧮 Key formulas (boxed)
5. 🔄 **Algorithm pseudocode + flowchart**
6. ✍ **Fully hand-worked numeric example** (small data, every arithmetic step shown in tables), then the same example verified in Python
7. 👨‍🏫 Professor's example / assignment solution (if applicable)
8. 📊 Diagrams (decision boundary, tree, dendrogram, trellis, etc.)
9. ⚖ Strengths, weaknesses, assumptions, hyper-parameters, complexity (train/predict), sensitivity to scaling and outliers
10. ⚠ Common mistakes & MCQ traps
11. 🧠 Memory hooks
12. 🖥 Lab Corner: from-scratch numpy + scikit-learn
13. 📝 Practice exercises (with solutions at the end of the unit)
14. ✅ **MCQ practice set: at least 40 per unit sub-block** (Unit 0: 40; Unit 1: 150; Unit 2: 110; Unit 3: 50; Unit 4: 50), mixed (~55% single-correct, 15% multiple-correct, 20% numerical-answer, 10% assertion–reason / match), tagged [E]/[M]/[H]; **≥ 40% calculation-based**
15. 🔑 Answer key with explanations
16. 📌 One-page unit summary

**Back matter:** the **Algorithm comparison master table** (type, supervised?, parametric?, loss/objective, key hyper-parameters, complexity, scaling needed?, handles non-linearity?, interpretability); a **"Which algorithm?" decision flowchart**; **clustering-algorithm comparison** (K-means / K-medoids / hierarchical / DBSCAN / GMM-EM / spectral); the **HMM vs. MEMM vs. CRF table**; a **formula sheet** (entropy, IG, gain ratio, Gini, distances, Bayes/NB, LDA discriminant, Fisher J(w), SVM primal/dual/margin, PCA steps, K-means objective, EM updates, silhouette/DB/Dunn, forward/Viterbi recursions, LDA Gibbs update); a glossary; **3 full-length mock CBT tests** (60 MCQs each, with answer keys and explanations); last-24-hours list; index.

---

## 6. DIAGRAMS / FIGURES THAT MUST APPEAR (PNG at ≥ 200 dpi, embedded)

The AI/ML/DL/DS Venn diagram; the structured/semi/unstructured chevrons; the big-data units circles and the 4 V's wheel; the supervised vs. unsupervised illustration (the movie scatter plot with the test point (50, 50) and kNN circles for k = 1, 2, 3); train/val/test split bars; k-fold CV diagram; the confusion matrix and ROC curve; overfit/underfit plots; the full ID3 tree for the golf data and the Gini trees for both assignments; entropy vs. p and Gini vs. p curves; kNN decision boundaries for k = 1, 5, 15; the Naive Bayes computation table graphic; the LDA vs. QDA boundaries; the SVM margin diagram (support vectors, w, b, margin width) plus the soft-margin slack diagram and kernel-trick 2D→3D lift; filter vs. wrapper vs. embedded flowcharts; forward and backward selection step diagrams; PCA (data cloud with PC1/PC2 arrows, scree plot, projection) and LDA (projection separating two classes) vs. PCA; k-means iterations (3–4 panels); the elbow and silhouette plots; the dendrogram from the worked example; GMM contours and E/M-step panels; the K-medoids swap illustration; DBSCAN core/border/noise with ε-circles and an arbitrary-shape (moons) comparison vs. k-means; the HMM state diagram, forward and Viterbi trellises with the best path highlighted; MEMM vs. CRF graphical-structure diagrams; semi-supervised label propagation; the active-learning loop flowchart; the LDA (topic model) plate diagram and a topic-word bar chart.

Use **graphviz (`dot` is installed)** for trees, flowcharts and graphical models, and **matplotlib** for plots. Install anything missing: `pip install numpy scipy scikit-learn matplotlib python-docx`.

---

## 7. DOCX BUILD SPECIFICATIONS

- Build with **python-docx** via a re-runnable script `build_cda205_guide.py`; output `/Users/aditya/STUDY/IIT_Patna_NOTES/SEMESTER_3/205/CDA205_ML_Exam_Guide.docx`.
- A4, 2 cm margins, page numbers, header "CDA 205 · Machine Learning Techniques".
- Heading 1 = Unit, Heading 2 = technique, Heading 3 = sub-part; body 11 pt; code in Consolas 9.5 pt in shaded boxes; **calculation tables** with header shading and right-aligned numbers (4 decimal places).
- Colour call-out boxes: Definition (blue), Formula (green), Professor's Example/Assignment (purple), Trap (red), Memory Hook (yellow), Lab (grey).
- Math: native Word equations (OMML via LaTeX → MathML → OMML), with high-dpi mathtext PNGs as a fallback; no raw LaTeX.
- A real TOC field (press F9 once); MCQs numbered per unit (Q2.37); answer keys at the end of each unit.
- Target: roughly **300–380 pages**, **≥ 400 unit MCQs** plus 3 × 60 mock MCQs.

---

## 8. QUALITY-ASSURANCE CHECKLIST (per unit)

- [ ] Every syllabus phrase of the unit is a heading and is ticked in the coverage matrix.
- [ ] Every professor dataset and assignment/lab problem is fully solved by hand **and** reproduced in code with matching numbers (log them in `qa_log_unitX.txt`).
- [ ] Every calculation MCQ was recomputed in Python; distractors reflect real mistakes (forgetting to normalise, not scaling features, log base 2 vs. e, using counts instead of probabilities, confusing precision and recall); answer letters are balanced.
- [ ] Errata in Section 4 are corrected, not repeated; the three LDAs are never confused.
- [ ] All figures render; the TOC field exists; the file re-opens cleanly.
- [ ] Report pages, MCQs and figures added, plus any ambiguity you resolved.

**Start now:** list and read the source files (including rendering the image-only slides and a sample of the StatQuest pages), then confirm the unit plan in one short message. Then build **Front matter + Unit 0**, and stop and wait for "continue".
