# investment-agent-overview
AI-assisted market intelligence platform combining multi-timeframe analysis, historical edge research, machine learning, and risk-aware decision support for financial markets.

# NovaStack Market Intelligence

NovaStack Market Intelligence is a Python-based financial research and decision-support platform for analyzing market structure, multi-timeframe context, order flow, historical behavior, and machine-learning-assisted opportunities.

The project is designed around a simple idea:

> **Use deterministic market logic to identify opportunities, measure what happens next, and use data and machine learning to improve future decision quality.**

It combines real-time market ingestion, historical replay, feature engineering, research analytics, model evaluation, governance, and simulated execution into one modular system.

The project is independent of any existing autonomous-agent framework. Agent platforms may be integrated later as orchestration or research assistants.

---

## Overview

NovaStack processes financial-market data through several layers:

```text
Market Data
    ↓
Multi-Timeframe Context
    ↓
Structure & Order Flow
    ↓
Deterministic Opportunity Detection
    ↓
Historical Edge Measurement
    ↓
Machine Learning Qualification
    ↓
Risk & Governance
    ↓
Decision / Simulation Layer
```

Each layer has a separate responsibility.

This separation makes the system easier to:

- explain,
- test,
- backtest,
- measure,
- extend,
- and eventually expose to autonomous agents through APIs.

---

## Core Ideas

### Multi-Timeframe Analysis

Different timeframes answer different questions.

A typical configuration may use:

| Layer | Purpose |
|---|---|
| Higher timeframe | Broad market context |
| Structure timeframe | Swing structure and directional conditions |
| Setup timeframe | Local opportunity development |
| Execution timeframe | Fine-grained confirmation and measurement |

This prevents a single short-term signal from being evaluated without larger context.

---

### Deterministic Opportunity Detection

The platform uses explicit setup logic to identify market opportunities.

Rather than allowing a machine-learning model to make an unrestricted decision, the system first detects a candidate using understandable market rules.

```text
Market Conditions
    ↓
Rule-Based Setup
    ↓
Candidate Opportunity
    ↓
ML Qualification
    ↓
Governance
```

This keeps the research process explainable and auditable.

---

## Opportunity Measurement

A key component is the opportunity-measurement engine.

When the system detects a valid candidate, it records a point-in-time snapshot containing information such as:

- instrument,
- direction,
- reference price,
- risk distance,
- potential objective,
- market context,
- structure state,
- order-flow conditions,
- model features,
- data-quality metrics,
- and supporting evidence.

The system then follows subsequent market behavior.

Possible outcomes include:

```text
TARGET_FIRST
STOP_FIRST
AMBIGUOUS
HORIZON_EXPIRED
```

This allows the platform to learn from more than only decisions that were ultimately acted upon.

It can also measure opportunities that were rejected, filtered, or remained unresolved.

---

## Edge Discovery

Edge Discovery asks:

> **When similar market conditions occurred historically, what tended to happen next?**

The research layer groups historical opportunities by measurable features and evaluates their outcomes.

Examples of questions the platform can explore:

- Does a particular structure improve outcome quality?
- Does order-flow confirmation strengthen a setup?
- Are some conditions more effective in one market regime than another?
- Does directional context matter?
- Which combinations of features reduce expectancy?
- Does a setup behave differently across instruments?

This creates a data-driven feedback loop between market theory and observed behavior.

---

## Historical Replay

Historical replay reconstructs market conditions chronologically.

The replay engine processes historical market information in time order so that each decision uses only information that would have been available at that point.

```text
Historical Data
    ↓
Chronological Reconstruction
    ↓
Context
    ↓
Structure
    ↓
Opportunity Detection
    ↓
Outcome Measurement
```

This helps avoid future-data leakage and produces research observations suitable for analysis and model development.

Example:

```bash
python -m scripts.run_historical_replay --instrument <SYMBOL>
```

---

## Machine Learning

Machine learning is used as a **qualification layer**, not as an uncontrolled decision engine.

The model evaluates feature combinations associated with an already-detected opportunity.

Conceptually:

```text
Rule-Based Opportunity
    ↓
Feature Snapshot
    ↓
ML Score
    ↓
Historical Comparison
    ↓
Research / Shadow Evaluation
```

This makes it possible to ask whether ML improves an existing process rather than replacing the entire process.

---

## Feature Engineering

Features are captured at the moment an opportunity is created.

They may represent:

