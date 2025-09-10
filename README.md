# meical
just testing 
# Install & load packages
packages <- c("MASS", "ggplot2")
lapply(packages, function(p) if (!require(p, character.only = TRUE)) install.packages(p))

library(MASS)
library(ggplot2)

# ---- Simulate patient data ----
set.seed(2025)
patients <- data.frame(
  BloodPressure = rnorm(60, mean = 120, sd = 15),
  Cholesterol   = rnorm(60, mean = 200, sd = 30),
  Glucose       = rnorm(60, mean = 100, sd = 20),
  Diagnosis     = factor(rep(c("Healthy", "Diseased"), each = 30))
)

# ---- Linear Discriminant Analysis (LDA) ----
lda_model <- lda(Diagnosis ~ BloodPressure + Cholesterol + Glucose, data = patients)

# Print results
print(lda_model)

# ---- Predict class membership ----
pred <- predict(lda_model)
patients$LDA <- pred$x[,1]   # first discriminant axis

# ---- Plot LDA results ----
ggplot(patients, aes(x = LDA, fill = Diagnosis)) +
  geom_histogram(binwidth = 1, alpha = 0.7, position = "identity", color = "black") +
  labs(title = "Discriminant Analysis of Patients",
       x = "Linear Discriminant Score",
       y = "Number of Patients") +
  theme_minimal()
