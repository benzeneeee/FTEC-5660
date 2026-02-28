# Reproducibility Study: Recursive Language Models (RLM)

This repository contains the reproducibility work for the **Recursive Language Models (RLM)** framework, submitted as part of the **FTEC5660: Agentic AI for Business and FinTech** course.

The study focuses on the "Needle-in-a-Haystack" (NIAH) task, evaluating the agent's ability to retrieve specific information from a massive **38.5 MB** context.

## 1. Project Overview

Recursive Language Models (RLM) is an innovative framework designed to bypass the inherent context window limitations of traditional Large Language Models (LLMs). The core philosophy of RLM involves the integration of a Python REPL (Read-Eval-Print Loop) environment, which empowers the agent to interactively explore, transform, and analyze massive datasets. By employing a recursive task-decomposition mechanism, the agent can "divide and conquer" long-context challenges—breaking down a high-token task into smaller, manageable sub-queries that are processed iteratively.

The primary objective of this reproducibility study is to verify the "Needle-in-a-Haystack" (NIAH) capability of the RLM framework. The task involves locating and extracting a specific "magic number" hidden within a massive, randomly generated text file. For this experiment, a context of approximately 38.5 MB (roughly 1 million lines) was used to evaluate the agent's ability to successfully retrieve a single data point. This project also includes an **Ablation Study** where the recursive sub-query tool is disabled to observe emergent agentic behavior.

---

## 2. Setup & Installation

### Prerequisites

* **Python**: 3.10 or higher.
* **Hardware**: MacBook Air with an M2 chip and 16GB of Unified Memory.

### Installation

1. **Clone the repository**:
```bash
git clone <your-github-repo-link>
cd rlm-minimal

```


2. **Install required dependencies**:
```bash
pip install openai python-dotenv rich

```



### API Configuration

To optimize cost-efficiency and reasoning performance, DeepSeek-V3 was selected as the underlying model. Access was facilitated through a third-party API aggregator using an OpenAI-compatible interface.

Create a `.env` file in the root directory and add your API credentials:

```env
OPENAI_API_KEY=your_api_key_here
OPENAI_BASE_URL=https://api.deepseek.com/v1

```

---

## 3. Running the Experiments

### Phase 1: Baseline Run (Recursion Enabled)

The baseline reproduces the original recursive behavior where the agent uses sub-LLM calls to navigate the context.

```bash
python main.py

```

**Expected Behavior:** The agent identifies the large context (approx. 38.5 MB) and uses `llm_query` to recursively scan the 1,000,000-line file.

### Phase 2: Modified Run (Recursion Disabled)

To observe the ablation results, the `llm_query` tool is overridden to simulate a depth limit of 1, forcing the agent to find alternative strategies.

1. **Open `rlm/repl.py`.**
2. **Modify `llm_query`:** Override the function to return: `"Error: Maximum recursion depth reached (Max Depth = 1). The sub-query tool is disabled."`.
3. **Run the script:**

```bash
python main.py

```

**Expected Behavior:** The agent receives the recursion error and autonomously pivots to a direct Python re (regex) search using the REPL environment to locate the magic number.

---

## 4. Key Results

#### Table 1: Reproduction Performance Comparison

| Metric | Reported Results | My Reproduction |
| --- | --- | --- |
| **Task Objective** | 1M context NIAH | 1M context NIAH |
| **Success / Accuracy** | Claimed Success | **Success (Found 6786234)** |
| **Data Volume** | ~38 MB | **38,507,554 characters** |
| **Reasoning Path** | Multi-step Recursion | **10 Steps** |

#### Table 2: Comparative Analysis (Baseline vs. Modified)

| Metric | Baseline (Recursion Enabled) | Modified (Recursion Disabled) |
| --- | --- | --- |
| **Final Result** | Success | **Success** |
| **Reasoning Steps** | 10 steps | **2 steps** |
| **API Cost** | ~$0.10 | **~$0.03** |
| **Solution Strategy** | Semantic Recursion | **Python REPL (Regex)** |

---

## 5. Major Findings: Emergent Agentic Adaptability

The study revealed significant **Emergent Agentic Adaptability** in resource-constrained environments. When the primary recursive tool was disabled, the agent demonstrated high tool-use flexibility:

* **Strategy Pivot:** It autonomously shifted from "semantic reasoning" to "programmatic search" using the Python `re` module to scan the local context string directly.
* **Efficiency Gain:** This shift resulted in an **80% reduction in reasoning steps** and a **70% reduction in API costs**.
* **Robustness:** Programmatic search via code execution proved significantly more robust and cost-effective than recursive semantic analysis for deterministic retrieval tasks.
  
