# Supported Evaluation Workflow Strategies

This document provides a comprehensive analysis of which strategies from the unified evaluation workflow are supported by EvalAI.

## Classification Criteria

### **✅ Natively Supported**
Steps that meet ALL of the following requirements:
- Available immediately after installing EvalAI
- Requires only import statements and minimal configuration (≤2 lines)
- No external dependencies beyond EvalAI itself
- No custom implementation or glue code required

**Example:**
```python
# EvalAI provides complete implementation out-of-the-box
from evalai import platform
platform.authenticate(token)
```

### **🔌 Supported via Third-Party Integration**
Steps that meet ALL of the following requirements:
- Requires installing ≥1 external package(s)
- Requires glue code (typically ≤10 lines)
- Has documented integration pattern or official example
- Functionality enabled through third-party tools rather than EvalAI alone

**Examples:**
- Integration with external services via documented APIs
- Using third-party libraries with EvalAI-provided integration points

### **❌ Not Supported**
Steps that fall into one of these categories:
- No framework, API, or infrastructure provided by EvalAI
- Requires extensive custom implementation (>10 lines of core logic)
- No documented integration patterns available
- Completely outside EvalAI's scope as a challenge hosting platform

## Summary

EvalAI is a **platform for hosting and managing AI challenges** with leaderboards and participant submissions. It provides infrastructure for challenge hosting but is **not a complete evaluation harness** with built-in evaluation capabilities.

**What EvalAI natively provides:**
- Challenge hosting platform (authentication, submissions, phases, teams)
- File storage and retrieval for datasets and submissions  
- Worker infrastructure to execute custom evaluation scripts
- Score aggregation and leaderboard management
- Web and CLI interfaces

**What requires custom implementation:**
- All evaluation logic within the `evaluate()` function
- Model loading, inference execution, and metric computation
- Specialized evaluation techniques (embeddings, judges, performance monitoring)

**Note on the `evaluate()` API:**
EvalAI provides a standardized function signature `evaluate(test_annotation_file, user_annotation_file, phase_codename, **kwargs)` that challenge hosts must implement. However, this is a contract/interface that hosts must fully implement themselves—it is not a built-in capability and therefore strategies requiring this implementation are classified as "Not Supported" per the native vs. integrated classification framework.

---

## Phase 0: Provisioning (The Runtime)

### Step A: Harness Installation

- **Strategy 1: PyPI Packages** ❌ **Not Supported**
  - EvalAI itself can be installed via PyPI/pip, but it does not act as an evaluation harness that installs and manages evaluation dependencies.
  
- **Strategy 2: Git Clone** ✅ **Natively Supported**
  - EvalAI platform itself can be cloned from GitHub and installed from source for development and deployment.
  - Note: This refers to installing EvalAI infrastructure, not cloning models or evaluation harnesses for evaluation purposes.
  
- **Strategy 3: Container Images** ✅ **Natively Supported**
  - EvalAI provides Docker and Docker Compose configurations for containerized deployment.
  - Docker images are used for both the platform itself and for code upload challenges where participant submissions run in containers.
  
- **Strategy 4: Binary Packages** ❌ **Not Supported**
  - No standalone binary distribution is provided.
  
- **Strategy 5: Node Package** ❌ **Not Supported**
  - Node Package installation refers to distributing evaluation harnesses via npm/npx package managers.
  - While EvalAI uses Node.js for frontend development, the platform is not distributed as a Node.js package (npm) for installation.

### Step B: Service Authentication

- **Strategy 1: Evaluation Platform Authentication** ✅ **Natively Supported**
  - EvalAI provides user authentication, team management, and API token generation for accessing the platform.
  - Users can authenticate via the web interface or CLI using auth tokens.
  
- **Strategy 2: API Provider Authentication** ❌ **Not Supported**
  - EvalAI does not natively integrate with or manage authentication for external API providers (e.g., OpenAI, Anthropic).
  
- **Strategy 3: Repository Authentication** ❌ **Not Supported**
  - EvalAI does not natively handle authentication with model or dataset repositories (e.g., Hugging Face, Git LFS).

---

## Phase I: Specification (The Contract)

### Step A: SUT Preparation

- **Strategy 1: Model-as-a-Service (Remote Inference)** ❌ **Not Supported**
  - EvalAI does not provide native model API integration or configuration.
  - Challenge hosts must implement all remote inference logic themselves in custom evaluation scripts (requires >10 lines of implementation).
  - No documented integration patterns for specific model API providers.
  
