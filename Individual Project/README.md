Reproducibility Study: Recursive Language Models (RLM)
This repository contains the reproducibility work for the Recursive Language Models (RLM) framework , as part of the FTEC5660: Agentic AI for Business and FinTech course.

The study focuses on the "Needle-in-a-Haystack" (NIAH) task, evaluating the agent's ability to retrieve specific information from a massive 38.5 MB context.

1. Project Overview
Recursive Language Models (RLM) address context window limitations by integrating a Python REPL environment. This allows the agent to iteratively decompose long-context tasks into manageable sub-queries.

This project reproduces the NIAH capability and includes an Ablation Study where the recursive sub-query tool is disabled to observe emergent agentic behavior.

2. Setup & Installation
Prerequisites

Python: 3.10 or higher 


Hardware: Tested on MacBook Air M2 (16GB RAM) 

Installation
Clone the repository:

Install required dependencies:

API Configuration
Create a .env file in the root directory and add your API credentials. Note: Use DeepSeek-V3 for cost-efficiency.


IMPORTANT: Never commit your .env file to GitHub.

3. Running the Experiments
Baseline Run (Recursion Enabled)
The baseline reproduces the original recursive behavior:


Behavior: The agent uses llm_query to recursively scan the 1,000,000-line context.

Modified Run (Recursion Disabled)
To observe the ablation results, the llm_query function is overridden in rlm/repl.py to simulate a depth limit of 1.

Open rlm/repl.py.

Ensure the llm_query function is set to return the recursion error message.

Run the script:


Behavior: The agent pivots to direct Python re (regex) search after receiving the recursion error.

4. Key Results
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

6. Major Findings
The study revealed Emergent Agentic Adaptability. When the primary recursive tool was disabled, the agent autonomously shifted from semantic reasoning to programmatic search using the Python REPL. This demonstrates that code execution environments provide a robust and cost-effective fallback for deterministic retrieval tasks.

Credits
Based on the original  by Alex Zhang.
