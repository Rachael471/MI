# MI
library(rstanarm)
library(mice)
library(miceadds)
library(lme4)

# Number of observations at the lowest level
n <- 5000

# Number of clusters
k <- 100

# Number of areas
m <- 500

# Generate random cluster assignments for each observation
Cluster <- sample(1:k, n, replace = TRUE)

# Generate random area assignments for each cluster
Class <- sample(1:m, k, replace = TRUE)

# Generate random data for level 1
Level_1 <- rbinom(n, 1, 0.5)

# Generate random data for level 2, with means that vary by cluster
mu_cluster <- rnorm(k)
prob_lvl2 = exp(mu_cluster[Cluster]) / (1 + exp(mu_cluster[Cluster]))

Level_2 <- rbinom(n, 1, prob_lvl2)

# Generate random data for level 3, with means that vary by area
mu_class <- rnorm(m)
prob_lvl3 = exp(mu_class[Class]) / (1 + exp(mu_class[Class]))

Level_3 <- rbinom(n, 1, prob_lvl3)

# Combine the data into a dataframe
df <- data.frame(Cluster, Class, Level_1, Level_2, Level_3)
#df_ <- df[order(df$Cluster),]
df_ <- df[
  with(df, order(Cluster, Class)),
]
head(df_, 10)