- market direction,
- structure,
- volatility,
- price location,
- order-flow state,
- evidence agreement,
- risk geometry,
- regime characteristics,
- and setup-specific context.

The eventual outcome becomes the measurement target.

The key principle is:

> **Only information available at decision time belongs in the feature set.**

---

## Walk-Forward Validation

Model quality is evaluated using walk-forward analysis.

Instead of training and evaluating on the same observations, the platform repeatedly trains on the past and evaluates on the next unseen period.

```text
Train
  ↓
Test on unseen observations
  ↓
Expand training window
  ↓
Test again
  ↓
Repeat
```

Typical metrics may include:

- AUC,
- balanced accuracy,
- probability calibration,
- selected observations,
- outcome distribution,
- expectancy,
- and cumulative risk-adjusted return units.

Example:

```bash
python -m scripts.update_ml_report --instrument <SYMBOL>
```

---

## Shadow Research

Models can operate in **shadow mode**.

In shadow mode, a model:

- observes opportunities,
- produces scores,
- records decisions,
- compares predictions with later outcomes,
- and has no independent authority.

This provides a safer path for evaluating new models before allowing them to influence higher-level decisions.

---

## Governance

The platform separates intelligence from authority.

A strategy or model may identify an attractive opportunity, but a final governance layer can still block it.

```text
Opportunity
    ↓
Model Evaluation
    ↓
Policy Checks
    ↓
Risk Checks
    ↓
Governor
    ↓
Approved / Rejected
```

This design supports explicit controls by:

- instrument,
- strategy,
- environment,
- model status,
- risk conditions,
- and operational state.

---

## Multi-Asset Design

NovaStack is designed so that each supported market maintains independent runtime state.

```text
Asset A
  ├── market state
  ├── setup state
  ├── model state
  └── opportunity state

Asset B
  ├── market state
  ├── setup state
  ├── model state
  └── opportunity state
```

This prevents one market from accidentally affecting another and provides a foundation for adding more instruments later.

---

## Restart Recovery

The runtime can reconstruct active research opportunities after a restart.

Recovery uses persisted state and available market events to determine whether an opportunity:

- was already resolved,
- needs missing observations replayed,
- or should remain active.

Historical research observations are kept separate from live runtime recovery.

---

## Event-Oriented Storage

NovaStack uses an event-oriented persistence model.

Instead of storing only the latest result, the system records the progression of:

- market observations,
- opportunity states,
- research results,
- model reports,
- and system decisions.

This provides a useful audit trail for later research and diagnostics.

SQLite is currently used for lightweight persistence, with the architecture capable of moving to a larger datastore if needed.

---

## Dashboard

The dashboard provides a consolidated research view across supported assets.

Typical sections include:

- current market context,
- structure,
- order flow,
- opportunity signals,
- historical readiness,
- strategy measurement,
- edge discovery,
- model evaluation,
- dataset status,
- model status,
- governance state,
- and system health.

The asset selector controls what is being viewed, not what the runtime is processing.

---

## Project Structure

```text
.
├── main.py
│
├── api/
│   ├── server.py
│   ├── dashboard_page.py
│   ├── ml_training.py
│   └── sim_execution.py
│
├── agents/
│   └── risk_agent.py
│
├── engine/
│   ├── analytics/
│   ├── backtest/
│   ├── ml/
│   └── research/
│
├── models/
│   ├── strategy_measurement.py
│   ├── trade_plan.py
│   ├── trade_signal.py
│   └── price_action.py
│
├── runtime/
│   ├── orchestrator.py
│   ├── rolling_bars.py
│   ├── measurement_runtime.py
│   ├── execution_policy.py
│   ├── neural_setup_shadow_runtime.py
│   └── ml_training/
│
├── scripts/
│   ├── run_historical_replay.py
│   ├── update_ml_report.py
│   └── audit_neural_setup_c_serving.py
│
├── storage/
│   └── event_store.py
│
├── data/
│   └── ml/
│
└── artifacts/
    └── neural/
```

The exact internal strategy definitions, infrastructure details, credentials, broker configuration, and private research artifacts are intentionally not documented in the public repository.

---

# Quick Start

## Requirements

- Linux or compatible Python environment
- Python 3
- Git
- Python virtual environment
- external market-data source or adapter

---

## Create and activate a virtual environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

---

## Install dependencies

If the repository includes a dependency file:

```bash
pip install -r requirements.txt
```

---

## Set the project path

```bash
export PYTHONPATH="$(pwd)"
```

---

## Start the application

```bash
python main.py
```

