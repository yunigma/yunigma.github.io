---
layout: page
title: Interaction effects and Model fitting
description: This page is reserved for my 7th BLOG.
---

### 1. Interaction
1) **Main effect**
mean differences *among the levels* of one factor.

2) **Interaction effect**
occurs when the mean differences *among conditions* differ from what predicted from overall main effect.

### 2. Model fitting
Types of correlation: positive and negative

#### Within-subjects factors

- Each participant has his/ her own baseline response --> One intercept (β0) for each participant --> **random effect**
- Each treatment changes the baseline response in the same way across participants --> Other coefficients (β1, β2, ...)are the same for all participants --> **fixed effect**

This is a **mixed-effects model** with **random intercept** (other names: “hierarchical model” or “multilevel linear model”)

```
lmer(DV ~ (1\|participant) + IV1 * IV2, data = data) (import::from(lmerTest, lmer))
```

#### Family-wise error rate (FWER)
**Tukey HSD**: for all-pairs test

```
glht(m_main,
    linfct = mcp(
      device = "Tukey",
      vision = "Tukey"))
```

