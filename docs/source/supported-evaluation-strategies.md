# Supported Evaluation Workflow Strategies

This document provides a comprehensive analysis of which strategies from the unified evaluation workflow are natively supported by EvalAI. A strategy is considered "supported" only if EvalAI provides it natively in its full installation—meaning that once EvalAI is fully installed, the strategy can be executed directly without implementing custom modules or integrating external libraries.

## Summary

EvalAI is a **platform for hosting and managing AI challenges** with leaderboards and participant submissions. It provides an **evaluation API framework** through the `evaluate()` function that challenge hosts must implement. EvalAI is **not a complete evaluation harness** that provides end-to-end evaluation capabilities natively.

**What EvalAI provides:**
- A standardized API (`evaluate()` function) for evaluation scripts
- Infrastructure to execute these scripts (workers, Docker containers, file storage)
- Score aggregation and leaderboard management
- Challenge hosting platform (authentication, submissions, phases)

**What challenge hosts must implement:**
- All evaluation logic within their `evaluate()` function
- Model loading, inference execution, and metric computation
- Any specialized evaluation techniques (embeddings, judges, performance monitoring)

---

## Phase 0: Provisioning (The Runtime)

### Step A: Harness Installation

- **Strategy 1: PyPI Packages** ❌ **NOT SUPPORTED**
  - EvalAI itself can be installed via PyPI/pip, but it does not act as an evaluation harness that installs and manages evaluation dependencies.
  
- **Strategy 2: Git Clone** ✅ **SUPPORTED**
  - EvalAI platform itself can be cloned from GitHub and installed from source for development and deployment.
  - Note: This refers to installing EvalAI infrastructure, not cloning models or evaluation harnesses for evaluation purposes.
  
- **Strategy 3: Container Images** ✅ **SUPPORTED**
  - EvalAI provides Docker and Docker Compose configurations for containerized deployment.
  - Docker images are used for both the platform itself and for code upload challenges where participant submissions run in containers.
  
- **Strategy 4: Binary Packages** ❌ **NOT SUPPORTED**
  - No standalone binary distribution is provided.
  
- **Strategy 5: Node Package** ❌ **NOT SUPPORTED**
  - Node Package installation refers to distributing evaluation harnesses via npm/npx package managers.
  - While EvalAI uses Node.js for frontend development, the platform is not distributed as a Node.js package (npm) for installation.

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

- **Strategy 1: Model-as-a-Service (Remote Inference)** ⚠️ **PARTIALLY SUPPORTED**
  - EvalAI provides an API framework where challenge hosts can implement remote model inference in their evaluation scripts.
  - The `evaluate()` function receives user submission files and can make API calls to remote models, but EvalAI does not provide native model API integration or configuration.
  - Challenge hosts must implement all remote inference logic themselves.
  
- **Strategy 2: Model-in-Process (Local Inference)** ⚠️ **PARTIALLY SUPPORTED**
  - EvalAI provides an API framework through the `evaluate()` function where challenge hosts can load models and run local inference.
  - The evaluation worker executes custom evaluation scripts that can load model weights and perform inference, but EvalAI does not provide model loading utilities or inference engines.
  - Challenge hosts must implement all model loading and inference logic themselves.
  
- **Strategy 3: Algorithm Implementation (In-Memory Structures)** ⚠️ **PARTIALLY SUPPORTED**
  - EvalAI's evaluation script framework allows challenge hosts to instantiate and use algorithmic data structures within the `evaluate()` function.
  - Challenge hosts can implement any algorithm (e.g., BM25, FAISS indexes) in their evaluation scripts, but EvalAI does not provide these algorithms natively.
  
- **Strategy 4: Policy/Agent Instantiation (Stateful Controllers)** ⚠️ **PARTIALLY SUPPORTED**
  - EvalAI supports **code upload challenges** where participants submit Docker images containing agents that are evaluated in environments.
  - Challenge hosts can implement agent evaluation logic in their evaluation scripts and Docker containers.
  - However, EvalAI does not provide the environments, agent frameworks, or instantiation logic—these must be supplied by challenge hosts.

### Step B: Benchmark Preparation (Inputs)

- **Strategy 1: Benchmark Dataset Preparation (Offline)** ⚠️ **PARTIALLY SUPPORTED**
  - Challenge hosts upload **test annotation files** (ground truth labels/answers for the test set) when creating challenges.
  - Participants submit predictions/outputs that are compared against these annotations.
  - EvalAI stores and provides these files to evaluation scripts, but does not perform data loading, splitting, normalization, or formatting—these must be implemented in custom evaluation scripts.
  
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
  - Challenge hosts upload ground truth annotation files (reference answers, labels, correct outputs) when creating challenges.
  - EvalAI stores these files and passes them to evaluation scripts for comparison with participant submissions.
  - Does not perform any pre-computation, indexing, embedding generation, or other processing—these must be implemented in custom evaluation scripts.
  - Note: While both I-B-1 and I-C-2 involve storing annotation files, the conceptual distinction is that I-B-1 refers to the benchmark dataset as inputs, while I-C-2 refers to the reference materials used for scoring in Phase III.

