Time-to-Incident Survival Analysis for Production Systems

Hazard-Based Modeling with Censoring, Calibration, and Diagnostics

 Overview

This project builds an end-to-end survival analysis pipeline to model time-to-incident in production systems using hazard-based statistical modeling.

Instead of treating incidents as a binary classification problem, this work models when incidents occur, explicitly handling right-censoring, uncertainty, and risk escalation under system stress. The methodology aligns with reliability engineering and site reliability analytics used in large-scale infrastructure.

Problem Statement

In production environments, many systems do not fail during the observation window. Traditional classification approaches ignore this censoring and fail to answer questions such as:

How long until the next incident?

How does system stress change instantaneous failure risk?

What is the probability the system survives the next hour?

This project addresses these questions using survival analysis, not classification.

 Why Survival Analysis 

Incidents are time-to-event outcomes

Many observations are right-censored (no incident observed yet)

Risk evolves continuously over time

Decisions depend on hazard and conditional survival, not labels

Survival analysis provides:

interpretable hazard functions

principled likelihood-based inference

calibrated survival probabilities

Data Description

The dataset is synthetically generated to mimic production telemetry while preserving known ground truth.

Each row represents a monitored service interval.

Column	Description
service_id	Service identifier
cpu_load	Average CPU utilization
memory_pressure	Memory stress indicator
latency_ms	Request latency (ms)
error_rate	Fraction of failed requests
duration	Time until incident or censoring
event	1 = incident observed, 0 = censored

Right-censoring naturally arises when monitoring ends before an incident occurs.

Methodology
1. Non-Parametric Baseline

Kaplan–Meier survival estimation

Stratified survival curves by stress level

Visual inspection of survival separation

2. Parametric Hazard Model

An exponential hazard model with covariates:

𝜆(t|x)=exp(β0​++β^⊤x)


Estimated via maximum likelihood, not black-box fitting.

3. Likelihood-Based Inference

Custom negative log-likelihood

Numerical optimization (BFGS)

Interpretable coefficients and hazard ratios

4. Model Evaluation

Train/Test split

Concordance index (C-index) for ranking quality

Calibration via risk stratification

5. Diagnostics

Cox–Snell residuals

Empirical vs Exp(1) comparison to assess model adequacy

🔍 Key Results

High-stress services exhibit lower survival probabilities

Error rate and latency strongly increase instantaneous incident risk

Predicted risk groups show monotonic survival separation

Cox–Snell residuals indicate reasonable model fit

🛠️ Actionable Outputs (SRE-Style)

The model enables:

Risk ranking of services (prioritization)

Conditional survival queries, e.g.
“What is the probability this service survives the next hour?”

Counterfactual analysis under reduced system stress

These outputs support proactive incident prevention, not just post-hoc analysis.
