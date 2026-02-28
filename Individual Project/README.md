# Reproducibility Study: Recursive Language Models (RLM)

This repository contains the reproducibility work for the **Recursive Language Models (RLM)** framework, submitted as part of the **FTEC5660: Agentic AI for Business and FinTech** course.

The study focuses on the "Needle-in-a-Haystack" (NIAH) task, evaluating the agent's ability to retrieve specific information from a massive **38.5 MB** context.

## 1. Project Overview
Recursive Language Models (RLM) address context window limitations by integrating a **Python REPL environment**. This allows the agent to iteratively decompose long-context tasks into manageable sub-queries. 

This project reproduces the NIAH capability and includes an **Ablation Study** where the recursive sub-query tool is disabled to observe emergent agentic behavior.

---

## 2. Setup & Installation

### Prerequisites
* **Python**: 3.10 or higher
* **Hardware**: Tested on MacBook Air M2 (16GB Unified Memory)

### Installation
1. **Clone the repository**:
   ```
   git clone <your-github-repo-link>
   cd rlm-minimal
   、、、
   
2. **Setup & Installation (Continued)**

[cite_start]**Install required dependencies[cite: 13]:**
```
pip install openai python-dotenv rich [cite: 14, 15, 16]
、、、

API Configuration:
Create a .env file in the root directory and add your API credentials.
Note: We recommend using DeepSeek-V3 for its high cost-efficiency and OpenAI-compatible interface.

OPENAI_API_KEY=your_api_key_here 
OPENAI_BASE_URL=[https://api.deepseek.com/v1](https://api.deepseek.com/v1) [cite: 19]

---

## 3. Running the Experiments

### Phase 1: Baseline Run (Recursion Enabled)
[cite_start]The baseline reproduces the original recursive behavior where the agent uses sub-LLM calls to navigate the context[cite: 36].
```bash
python main.py

```
**Expected Behavior:** The agent identifies the large context (approx. 38.5 MB) and uses `llm_query` to recursively scan the 1,000,000-line file.

### Phase 2: Modified Run (Recursion Disabled)

To observe the ablation results, the `llm_query` tool is overridden to simulate a depth limit of 1, forcing the agent to find alternative strategies.

1. **Open `rlm/repl.py`.**
2. 
**Modify `llm_query`:** Override the function to return: `"Error: Maximum recursion depth reached (Max Depth = 1). The sub-query tool is disabled."`.
3. **Run the script:**

```
python main.py
```

Expected Behavior: The agent receives the recursion error and autonomously pivots to a direct Python re (regex) search using the REPL environment to locate the magic number.

---

## 4. Key Results
Reproduction Performance (Table 1)
#### Table 1: Reproduction Performance Comparison
| Metric | Reported Results | My Reproduction |
| :--- | :--- | :--- |
| **Task Objective** | 1M context NIAH | 1M context NIAH |
| **Success / Accuracy** | Claimed Success | **Success (Found 6786234)** |
| **Data Volume** | ~38 MB | **38,507,554 characters** |
| **Reasoning Path** | Multi-step Recursion | **10 Steps** |

Comparative Analysis (Table 2)
#### Table 2: Comparative Analysis (Baseline vs. Modified)
| Metric | Baseline (Recursion Enabled) | Modified (Recursion Disabled) |
| :--- | :--- | :--- |
| **Final Result** | Success | **Success** |
| **Reasoning Steps** | 10 steps | **2 steps** |
| **API Cost** | ~$0.10 | **~$0.03** |
| **Solution Strategy** | Semantic Recursion | **Python REPL (Regex)** |

---

## 5. Major Findings: Emergent Agentic Adaptability

The study revealed significant **Emergent Agentic Adaptability** in resource-constrained environments. When the primary recursive tool was disabled, the agent demonstrated high tool-use flexibility:

* 
**Strategy Pivot:** It autonomously shifted from "semantic reasoning" to "programmatic search" using the Python `re` module to scan the local context string directly.

* 
**Efficiency Gain:** This shift resulted in an **80% reduction in reasoning steps** and a **70% reduction in API costs**.
  
* 
**Robustness:** Programmatic search via code execution proved significantly more robust and cost-effective than recursive semantic analysis for deterministic retrieval tasks.
  
