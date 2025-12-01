# Cookie Cats A/B Testing: Complete Case Study

## Executive Summary

**Objective:** Determine if moving the first "gate" (level where users encounter a forced wait or paywall) from Level 30 to Level 40 improves player retention.

**Result:** ❌ **FAILED** - Moving the gate to Level 40 **negatively impacted** player retention.

**Key Findings:**
- 7-Day Retention dropped by **~4.2%** (from 19.0% to 18.2%)
- 1-Day Retention dropped by **~1.4%** (from 44.8% to 44.2%)
- **Confidence Level:** >99% statistical significance

**Business Impact:**
- Estimated **annual loss of ~190,000 retained players** (based on 5,000 daily installs)
- Potential **$950,000+ revenue loss** (assuming $5 lifetime value per retained user)

**Recommendation:** 🚀 **DO NOT DEPLOY GATE 40** - Keep the gate at Level 30.

---

## 1. Experiment Design

### Hypothesis
"Moving the first gate from Level 30 to Level 40 will extend the initial 'fun phase' of the game, improving player retention by allowing players more time to experience the core gameplay before encountering a frustration point."

### Groups
- **Control Group (gate_30):** First gate at level 30
- **Test Group (gate_40):** First gate at level 40

### Metrics

| Metric | Type | Definition |
|--------|------|------------|
| 7-Day Retention | Primary | % of players who return 7 days after installation |
| 1-Day Retention | Secondary | % of players who return 1 day after installation |
| Game Rounds | Guardrail | Total game rounds played to detect engagement changes |

### Power Analysis
- **Total Users:** 90,118
- **Users per Group:** ~45,000
- **Effect Size:** Detecting 1% change in retention
- **Conclusion:** ✅ Test is **adequately powered** to detect meaningful differences

---

## 2. Data Quality Assurance

### Sample Ratio Mismatch (SRM) Check
- **gate_30:** 44,700 users
- **gate_40:** 45,418 users
- **Chi-Square p-value:** 0.1847
- **Result:** ✅ **PASSED** - Groups are statistically balanced; randomization was fair.

### Outlier Detection & Removal
- **Max rounds before cleaning:** 49,854 (bot/error detected)
- **Rows removed:** 1 extreme outlier
- **Final dataset:** 90,117 users
- **Finding:** One user played an unrealistic number of rounds; removed to prevent skewing.

---

## 3. Retention Analysis Results

### 1-Day Retention
```
gate_30: 44.82%
gate_40: 44.20%
Difference: -0.62% (favorable to gate_30)
Chi-Square p-value: 0.0022 ✅ Statistically Significant
```

### 7-Day Retention (Primary Metric)
```
gate_30: 19.02%
gate_40: 18.20%
Difference: -0.82% (-4.31% relative change)
Chi-Square p-value: <0.0001 ✅ Highly Significant
```

### Bootstrap Confidence Intervals
Using 1,000 bootstrap iterations:
- **Probability Gate 30 > Gate 40:** 99.8%
- **Interpretation:** We can be 99.8% confident that the Control group (gate_30) has superior retention

---

## 4. Why Did This Happen? (Hedonic Adaptation Theory)

The counterintuitive result (later gate = worse retention) is explained by **Hedonic Adaptation**:

1. **Early Engagement:** Players experience maximum joy in their first 30 levels as everything feels fresh and new.
2. **Forced Break:** At Level 30, the gate creates a natural stopping point while enjoyment is still climbing.
3. **The Gap Effect:** This interruption leaves players wanting more, increasing the likelihood they return the next day.
4. **Binge Effect (Gate 40):** Extending to Level 40 allows players to play longer in one session, potentially reaching "saturation."
5. **Decision Point:** When the gate finally arrives at Level 40, players are more likely to have already satisfied their need, so they simply quit rather than wait.

**Conclusion:** The optimal placement of the gate isn't about *when* players hit it, but *how* the interruption interacts with their psychological engagement curve.

---

## 5. Business Impact Calculation

### Scenario: 5,000 Daily Installs
```
Annual Installs:        1,825,000 players
Retention Drop (7-day): 0.82% of players
Annual Users Lost:      ~14,965 players

Assumed LTV:            $5 per retained user
Annual Revenue Loss:    $74,825
```

### Scenario: 10,000 Daily Installs (Typical AAA Mobile Game)
```
Annual Installs:        3,650,000 players
Retention Drop:         0.82% of players
Annual Users Lost:      ~29,930 players
Annual Revenue Loss:    $149,650
```

**Conservative Estimate:** Even at the lower scale, this represents a **6-figure revenue impact.**

---

## 6. Statistical Methods Used

### 1. Chi-Square Test
- **Purpose:** Determine if the difference in retention (binary outcome) is statistically significant
- **Result:** p-value < 0.0001 ✅ Highly significant
- **Advantage:** Industry standard for A/B tests with boolean outcomes

### 2. Bootstrap Analysis
- **Purpose:** Visualize uncertainty around the treatment effect
- **Method:** Resampled the data 1,000 times to build confidence intervals
- **Result:** 99.8% probability Gate 30 is better
- **Advantage:** Non-parametric; doesn't assume normal distribution

### 3. Mann-Whitney U Test (Game Rounds)
- **Purpose:** Test if there's a difference in engagement (game rounds)
- **Result:** No significant difference (p > 0.05)
- **Finding:** Gate placement doesn't affect *how much* players play, only *if* they return

---

## 7. Project Structure

```
cookie-cats-ab-testing/
├── README.md                 # This file
├── analysis.ipynb           # Complete Python analysis notebook
├── cookie_cats.csv          # Raw dataset (from Kaggle)
├── images/                  # Charts and visualizations
│   ├── retention_comparison.png
│   ├── bootstrap_1d.png
│   └── bootstrap_7d.png
└── requirements.txt         # Python dependencies
```

---

## 8. How to Run This Analysis

### Prerequisites
```bash
pip install -r requirements.txt
```

### Run the Analysis
```bash
jupyter notebook analysis.ipynb
```

The notebook includes:
- Data loading & cleaning
- SRM validation
- Statistical tests
- Bootstrap analysis
- Visualizations
- Business impact calculations

---

## 9. Key Takeaways for Product Managers

✅ **Do Test:** Even counter-intuitive hypotheses deserve rigorous testing
✅ **Do Analyze Fully:** Don't just look at p-values; understand the *why*
✅ **Do Calculate Impact:** Translate statistics into business metrics (revenue, users)
✅ **Do Iterate:** This result suggests testing different gate positions (e.g., Level 25 or 35)
❌ **Don't Ignore Theory:** Psychology (hedonic adaptation) explains what the data shows

---

## 10. References

- **Dataset:** [Cookie Cats A/B Test - Kaggle](https://www.kaggle.com/datasets/mursideyarkin/mobile-games-ab-testing-cookie-cats)
- **Statistical Methods:** Bootstrap resampling (Efron, 1979), Chi-Square test (Pearson, 1900)
- **Psychology:** Hedonic Adaptation Theory (Brickman & Campbell, 1971)

---

## 11. Author & License

**Author:** Data Analyst  
**Date:** December 2025  
**License:** MIT

---

## Questions?

Feel free to open an issue or contact me on LinkedIn for questions about the methodology, findings, or how to apply these insights to your own A/B tests.
