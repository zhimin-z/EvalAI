# Supported Evaluation Workflow Strategies

This document provides a comprehensive analysis of which strategies from the unified evaluation workflow are natively supported by EvalAI. A strategy is considered "supported" only if EvalAI provides it natively in its full installation—meaning that once EvalAI is fully installed, the strategy can be executed directly without implementing custom modules or integrating external libraries.

## Summary

EvalAI is a **platform for hosting and managing AI challenges** with leaderboards and participant submissions. It is **not a complete evaluation harness** that provides end-to-end evaluation capabilities. Instead, EvalAI provides infrastructure for challenge hosting, submission management, and leaderboard maintenance, while relying on challenge hosts to provide custom evaluation logic.

---

## Phase 0: Provisioning (The Runtime)

### Step A: Harness Installation

- **Strategy 1: PyPI Packages** ❌ **NOT SUPPORTED**
  - EvalAI itself can be installed via PyPI/pip, but it does not act as an evaluation harness that installs and manages evaluation dependencies.
  
- **Strategy 2: Git Clone** ✅ **SUPPORTED**
  - EvalAI can be cloned from GitHub and installed from source for development and deployment.
  
- **Strategy 3: Container Images** ✅ **SUPPORTED**
  - EvalAI provides Docker and Docker Compose configurations for containerized deployment.
  - Docker images are used for both the platform itself and for code upload challenges where participant submissions run in containers.
  
- **Strategy 4: Binary Packages** ❌ **NOT SUPPORTED**
  - No standalone binary distribution is provided.
  
- **Strategy 5: Node Package** ❌ **NOT SUPPORTED**
  - While EvalAI uses Node.js for frontend development, it is not distributed as a Node package for evaluation purposes.

### Step B: Service Authentication

- **Strategy 1: Evaluation Platform Authentication** ✅ **SUPPORTED**
  - EvalAI provides user authentication, team management, and API token generation for accessing the platform.
  - Users can authenticate via the web interface or CLI using auth tokens.
  
- **Strategy 2: API Provider Authentication** ❌ **NOT SUPPORTED**
  - EvalAI does not natively integrate with or manage authentication for external API providers (e.g., OpenAI, Anthropic).
  
- **Strategy 3: Repository Authentication** ❌ **NOT SUPPORTED**
  - EvalAI does not natively handle authentication with model or dataset repositories (e.g., Hugging Face, Git LFS).

---

## Phase I: Specification (The Contract)

### Step A: SUT Preparation

- **Strategy 1: Model-as-a-Service (Remote Inference)** ❌ **NOT SUPPORTED**
  - EvalAI does not provide native capabilities for configuring or invoking remote model APIs.
  
- **Strategy 2: Model-in-Process (Local Inference)** ❌ **NOT SUPPORTED**
  - EvalAI does not load models or run inference. Model loading and inference must be implemented in custom evaluation scripts.
  
- **Strategy 3: Algorithm Implementation (In-Memory Structures)** ❌ **NOT SUPPORTED**
  - EvalAI does not instantiate or manage algorithmic data structures.
  
- **Strategy 4: Policy/Agent Instantiation (Stateful Controllers)** ⚠️ **PARTIALLY SUPPORTED**
  - EvalAI supports **code upload challenges** where participants submit Docker images containing agents that are evaluated in environments.
  - However, EvalAI does not provide the environments or agent frameworks—these must be supplied by challenge hosts in their custom evaluation scripts.

### Step B: Benchmark Preparation (Inputs)

- **Strategy 1: Benchmark Dataset Preparation (Offline)** ⚠️ **PARTIALLY SUPPORTED**
  - Challenge hosts upload **test annotation files** (ground truth) when creating challenges.
  - EvalAI stores and provides these to evaluation scripts, but does not perform data loading, splitting, normalization, or formatting.
  
- **Strategy 2: Synthetic Data Generation (Generative)** ❌ **NOT SUPPORTED**
  - No native support for generating synthetic test data.
  
- **Strategy 3: Simulation Environment Setup (Simulated)** ❌ **NOT SUPPORTED**
  - EvalAI does not provide or manage simulation environments. These must be provided by challenge hosts.
  
- **Strategy 4: Production Traffic Sampling (Online)** ❌ **NOT SUPPORTED**
  - No support for sampling or evaluating production traffic.

### Step C: Benchmark Preparation (References)

- **Strategy 1: Judge Preparation** ❌ **NOT SUPPORTED**
  - EvalAI does not train, load, or configure judge models. All evaluation logic must be implemented in custom scripts.
  
- **Strategy 2: Ground Truth Preparation** ⚠️ **PARTIALLY SUPPORTED**
  - Challenge hosts upload ground truth annotations when creating challenges.
  - EvalAI stores these files and passes them to evaluation scripts, but does not perform any pre-computation, indexing, or processing.

---

## Phase II: Execution (The Run)

### Step A: SUT Invocation