The application exposes its API and research dashboard through the configured application port.

---

## Run historical research

```bash
python -m scripts.run_historical_replay   --instrument <SYMBOL>
```

---

## Generate a model research report

```bash
python -m scripts.update_ml_report   --instrument <SYMBOL>
```

---

# Future Direction

## Autonomous Research Agents

NovaStack can eventually become a backend for a more autonomous financial-research agent.

An external agent framework could interact with the platform through a controlled tool/API layer.

For example:

```text
Autonomous Agent
    │
    ├── inspect market state
    ├── request historical analysis
    ├── compare strategy performance
    ├── build research datasets
    ├── evaluate candidate models
    ├── monitor system health
    └── propose policy changes
         │
         ▼
     NovaStack APIs
         │
         ▼
     Governance Layer
```

This could include general-purpose agent systems, local agent frameworks, or future integrations with platforms such as Hermes-style autonomous agents.

The important architectural rule would remain:

> **The agent can reason and propose; deterministic policy and governance remain responsible for authority.**

---

## Continuous Research Agent

A future research agent could automatically:

```text
collect completed observations
    ↓
measure data coverage
    ↓
identify weak regions
    ↓
run historical analysis
    ↓
train candidate models
    ↓
perform walk-forward evaluation
    ↓
compare against the current model
    ↓
produce a research recommendation
```

This would turn the platform into a continuously improving research environment.

---

## Natural-Language Research

An agent could eventually answer questions such as:

```text
Why was this opportunity detected?

What market conditions were present?

How did similar historical situations perform?

Did the model agree with the deterministic setup?

Which features contributed most?

Has this pattern improved or weakened recently?
```

The agent would use NovaStack's persisted data and APIs rather than relying on unsupported assumptions.

---

## Portfolio Research

The same architecture can be expanded from individual markets into broader investment research.

Possible extensions include:

- equities,
- futures,
- ETFs,
- indices,
- digital assets,
- fixed-income instruments,
- and derivatives.

The research layer could evaluate:

- cross-asset relationships,
- regime changes,
- sector strength,
- volatility conditions,
- portfolio exposure,
- and risk concentration.

---

## Options Research

Options are a natural extension because they add another decision layer on top of the underlying market view.

An options research component could evaluate:

```text
Underlying Opportunity
    ↓
Directional / Volatility Thesis
    ↓
Expiration Selection
    ↓
Strike Selection
    ↓
Liquidity Filter
    ↓
Greeks / Volatility Analysis
    ↓
Risk Model
    ↓
Governance
```

Useful features could include:

- implied volatility,
- volatility rank,
- skew,
- term structure,
- delta,
- gamma,
- theta,
- vega,
- days to expiration,
- spread width,
- volume,
- open interest,
- and expected move.

Machine learning could then research not only whether an underlying opportunity is attractive, but also which derivative structure has historically expressed that view most effectively.

---

## More Advanced AI

Future AI components could focus on:

- regime classification,
- anomaly detection,
- feature discovery,
- explainability,
- strategy degradation detection,
- cross-asset context,
- portfolio-level reasoning,
- and automated research planning.

A mature system could eventually operate as a collection of specialized agents:

```text
Market Research Agent
Risk Agent
Model Evaluation Agent
Portfolio Agent
Operations Agent
Governance Agent
```

All sharing the same audited market and research infrastructure.

---

# Development Philosophy

NovaStack follows a few core principles:

- deterministic logic before opaque automation,
- separate research from authority,
- keep asset state isolated,
- distinguish historical and live observations,
- evaluate models on unseen data,
- make decisions auditable,
- prefer simulation before higher-risk deployment,
- and improve the process through measured evidence rather than assumptions.

---

<img width="1157" height="807" alt="1" src="https://github.com/user-attachments/assets/f396b56a-6ff6-4d43-b4c3-89569c6a7a87" />
**AI Edge Discovery — Walk-Forward Model Validation.**

<img width="1167" height="647" alt="2" src="https://github.com/user-attachments/assets/1b644c3f-b23c-4718-ac42-aff8e5fe8978" />
**Market Opportunity Analytics — Live Measurement & Historical Replay**


# Disclaimer

NovaStack is an experimental software-engineering and financial-research project.

Nothing in this repository constitutes financial, investment, legal, or professional advice. Historical analysis, simulation results, model outputs, and research metrics do not guarantee future performance. Financial markets involve substantial risk, and any real-world deployment requires independent validation, appropriate safeguards, and professional judgment.