---

## Phase II: Execution (The Run)

### Step A: SUT Invocation

- **Strategy 1: Batch Inference** ⚠️ **PARTIALLY SUPPORTED**
  - EvalAI provides an API framework through the `evaluate()` function that receives user submission files and test annotations.
  - Challenge hosts can implement batch inference logic within their evaluation scripts to process multiple test instances.
  - However, EvalAI does not execute batch inference itself—challenge hosts must implement all inference logic, model loading, and batch processing.
  
- **Strategy 2: Interactive Loop** ⚠️ **PARTIALLY SUPPORTED**
  - For code upload challenges (especially RL challenges), participant Docker containers can interact with environments.
  - Challenge hosts can implement interactive evaluation loops in their evaluation scripts and Docker containers.
  - However, the interactive loop logic and environment must be provided by the challenge host's evaluation infrastructure.
  
- **Strategy 3: Arena Battle** ❌ **NOT SUPPORTED**
  - No native support for pairwise model comparison or arena battles.
  - Challenge hosts could implement arena battles in custom evaluation scripts, but EvalAI provides no framework or utilities for this.
  
- **Strategy 4: Production Streaming** ❌ **NOT SUPPORTED**
  - No support for processing live production traffic.

---

## Phase III: Assessment (The Score)

### Step A: Individual Scoring

- **Strategy 1: Deterministic Measurement** ⚠️ **PARTIALLY SUPPORTED**
  - EvalAI provides an **API framework** (`evaluate()` function) that receives test annotations and user submissions for scoring.
  - Challenge hosts implement deterministic metrics (accuracy, F1, BLEU, edit distance, etc.) within their evaluation scripts.
  - EvalAI executes these scripts, collects individual scores, and passes them to the aggregation layer, but does not provide metric implementations.
  
- **Strategy 2: Embedding Measurement** ⚠️ **PARTIALLY SUPPORTED**
  - Challenge hosts can implement embedding-based metrics (BERTScore, semantic similarity, etc.) in their evaluation scripts.
  - The evaluation framework allows loading embedding models and computing similarity scores within the `evaluate()` function.
  - However, EvalAI does not provide native embedding models, similarity computation utilities, or pre-computed embeddings.
  
- **Strategy 3: Subjective Measurement** ⚠️ **PARTIALLY SUPPORTED**
  - Challenge hosts can implement LLM-as-judge or model-based evaluation in their evaluation scripts.
  - The evaluation framework allows calling judge models (local or remote) within the `evaluate()` function to assess subjective quality.
  - However, EvalAI does not provide judge models, prompt templates, or subjective evaluation utilities natively.
  
- **Strategy 4: Performance Measurement** ⚠️ **PARTIALLY SUPPORTED**
  - Challenge hosts can measure performance metrics (latency, throughput, memory) within their evaluation scripts.
  - The evaluation framework allows timing operations and collecting resource usage data within the `evaluate()` function.
  - However, EvalAI does not provide native performance monitoring, profiling tools, or automated resource tracking.

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

### Partially Supported Strategies (API Framework Provided, Implementation Required)

**SUT Preparation (Phase I-A):**
1. **Model-as-a-Service** (I-A-1) - API framework for remote inference
2. **Model-in-Process** (I-A-2) - API framework for local inference
3. **Algorithm Implementation** (I-A-3) - API framework for algorithms
4. **Policy/Agent Instantiation** (I-A-4) - Docker container framework for agents

**Benchmark Preparation (Phase I-B & I-C):**
5. **Benchmark Dataset Preparation** (I-B-1) - File storage, no processing
6. **Ground Truth Preparation** (I-C-2) - File storage, no processing

**SUT Invocation (Phase II-A):**
7. **Batch Inference** (II-A-1) - API framework for batch processing
8. **Interactive Loop** (II-A-2) - Docker container framework for RL

**Individual Scoring (Phase III-A):**
9. **Deterministic Measurement** (III-A-1) - API framework for metrics
10. **Embedding Measurement** (III-A-2) - API framework for embeddings
11. **Subjective Measurement** (III-A-3) - API framework for judges
12. **Performance Measurement** (III-A-4) - API framework for profiling

### Not Supported (16 strategies)

All other strategies require custom implementation or are outside EvalAI's scope as a challenge hosting platform.

### Key Insight

**EvalAI provides an evaluation API framework, not a complete evaluation harness.** Its primary role is to:

1. **Provide the `evaluate()` API** - A standardized function signature that challenge hosts implement
2. **Execute evaluation scripts** - Workers that call the `evaluate()` function with test data and user submissions
3. **Manage infrastructure** - File storage, submission queueing, Docker container execution for code upload
4. **Aggregate and display results** - Collect scores from evaluation scripts and populate leaderboards
5. **Host challenges** - Web and CLI interfaces for participants and hosts

The actual evaluation logic—including model loading, inference, metric computation, and specialized evaluation techniques—must be implemented by challenge hosts in their **custom evaluation scripts** using the API framework EvalAI provides. EvalAI executes these scripts and displays results, but does not provide the evaluation implementations themselves.
