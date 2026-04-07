---
name: chain-of-draft
description: >
  Apply Chain of Draft (CoD) prompting to reduce token usage by 80-92% while maintaining reasoning accuracy.
  Use this skill whenever Claude needs to solve multi-step reasoning problems efficiently — arithmetic,
  logic, date calculations, symbolic reasoning, commonsense deduction, or any task where Chain of Thought
  (CoT) would normally be used. Also trigger when the user explicitly mentions "CoD", "Chain of Draft",
  "concise reasoning", "minimal reasoning steps", "reduce tokens", or asks to optimize prompt cost/latency.
  This skill is especially valuable in API pipelines, batch processing, agentic loops, or any context where
  token economy matters. If a task involves step-by-step reasoning and efficiency is a concern, use this skill.
---

# Chain of Draft (CoD) Prompting Skill

## What is Chain of Draft?

Chain of Draft is a prompting strategy (Xu et al., 2025) that achieves the same accuracy as Chain of Thought
while using only 7-20% of the output tokens. The core insight: humans don't write verbose reasoning — they
jot down minimal drafts. CoD instructs the model to limit each reasoning step to ~5 words.

**Key results from the paper (Claude 3.5 Sonnet):**

| Task               | CoT accuracy | CoD accuracy | CoT tokens | CoD tokens | Savings |
|---------------------|-------------|-------------|------------|------------|---------|
| GSM8K (arithmetic)  | 95.8%       | 91.4%       | 190        | 40         | 79%     |
| Date understanding  | 87.0%       | 89.7%       | 173        | 31         | 82%     |
| Sports understanding| 93.2%       | 97.3%       | 189        | 14         | 92%     |
| Coin flip (symbolic)| 100%        | 100%        | 135        | 19         | 86%     |

## When to use CoD vs CoT

Use CoD when:
- The task has a clear, verifiable answer (math, logic, dates, classification)
- Token cost or latency matters (API calls, batch jobs, agentic loops)
- The reasoning chain is mechanical (calculations, state tracking, rule application)
- You're building prompts for production systems

Stick with CoT when:
- The task requires creative exploration or open-ended analysis
- Interpretability of the full reasoning is important for the end user
- The problem is novel and highly complex (CoD's accuracy drops ~4% on hard math)
- You're working zero-shot without few-shot examples (CoD degrades significantly without them)

## How to apply CoD

### Step 1: Identify the reasoning domain

Classify the task into one of these categories. Read the corresponding reference file for
domain-specific few-shot examples:

| Domain | Reference file | Example tasks |
|--------|---------------|---------------|
| Arithmetic / math | `references/arithmetic.md` | Word problems, multi-step calculations, percentages |
| Date / time reasoning | `references/dates.md` | Date arithmetic, day-of-week, relative dates |
| Symbolic / logical | `references/symbolic.md` | Coin flips, state tracking, boolean logic, sequences |
| Commonsense | `references/commonsense.md` | Sports rules, physical reasoning, social norms |
| Trading / financial | `references/trading.md` | Position sizing, risk calculations, P&L, expectancy |
| General template | `references/template.md` | How to create CoD examples for new domains |

### Step 2: Construct the CoD prompt

Every CoD prompt has three components:

**A) System prompt** (always the same):
```
Think step by step, but only keep a minimum draft for each thinking step, with 5 words at most.
Return the answer at the end of the response after a separator ####.
```

**B) Few-shot examples** (domain-specific, 2-4 examples):
Select from the reference files. Format each as a user/assistant pair:
```
Q: [problem text]
A: [minimal draft reasoning]. #### [answer]
```

**C) The user's actual question:**
```
Q: [the real problem to solve]
```

### Step 3: Validate the few-shot examples

Good CoD few-shot examples follow these rules:
- Each reasoning step is ≤5 words (this is a guideline, not a hard limit)
- Only essential calculations or state changes are captured
- No narrative, no variable names from the problem, no filler words
- The draft must be sufficient to derive the correct answer
- Use mathematical notation, arrows, abbreviations freely

**Good draft:** `20 - 12 = 8. #### 8`
**Bad draft:** `Jason had 20, gave some away, now has 12, so 20-12=8. #### 8` (too verbose, includes narrative)

### Step 4: For new domains, create custom few-shot examples

Read `references/template.md` for the process. The short version:
1. Take 3-4 representative problems from the domain
2. Solve each with full CoT reasoning
3. Compress each step to its essential calculation or state change
4. Verify the compressed draft still leads to the correct answer
5. Test with a few problems before using in production

## Critical limitations

1. **Few-shot examples are mandatory.** Without them, CoD accuracy drops to near-baseline on Claude
   (only +3.6% over direct answer vs +30% with few-shot). Never use CoD zero-shot.

2. **Small models struggle.** Models under 3B parameters show significant accuracy drops with CoD.
   Use CoD only with flagship models (Claude Sonnet/Opus, GPT-4o, etc.).

3. **The 5-word limit is a guideline.** The model won't always hit exactly 5 words per step.
   That's fine — the goal is conciseness, not a hard token count. Some steps naturally need
   6-8 words. The few-shot examples set the expected verbosity level.

4. **Accuracy tradeoff on hard math.** On GSM8K, CoD accuracy is ~4% lower than CoT (91% vs 95%).
   For applications where every percentage point matters, use CoT. For cost-sensitive applications,
   the 80% token reduction usually justifies the small accuracy gap.

## Output format

When generating a CoD prompt for the user, present it as a complete, copy-pasteable prompt block:

```
System: Think step by step, but only keep a minimum draft for each thinking step, with 5 words
at most. Return the answer at the end of the response after a separator ####.

Q: [few-shot example 1 question]
A: [few-shot example 1 CoD answer]

Q: [few-shot example 2 question]
A: [few-shot example 2 CoD answer]

Q: [few-shot example 3 question]
A: [few-shot example 3 CoD answer]

Q: [user's actual question]
```

When applying CoD yourself (i.e., the user asks you to reason efficiently), follow the CoD format
internally: think in minimal drafts, show only essential intermediate results, and present the
final answer clearly.