- **Strategy 2: Model-in-Process (Local Inference)** ❌ **Not Supported**
  - EvalAI does not provide model loading utilities or inference engines.
  - Challenge hosts must implement all model loading and inference logic themselves in custom evaluation scripts (requires extensive custom code).
  - No built-in support for model frameworks (PyTorch, TensorFlow, etc.).
  
- **Strategy 3: Algorithm Implementation (In-Memory Structures)** ❌ **Not Supported**
  - EvalAI does not provide algorithmic data structures or implementations.
  - Challenge hosts must implement any algorithms (e.g., BM25, FAISS indexes) themselves in custom evaluation scripts.
  
- **Strategy 4: Policy/Agent Instantiation (Stateful Controllers)** ❌ **Not Supported**
  - While EvalAI supports code upload challenges where participants submit Docker images, EvalAI does not provide environments, agent frameworks, or instantiation logic.
  - Challenge hosts must supply all agent evaluation infrastructure themselves (requires extensive custom implementation).

### Step B: Benchmark Preparation (Inputs)

- **Strategy 1: Benchmark Dataset Preparation (Offline)** ❌ **Not Supported**
  - Challenge hosts upload test annotation files, but EvalAI only stores these files.
  - EvalAI does not perform data loading, splitting, normalization, or formatting—all must be implemented in custom evaluation scripts.
  
- **Strategy 2: Synthetic Data Generation (Generative)** ❌ **Not Supported**
  - No native support for generating synthetic test data.
  
- **Strategy 3: Simulation Environment Setup (Simulated)** ❌ **Not Supported**
  - EvalAI does not provide or manage simulation environments. These must be provided by challenge hosts.
  
- **Strategy 4: Production Traffic Sampling (Online)** ❌ **Not Supported**
  - No support for sampling or evaluating production traffic.

### Step C: Benchmark Preparation (References)

- **Strategy 1: Judge Preparation** ❌ **Not Supported**
  - EvalAI does not train, load, or configure judge models. All evaluation logic must be implemented in custom scripts.
  
- **Strategy 2: Ground Truth Preparation** ❌ **Not Supported**
  - Challenge hosts upload ground truth annotation files, but EvalAI only stores and passes these files to evaluation scripts.
  - Does not perform any pre-computation, indexing, embedding generation, or other processing—all must be implemented in custom evaluation scripts.

---

## Phase II: Execution (The Run)

### Step A: SUT Invocation

- **Strategy 1: Batch Inference** ❌ **Not Supported**
  - EvalAI does not execute batch inference itself.
  - Challenge hosts must implement all inference logic, model loading, and batch processing in custom evaluation scripts (requires extensive custom implementation).
  
- **Strategy 2: Interactive Loop** ❌ **Not Supported**
  - For code upload challenges, participant Docker containers can run custom code, but EvalAI does not provide interactive loop logic or environments.
  - Challenge hosts must provide all interactive loop logic and environment infrastructure themselves.
  
- **Strategy 3: Arena Battle** ❌ **Not Supported**
  - No native support for pairwise model comparison or arena battles.
  - No documented framework or utilities for implementing this.
  
- **Strategy 4: Production Streaming** ❌ **Not Supported**
  - No support for processing live production traffic.

---

## Phase III: Assessment (The Score)

### Step A: Individual Scoring

- **Strategy 1: Deterministic Measurement** ❌ **Not Supported**
  - EvalAI does not provide metric implementations (accuracy, F1, BLEU, edit distance, etc.).
  - Challenge hosts must implement all deterministic metrics themselves in custom evaluation scripts (requires custom implementation).
  - EvalAI only collects scores returned by these custom scripts and passes them to the aggregation layer.
  
- **Strategy 2: Embedding Measurement** ❌ **Not Supported**
  - EvalAI does not provide embedding models, similarity computation utilities, or pre-computed embeddings.
  - Challenge hosts must implement all embedding-based metrics (BERTScore, semantic similarity, etc.) themselves in custom evaluation scripts.
  
- **Strategy 3: Subjective Measurement** ❌ **Not Supported**
  - EvalAI does not provide judge models, prompt templates, or subjective evaluation utilities.
  - Challenge hosts must implement all LLM-as-judge or model-based evaluation logic themselves in custom evaluation scripts.
  