- **Strategy 1: Batch Inference** ❌ **NOT SUPPORTED**
  - EvalAI does not execute batch inference. Challenge hosts must implement this in their custom evaluation scripts.
  
- **Strategy 2: Interactive Loop** ⚠️ **PARTIALLY SUPPORTED**
  - For code upload challenges (especially RL challenges), participant Docker containers can interact with environments.
  - However, the interactive loop logic and environment must be provided by the challenge host's evaluation infrastructure.
  
- **Strategy 3: Arena Battle** ❌ **NOT SUPPORTED**
  - No native support for pairwise model comparison or arena battles.
  
- **Strategy 4: Production Streaming** ❌ **NOT SUPPORTED**
  - No support for processing live production traffic.

---

## Phase III: Assessment (The Score)

### Step A: Individual Scoring

- **Strategy 1: Deterministic Measurement** ⚠️ **PARTIALLY SUPPORTED**
  - Challenge hosts implement deterministic metrics (accuracy, F1, BLEU, etc.) in their custom **evaluation scripts**.
  - EvalAI provides the framework to execute these scripts and collect results, but does not provide the metrics themselves.
  
- **Strategy 2: Embedding Measurement** ❌ **NOT SUPPORTED**
  - No native support for embedding-based metrics (BERTScore, semantic similarity, etc.).
  
- **Strategy 3: Subjective Measurement** ❌ **NOT SUPPORTED**
  - No native support for LLM-as-judge or model-based evaluation.
  
- **Strategy 4: Performance Measurement** ❌ **NOT SUPPORTED**
  - No native support for measuring latency, throughput, memory, or other performance metrics.

### Step B: Collective Aggregation

- **Strategy 1: Score Aggregation** ✅ **SUPPORTED**
  - EvalAI aggregates individual scores returned by evaluation scripts into **leaderboard rankings**.
  - Supports multiple metrics, dataset splits, and automatic leaderboard population.
  
- **Strategy 2: Uncertainty Quantification** ❌ **NOT SUPPORTED**
  - No support for confidence intervals, bootstrapping, or uncertainty estimation.

---

## Phase IV: Reporting (The Output)

### Step A: Insight Presentation

- **Strategy 1: Execution Tracing** ❌ **NOT SUPPORTED**
  - No built-in support for displaying step-by-step execution logs or reasoning traces.
  
- **Strategy 2: Subgroup Analysis** ❌ **NOT SUPPORTED**
  - No native support for stratified performance analysis by subgroups.
  
- **Strategy 3: Chart Generation** ❌ **NOT SUPPORTED**
  - EvalAI does not generate charts or visualizations beyond basic leaderboard tables.
  
- **Strategy 4: Dashboard Creation** ✅ **SUPPORTED**
  - EvalAI provides a **web interface** with challenge pages, leaderboard displays, and submission status tracking.
  - Dashboards show metric comparisons, ranked results, and submission history.
  
- **Strategy 5: Leaderboard Publication** ✅ **SUPPORTED**
  - EvalAI's core functionality is **leaderboard management** with public and private leaderboards.
  - Supports automatic ranking, metric display, and challenge phase management.
  
- **Strategy 6: Regression Alerting** ❌ **NOT SUPPORTED**
  - No native support for performance regression detection or alerting.

---

## Conclusion

### Natively Supported Strategies (Core Features)

1. **Git Clone installation** (Phase 0-A-2)
2. **Container Images deployment** (Phase 0-A-3)
3. **Evaluation Platform Authentication** (Phase 0-B-1)
4. **Score Aggregation** (Phase III-B-1)
5. **Dashboard Creation** (Phase IV-A-4)
6. **Leaderboard Publication** (Phase IV-A-5)

### Partially Supported Strategies (Requires Custom Implementation)

1. **Policy/Agent Instantiation** (Phase I-A-4) - via code upload challenges
2. **Benchmark Dataset Preparation** (Phase I-B-1) - stores files, doesn't process them
3. **Ground Truth Preparation** (Phase I-C-2) - stores files, doesn't process them
4. **Interactive Loop** (Phase II-A-2) - framework only, not implementation
5. **Deterministic Measurement** (Phase III-A-1) - framework only, not metrics

### Not Supported (27+ strategies)

All other strategies require custom implementation by challenge hosts or are outside EvalAI's scope as a challenge hosting platform.

### Key Insight

**EvalAI is a challenge hosting and leaderboard management platform, not a comprehensive evaluation harness.** Its primary role is to:

1. Host challenges with multiple phases and datasets
2. Accept participant submissions (predictions or code)
3. Queue and execute custom evaluation scripts
4. Aggregate scores and maintain leaderboards
5. Provide web and CLI interfaces for participants and hosts

The actual evaluation logic—including model loading, inference, metric computation, and specialized evaluation techniques—must be implemented by challenge hosts in their **custom evaluation scripts**. EvalAI provides the infrastructure to run these scripts and display results, but does not provide the evaluation capabilities themselves.
