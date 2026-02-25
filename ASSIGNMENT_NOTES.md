# Assignment Notes — Correctness Review (main vs esshan)

Short, shareable notes by assignment. Correctness: **Correct** | **Mostly correct (minor issues)** | **Incorrect**.

---

## 14 — R-Squared

### Assignment 1: Temperature vs. Sales
- **Correctness:** Correct. Code replaces temperature with random values; R² drops to 0.00; explanation correctly states that with no relationship, none of the variance in sales is explained by temperature.
- **Key takeaway:** R² measures how much variance in y is explained by X. Random X → R² ≈ 0. Always interpret in context (e.g., “no sales explained by temperature”).
- **Corrections:** Fix typo: “becuase” → “because”; “explain” → “explained.”

### Assignment 2: Predicting Test Scores
- **Correctness:** Correct. Two single-predictor models and one two-predictor model are built; R² is higher for the combined model; explanation correctly ties this to more relevant information and variance explained.
- **Key takeaway:** Adding relevant predictors usually increases R². Multiple factors (e.g., hours studied and slept) can jointly explain more variance than either alone.
- **Corrections:** Typo “becuase” → “because.”

### Assignment 3: Misleading R-squared
- **Correctness:** Mostly correct. Code uses x in [-5, 5] and y = x²; linear fit gives R² = 0 (symmetric U). The notebook also tries x in [0, 5], giving high R² (~0.92), and correctly discusses why R² can be misleading and why plotting matters.
- **Key takeaway:** High R² does not guarantee a good model. For y = x², a linear fit on symmetric x ∈ [-5, 5] gives R² = 0; on [0, 5] it can be high. Always inspect the fit (e.g., plots) and use R² with other checks.
- **Corrections:** The assignment text says “R-squared might be surprisingly high”—for the stated range [-5, 5] it is actually 0; the “surprisingly high” case is when using only non-negative x (e.g., 0–5). The student’s note in the notebook is correct.

---

## 15 — P-Value

### Assignment 1: Temperature vs. Sales
- **Correctness:** Correct. Random temperature data yields high P-value (e.g., 0.863); explanation correctly describes that the result is more plausible under chance and contrasts with the strong-relationship case.
- **Key takeaway:** When X has no real relationship with y, the P-value is large (we don’t reject the null). When the relationship is strong, P-value is very small.
- **Corrections:** “becuase” → “because.” **Important:** The intro cell states “P-Value is also called Chi Value”—this is incorrect. P-value and Chi-squared (or chi statistic) are different concepts; do not treat them as the same.

### Assignment 2: Real-World Dataset Analysis
- **Correctness:** Correct. California housing data is used; P-values are interpreted (e.g., very small for median_income and housing_median_age, 0.027 for population). Explanation correctly identifies which variable has the “less significant” P-value and what that means.
- **Key takeaway:** Smaller P-value means stronger evidence against the null (variable has no effect). Comparing P-values across predictors helps see which relationships are most statistically significant.
- **Corrections:** None. (Model note: scaling/constant can affect interpretation; the written conclusion about P-values is still valid.)

### Assignment 3: The Importance of Sample Size
- **Correctness:** Correct (concept and typical outcome). Larger sample with same true relationship generally yields a smaller P-value; explanation about more data and confidence is correct.
- **Key takeaway:** More data (larger n) typically yields more precise estimates and smaller P-values when the effect is real, reducing the chance of attributing a real effect to coincidence.

---

## 7 — AUC/ROC

### Assignment 1: Interpretation
- **Correctness:** Correct. AUC = 0.6 is slightly better than random (0.5); perfect model at top-left of ROC; high threshold → more false negatives. All three answers are right.
- **Key takeaway:** AUC &gt; 0.5 means better than random; perfect classifier sits at (0, 1); raising the threshold tends to reduce positives (more FNs, fewer FPs).

### Assignment 2: Experiment
- **Correctness:** Correct. Using a high threshold (0.7) yields zero FPs and zero TPs in the shown confusion matrix; the written conclusion (zero FPs and zero TPs when threshold is too high) is correct.
- **Key takeaway:** Pushing the threshold high classifies almost everything as negative, reducing false alarms but also missing true positives.

### Assignment 3: Real-World Thinking (Earthquakes)
- **Correctness:** Correct. False negative (missing a real quake) is worse; operating toward high TPR even at higher FPR is the right trade-off.
- **Key takeaway:** In safety-critical settings, we often prefer high sensitivity (catch all events) even if it means more false alarms.

---

## 8 — Time to Frequency Domain

### Assignment 1: Remove Noise from an Audio Signal
- **Correctness:** Mostly correct. Code uses FFT, zeros frequencies above a cutoff, and uses IFFT. Logic is sound; implementation depends on a Colab/Drive path for the WAV file and has a small bug: `fftfreq` should use sample spacing `1/sample_rate`, not `noisy_signal[1] - noisy_signal[0]` (that’s a time diff, not sample period).
- **Key takeaway:** High-frequency components often correspond to noise; zeroing them in the frequency domain and inverting FFT can denoise the signal.
- **Corrections:** Use `1/sample_rate` (or `1.0/sample_rate`) in `np.fft.fftfreq(len(noisy_signal), 1/sample_rate)`.