- **Strategy 4: Performance Measurement** ❌ **Not Supported**
  - EvalAI does not provide native performance monitoring, profiling tools, or automated resource tracking.
  - Challenge hosts must implement timing operations and resource usage collection themselves in custom evaluation scripts.

### Step B: Collective Aggregation

- **Strategy 1: Score Aggregation** ✅ **Natively Supported**
  - EvalAI aggregates individual scores returned by evaluation scripts into **leaderboard rankings**.
  - Supports multiple metrics, dataset splits, and automatic leaderboard population.
  
- **Strategy 2: Uncertainty Quantification** ❌ **Not Supported**
  - No support for confidence intervals, bootstrapping, or uncertainty estimation.

---

## Phase IV: Reporting (The Output)

### Step A: Insight Presentation

- **Strategy 1: Execution Tracing** ❌ **Not Supported**
  - No built-in support for displaying step-by-step execution logs or reasoning traces.
  
- **Strategy 2: Subgroup Analysis** ❌ **Not Supported**
  - No native support for stratified performance analysis by subgroups.
  
- **Strategy 3: Chart Generation** ❌ **Not Supported**
  - EvalAI does not generate charts or visualizations beyond basic leaderboard tables.
  
- **Strategy 4: Dashboard Creation** ✅ **Natively Supported**
  - EvalAI provides a **web interface** with challenge pages, leaderboard displays, and submission status tracking.
  - Dashboards show metric comparisons, ranked results, and submission history.
  
- **Strategy 5: Leaderboard Publication** ✅ **Natively Supported**
  - EvalAI's core functionality is **leaderboard management** with public and private leaderboards.
  - Supports automatic ranking, metric display, and challenge phase management.
  
- **Strategy 6: Regression Alerting** ❌ **Not Supported**
  - No native support for performance regression detection or alerting.

---

## Conclusion

### Natively Supported Strategies (6 total)

**Platform Infrastructure:**
1. **Git Clone installation** (Phase 0-A-2)
2. **Container Images deployment** (Phase 0-A-3)
3. **Evaluation Platform Authentication** (Phase 0-B-1)
4. **Score Aggregation** (Phase III-B-1)
5. **Dashboard Creation** (Phase IV-A-4)
6. **Leaderboard Publication** (Phase IV-A-5)

### Supported via Third-Party Integration (0 strategies)

EvalAI does not provide documented integration patterns for third-party tools that would qualify under this category. While challenge hosts can use external libraries in their custom evaluation scripts, these require extensive custom implementation rather than simple glue code (≤10 lines).

### Not Supported (28 strategies)

All other strategies require extensive custom implementation by challenge hosts or are outside EvalAI's scope as a challenge hosting platform. This includes:

- **Phase 0**: PyPI packages, binary packages, Node packages, API provider auth, repository auth (5 strategies)
- **Phase I**: All SUT preparation, benchmark preparation, and ground truth preparation strategies (10 strategies)
- **Phase II**: All SUT invocation strategies (4 strategies)
- **Phase III**: All individual scoring strategies (4 strategies), uncertainty quantification (1 strategy)
- **Phase IV**: Execution tracing, subgroup analysis, chart generation, regression alerting (4 strategies)

### Key Insight

**EvalAI is a challenge hosting platform, not an evaluation harness.** Its role is to:

1. **Host challenges** - Manage challenge configuration, phases, and participant teams
2. **Store data** - Store test annotations and participant submissions
3. **Execute scripts** - Run custom evaluation scripts provided by challenge hosts
4. **Aggregate results** - Collect scores from evaluation scripts and compute leaderboard rankings
5. **Display leaderboards** - Present results through web and CLI interfaces

**What EvalAI does NOT provide:**
- Built-in evaluation metrics or scoring logic
- Model loading or inference capabilities
- Data preprocessing or transformation utilities
- Third-party integrations with documented patterns

Challenge hosts must implement all evaluation logic themselves within the `evaluate(test_annotation_file, user_annotation_file, phase_codename, **kwargs)` function. This function is a contract/interface that hosts fully implement—it is not a built-in capability with implementations that can be imported and configured with ≤2 lines of code.

### Classification Summary

- **✅ Natively Supported**: 6 strategies (18%)
- **🔌 Supported via Third-Party Integration**: 0 strategies (0%)
- **❌ Not Supported**: 28 strategies (82%)

**Total**: 34 strategies across 5 workflow phases
