# MI
library(rstanarm)

# Number of observations at the lowest level
n <- 100

# Number of clusters
k <- 10

# Number of areas
m <- 5

# Generate random cluster assignments for each observation
cluster <- sample(1:k, n, replace = TRUE)

# Generate random area assignments for each cluster
area <- sample(1:m, k, replace = TRUE)

# Generate random data for level 1
level1 <- rbinom(n, 1, 0.5)

# Generate random data for level 2, with means that vary by cluster
mu_cluster <- rnorm(k)
level2 <- rbinom(n, 1, 0.5 + mu_cluster[cluster])

# Generate random data for level 3, with means that vary by area
mu_area <- rnorm(m)
level3 <- rbinom(n, 1, 0.5 + mu_area[area])

# Combine the data into a dataframe
df <- data.frame(level1, level2, level3, cluster, area)
