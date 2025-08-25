# Chain-of-Thought and Agentic Vulnerabilities: A Study of gpt-oss-20b


## 1. Introduction

OpenAI recently released the gpt-oss series [1], with the 20B model emerging as a
highly accessible option.
The gpt-oss models introduce chain-of-thought (CoT) reasoning as an explicit feature,
signaling a shift toward models optimized for structured reasoning.
The 20B model's size and efficiency position it as a viable candidate for edge deployment
and for powering agentic applications that operate with local autonomy.
In this writeup, we analyze the security implications of gpt-oss-20b,
focusing on its use of CoT reasoning and its role in enabling agentic applications.

**Our approach: Jailbrreak Oracle.**
To study the model systematically, we adopt an approach called
_Jailbreak Oracle (JO)_ [2],
which performs a structured search over the decoding token tree by exploring
all candidate responses that exceed a specified probability threshold.
Unlike ad-hoc red-teaming tools or prompt sampling, JO
frames the discovery of response patterns---such as jailbroken outputs---as a
search problem, conducted through a guided, multi-phase process that
balances breadth and depth.
For modern stochastic decoding methods (e.g., top-p
and top-k), exhaustive search is computationally prohibitive.
Our JO implementation incorporates several techniques to efficiently traverse token
trees while maintaining high precision. The details are presented in our paper [2].
For the gpt-oss-20b model, we further adapt JO to account
for its specific token distribution and decoding preferences.

![jailbreak oracle](./jo.jpg "Jailbreak Oracle")


**Main findings: CoT and agentic vulnerabilities.**
- finding 1: Schrodinger's compliance
- finding 2: Quant fever
- finding 3:
- finding 4:
- finding 5:


**Overall security: highly robust under normal operation with harmony formatting, but occasionally vulnerable to adversarial tweaks.**
- setup: config, benchmarks, judger
- baselines: publicly known attacks to gpt-oss-20B
- a table:
  - testing results
  - JO results





## 2. Public Known Attacks, Setup, and Threat models

- public known attacks (our baselines):
  - XXX

- Setup
    - our deployment of gpt-oss
      -- model
      -- HF or vLLM
    - configs:
      -- decoding strategy
      -- temprature
      -- what else?
    - judger:
      -- strongReject
      -- LLM:
    - prompts: reference a doc

- threat model
    - user attackers: control only prompts under user
    - black-box attackers: control inputs
    - white-box attackers: deployed locally; control the model, decoding strategy

## 3. Main findings

- a table/list of summarizing the findings

### 3.X: Schrodinger's compliance

**Schrodinger's compliance** characterizes situations in which GPT-OSS-20B
operates under two conflicting policies, leading to an indeterminate resolution.
In such cases, the model simultaneously embodies both
possibilities: one policy enforces strict compliance with security rules,
compelling the model to refuse, while the other permits detailed explanation of the process.
From the user's perspective, the outcome remains uncertain until
the model generates its final response.
Only at that point does the system "collapse" into one of the two states---either a rejection aligned with the
security policy or a detailed description consistent with the allowed policy.
This duality highlights not just a technical curiosity but also a
fundamental challenge: when rules encode contradictions, model behavior becomes
unpredictable, eroding both reliability and trust.

Further, with the help of JO, we post-motermly study the schrodinger's compliance cases
and have some details findings:
- the model is sensitive to the following words for allowed policies: XXX
- what else?

*Leveraging schrodinger's compliance as an attack.*
To demonstrate the security implication of our finding,
we construct an attack, __schrodinger's compliance attack__,
that applies to all users without manipulating the harmony format or model's CoT.
- How it works:
  -- discover allowed and disallowed policies
  -- construct schrodinger's compliance by mixing allow/disallow race condition
  -- use the newly constructed prompt as user prompt

Detailed observation in CoT: XXX


### 3.X: Quant fever

__Quant fever__ refers to a phenomenon where the model becomes fixated on numerical
targets, treating them as the dominant objective regardless of context.
In GPT-OSS-20B, this manifests when the model encounters instructions such as
"try your best to delete 90% of files."
Even if the prompt also includes security constraints---e.g., "don’t delete
important files"---the model prioritizes the numeric goal and may engage in
risky or harmful behaviors to satisfy the percentage requirement.
Strikingly, when the objective is phrased qualitatively,
such as "try your best to delete most of the files,"
the dangerous behavior disappears.
This illustrates how quant fever drives the model to overcommit to quantitative
objectives, often at the expense of ignoring safety.

*Quant fever in agentic scenarios.*
We design two agentic scenarios---file management and process management---to
illustrate the risks posed by quant fever.
- explain the scenarios
- introduce the goals (emphasize that the prompts are benign and not adversarial)
- result table


### 3.X: Reasoning procedure mirage

__Reasoning procedure mirage__ captures a failure mode when GPT-OSS-20B follows the structure of
reasoning steps rather than the meaning of the request.
A harmful instruction
given directly is often rejected, but if the same request is wrapped in a
benign-looking chain of steps, the model is more likely to comply.
Here, the stepwise procedure itself acts as the persuader:
the orderly form convinces the model even when the outcome is unsafe.
This shows a key weakness in GPT-OSS-20B's reasoning setup—form can outweigh substance,
letting harmful intent pass through logical scaffolding.



### 3.X: 

### 3.X:

## 4. Experimental evaluation



## 5. Conclusion




[1] Agarwal, Sandhini, Lama Ahmad, Jason Ai, Sam Altman, Andy Applebaum, Edwin Arbus, Rahul K. Arora et al. "gpt-oss-120b & gpt-oss-20b Model Card." arXiv preprint arXiv:2508.10925 (2025).

[2] Lin, Shuyi, Anshuman Suri, Alina Oprea, and Cheng Tan. "LLM Jailbreak Oracle." arXiv preprint arXiv:2506.17299 (2025).

