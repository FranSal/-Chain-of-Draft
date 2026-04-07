# Chain of Draft — Prompting Skill for Claude

> **Think step by step, but only keep a minimum draft.**

A prompting skill that implements [Chain of Draft (CoD)](https://arxiv.org/abs/2502.18600) — a technique that achieves the same accuracy as Chain of Thought while using **only 7–20% of the output tokens**.

## What is Chain of Draft?

Chain of Draft (Xu et al., 2025) is a prompting strategy inspired by how humans actually reason: we jot down minimal notes, not verbose explanations. Instead of asking an LLM to "think step by step" (which produces long, token-heavy reasoning chains), CoD instructs the model to limit each reasoning step to ~5 words.

**Results on Claude 3.5 Sonnet:**

| Task | CoT accuracy | CoD accuracy | Token savings |
|------|-------------|-------------|---------------|
| GSM8K (math) | 95.8% | 91.4% | **79%** |
| Date understanding | 87.0% | 89.7% | **82%** |
| Sports understanding | 93.2% | 97.3% | **92%** |
| Coin flip (logic) | 100% | 100% | **86%** |

## Live demo

**[→ Try the CoD Prompt Studio](https://YOUR_USERNAME.github.io/chain-of-draft/)**

An interactive tool to generate CoD prompts and compare CoT vs CoD in real time. No install required — runs entirely in your browser.

- **Prompt generator** (free): select a reasoning domain, enter your problem, get a copy-pasteable CoD prompt with optimized few-shot examples.
- **Live comparison** (optional): add your Anthropic API key to run both CoT and CoD side by side and see the token savings in real time. Your key stays in your browser — never sent anywhere except `api.anthropic.com`.

## What's in this repo

```
chain-of-draft/
├── SKILL.md                     # Main skill file — when & how to apply CoD
└── references/
    ├── arithmetic.md            # Few-shot examples for math problems
    ├── dates.md                 # Few-shot examples for date/time reasoning
    ├── symbolic.md              # Few-shot examples for logic & state tracking
    ├── commonsense.md           # Few-shot examples for common sense tasks
    ├── trading.md               # Few-shot examples for trading calculations
    └── template.md              # Guide to create CoD examples for new domains
docs/
└── index.html                   # CoD Prompt Studio (GitHub Pages demo)
```

## Quick start

### As a Claude Code skill

Copy the `chain-of-draft/` folder into your project's skill directory:

```bash
cp -r chain-of-draft/ /path/to/your/project/.claude/skills/
```

Claude will automatically detect and use the skill when it encounters multi-step reasoning tasks where token efficiency matters.

### As a standalone prompt template

The core CoD prompt is simple:

```
System: Think step by step, but only keep a minimum draft for each thinking
step, with 5 words at most. Return the answer at the end of the response
after a separator ####.

Q: Jason had 20 lollipops. He gave Denny some lollipops. Now Jason has 12 lollipops.
How many lollipops did Jason give to Denny?
A: 20 - 12 = 8. #### 8

Q: There are 15 trees in the grove. Workers will plant trees today. After they
are done, there will be 21 trees. How many trees did workers plant?
A: 21 - 15 = 6. #### 6

Q: [YOUR QUESTION HERE]
```

**Important:** Few-shot examples are critical. Without them, CoD performance drops significantly (only +3.6% over direct answer on Claude vs +30% with examples).

### Via the API

```python
import anthropic

client = anthropic.Anthropic()

few_shot = [
    {"role": "user", "content": "Q: Jason had 20 lollipops. He gave Denny some. Now he has 12. How many did he give?"},
    {"role": "assistant", "content": "A: 20 - 12 = 8. #### 8"},
    {"role": "user", "content": "Q: Shawn has 5 toys. He got 2 each from mom and dad. How many now?"},
    {"role": "assistant", "content": "A: 2+2=4 new; 5+4=9. #### 9"},
]

response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=256,
    system="Think step by step, but only keep a minimum draft for each thinking step, with 5 words at most. Return the answer at the end of the response after a separator ####.",
    messages=[
        *few_shot,
        {"role": "user", "content": "Q: A farmer has 17 sheep. All but 9 die. How many are left?"}
    ]
)
print(response.content[0].text)
# Expected: "All but 9 = 9. #### 9"
```

## Supported domains

| Domain | Reference file | Example use cases |
|--------|---------------|-------------------|
| **Arithmetic** | `references/arithmetic.md` | Word problems, multi-step calculations, percentages |
| **Date / time** | `references/dates.md` | Date arithmetic, leap years, day-of-week |
| **Symbolic / logic** | `references/symbolic.md` | Coin flips, state tracking, boolean logic |
| **Commonsense** | `references/commonsense.md` | Sports rules, physical reasoning |
| **Trading** | `references/trading.md` | Position sizing, SQN, expectancy, P&L |
| **Custom** | `references/template.md` | Guide to create your own domain |

## Creating few-shot examples for new domains

See [`references/template.md`](chain-of-draft/references/template.md) for a step-by-step guide. The short version:

1. Pick 4–6 representative problems with verifiable answers
2. Solve each with full Chain of Thought
3. Compress each step to its essential calculation (≤5 words per step)
4. Verify the compressed draft still yields the correct answer
5. Test on unseen problems before using in production

## When NOT to use CoD

- **Zero-shot**: CoD requires few-shot examples to work well
- **Creative/open-ended tasks**: CoD is for convergent reasoning, not divergent thinking
- **Small models** (<3B parameters): significant accuracy degradation
- **When every % of accuracy matters**: CoD trades ~4% accuracy for ~80% fewer tokens on hard math

## References

- Xu, S., Xie, W., Zhao, L., & He, P. (2025). *Chain of Draft: Thinking Faster by Writing Less*. [arXiv:2502.18600](https://arxiv.org/abs/2502.18600) — [PDF](https://arxiv.org/pdf/2502.18600)
- Wei, J., et al. (2022). *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models*. NeurIPS 2022.
- Original code: [github.com/sileix/chain-of-draft](https://github.com/sileix/chain-of-draft)

## License

MIT — see [LICENSE](LICENSE).

## Contributing

Contributions welcome, especially new domain reference files. Open a PR with your few-shot examples following the template in `references/template.md`.
