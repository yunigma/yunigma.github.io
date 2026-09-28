---
layout: page
title: R playing around with a language corpus
description: This page is reserved for my 4th BLOG.
---

### The WALS data set
This is blog is a playing around with two variables and their visualisation of the data from a linguistic dataset WALS. The WALS is a dataset that contains many languages of the world with their phonetic, grammatic and syntactic characteristics. Using the chi^2 test the relation between two variables - Future Tense and the expression of Prohibitive in a language - is investigated. To build the plot, an additional variable - probability of explicite Future Tense with a given Prohibitive - is calculates.

#### 1. Taking two variables from the WALS data set and investigating the relationship between the two variables.

```
wals <- read.csv('data/wals_data.csv', header=T, sep=',')
wals_small <- wals[, c(77,81)]
colnames(wals_small) <- c('FutureTense', 'Prohibitive')
wals_small <- wals_small[wals_small$FutureTense!='' &
                         wals_small$FutureTense!='3 No dominant order' &
                         wals_small$Prohibitive!='' &
                         wals_small$Prohibitive!='3 No dominant order',]
wals_small <- droplevels(wals_small)
summary(wals_small)
xtabs( ~ Prohibitive + FutureTense, wals_small)
curve(dchisq(x, 3), 0, 50, col='red')
```

Visualization of the relationship between the two variables:
```
# function to calculate the probability of FutureTense type given Prohibitive
prob <- function(i, j) {
  return(nrow(wals_small[wals_small$FutureTense==i &
                         wals_small$Prohibitive==j,]) / nrow(wals_small))}

# test
prob(as.character(wals_small[2, "FutureTense"]),
     as.character(wals_small[2, "Prohibitive"]))

prob_futur2 <- vector("list", nrow(wals_small))
prob_futur <- numeric(nrow(wals_small))

for (i in 1:nrow(wals_small)) {
  prob_futur[i] <- prob(as.character(wals_small[i, "FutureTense"]),
                        as.character(wals_small[i, "Prohibitive"]))}

wals_small$ProbabilityFuture <- prob_futur
```

```
interaction.plot(wals_small$Prohibitive, wals_small$FutureTense,
                 wals_small$ProbabilityFuture, trace.label="FutureTense",
                 xlab="Prohibitive", ylab="ProbabilityOfFuture", col=2:4)
```

#### 2. Contigency table
```
wals_xtabs <- xtabs( ~ Prohibitive + FutureTense, wals_small)

expected <- function(i,j) {
            return((sum(wals_xtabs[i,])*
                  sum(wals_xtabs[,j]))/
                  sum(wals_xtabs))
}
a <- (wals_xtabs[1,1]-expected(1,1))^2/expected(1,1)
b <- (wals_xtabs[1,2]-expected(1,2))^2/expected(1,2)
c <- (wals_xtabs[2,1]-expected(2,1))^2/expected(2,1)
d <- (wals_xtabs[2,2]-expected(2,2))^2/expected(2,2)

wals_chiSq <- a + b + c + d
wals_chiSq
```

Degrees of freedom:
Number of rows - 1 $\times$ number of columns - 1 = (4 - 1) x (2 - 1) = 3

#### 3. chi^2 test
```
chisq.test(xtabs( ~ Prohibitive + FutureTense, wals_small))
summary(wals_xtabs)
```

