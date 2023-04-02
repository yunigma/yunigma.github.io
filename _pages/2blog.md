---
layout: page
title: Linear models
description: This page is reserved for my 2d BLOG.
---

#### 1. Formulating the model

| Model function  | Description |
| -------- | -------- |
| One IV | lm(DV ~ IV, data = data) |
| Two IVs | lm(DV ~ IV1 + IV2, data = data) |
| Two IVs with interaction | lm(DV ~ IV1 * IV2, data = data) |
| Two IVs, interaction, **within-subjects design** | lmer(DV ~ (1\|participant) + IV1 * IV2, data = data) (import::from(lmerTest, lmer))|

#### 2. Analysis of variance

```
import::from(psycho, analyze)

m_full <- lm(DV ~ IV1 * IV2, data = data)
anova_m_full <- anova(m_full)
anova_m_full

results_anova <- analyze(anova_m_full)
print(results_anova)
summary(results_anova)
```