### Assignments 2 & 3 (Heart Rate, Temperature Cycles)
- **Correctness:** Not fully verified in this pass; assignments are present in the notebook. Same FFT/periodicity concepts apply.

---

## 5 — Eigenvectors and Eigenvalues

### Assignment (Modify Matrix, Verify, Real-World)
- **Correctness:** Mostly correct. Matrix changed to [[3,1],[1,3]]; eigenvalues [4, 2] and eigenvectors are correct. Verification step uses `first_eigenvector` (from an earlier cell with a different matrix) instead of the current `eigenvector`; if run in order the numbers can still match the first eigenvector of the new matrix by kernel state, but the code is wrong.
- **Key takeaway:** For A = [[3,1],[1,3]], eigenvalues are 4 and 2; eigenvectors are (1,1) and (-1,1) (up to scaling). PCA uses eigenvectors (principal directions) and eigenvalues (variance along those directions).
- **Corrections:** In the verification, use `result_one = np.dot(A, eigenvector)` (not `first_eigenvector`). Typo “quanitfy” → “quantify.”

---

## 3 — Kurtosis

### Assignment tasks (Data collection, parameters, extended analysis, visualization, modeling)
- **Correctness:** Mostly correct (concept); code has a bug. `analyze_kurtosis` uses `returns.mean()` and `returns.std()` in a context where `returns` is a pandas Series; mixing with `np.exp`/array operations can cause errors (e.g., in the normal-curve plot). Converting to numpy (e.g., `returns = returns.to_numpy()` or using the already-computed `arr`) for those steps would fix it.
- **Key takeaway:** Kurtosis describes tail heaviness and peak sharpness; financial returns often show excess kurtosis. Rolling kurtosis and crisis periods (e.g., COVID) are good applications.
- **Corrections:** In `analyze_kurtosis`, use a numpy array for the normal-curve plot (e.g., `m, s = arr.mean(), arr.std()` and use `m`, `s` in the formula).

---

## 11 / Confidence Intervals

(Notebook name on branch: may be `Confidence Intervals.ipynb` or `11_Confidence_Intervals.ipynb`.)

- **Correctness:** Not fully re-run in this review. Assignments typically cover sample size, confidence level, and real-world poll analysis. If answers follow the same pattern as other notebooks (code + short interpretation), apply the same checks: code runs, CIs narrow with larger n and widen with higher confidence level.
- **Key takeaway:** Larger n → narrower intervals; higher confidence level → wider intervals. Always report the level (e.g., 95% CI).

---

## 12 — Naive Bayes

### Data Exploration, Feature Engineering, Model Comparison
- **Correctness:** Notebook contains filled code and outputs (e.g., accuracy 0.67). Full assignment-by-assignment verification was not repeated here; structure matches “run model, interpret” style.
- **Key takeaway:** Naive Bayes assumes feature independence given the class; useful baseline for classification. Explore data, engineer features, then compare models (e.g., accuracy).

---

## 16 — N-Gram Score

### Assignment 1: Change n (bigrams, 4-grams)
- **Correctness:** Correct. Bigrams and 4-grams are computed; explanation correctly states that as n increases, repeated n-grams become rarer (longer sequences are less likely to repeat).
- **Key takeaway:** Larger n means longer patterns; repetition (and thus frequency counts) tends to decrease. Conditional probability interpretation (next token given previous n−1) is the right direction.

### Assignment 2: Conditional probability
- **Correctness:** Correct. Code implements P(token | prefix) as count(ngram)/count(prefix); formula and implementation match the hint.
- **Key takeaway:** Sophisticated n-gram scores use conditional probability; count(ngram) / count((n−1)-gram prefix).

### Assignment 3: Real-world (e.g., temperatures)
- **Correctness:** Correct. High-scoring bigrams/trigrams for temperatures can indicate likely next values and seasonal patterns; use for simple predictive or pattern-finding tasks.
- **Key takeaway:** N-gram scores on time series (e.g., daily temperatures) can capture recurring patterns and support short-horizon predictions or pattern analysis.

---

## 9 — Edgeworth Cycles

### Assignments 1–3 (Price war severity, Price reset, Asymmetry)
- **Correctness:** Not re-executed in this review. Assignments ask for parameter changes and observing plots; if code runs and comments tie parameter changes to cycle shape, treat as conceptually correct.
- **Key takeaway:** Parameters (e.g., min/max price drop, jump factor, asymmetry) control cycle depth, reset magnitude, and symmetry; experiment with the simulation to see their effect.

---

## Summary

- **Correct:** 14 (all three), 15 (all three), 7 (all three), 16 (all three), and the conceptual parts of 5 and 8.
- **Mostly correct (minor issues):** 14 Assn 3 (wording about “surprisingly high” vs range), 5 (wrong variable in verification, typo), 3 (pandas/numpy bug in kurtosis plot), 8 Assn 1 (fftfreq argument).
- **Corrections to apply:** Fix “P-Value = Chi Value” in 15; use correct variable in 5 verification; fix fftfreq and kurtosis plot as above; fix repeated typos (“becuase”, “quanitfy”).
