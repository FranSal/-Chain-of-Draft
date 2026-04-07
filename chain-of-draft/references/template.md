# Creating CoD Few-Shot Examples for New Domains — Template

Follow this process to create effective Chain of Draft examples for any domain.

## Step 1: Select representative problems

Pick 4-6 problems from the target domain that:
- Require 2-5 reasoning steps (not too simple, not too complex)
- Have a single, verifiable correct answer
- Cover the range of typical difficulty in the domain
- Include the most common reasoning patterns users will encounter

## Step 2: Solve with full CoT first

For each problem, write out the complete Chain of Thought reasoning. This gives you
the full set of intermediate steps to compress.

**Example — Chemistry:**
Q: What is the molar mass of H₂SO₄?
CoT: Hydrogen (H) has atomic mass 1.008. There are 2 hydrogens, so 2 × 1.008 = 2.016.
Sulfur (S) has atomic mass 32.07. There is 1 sulfur, so 32.07.
Oxygen (O) has atomic mass 16.00. There are 4 oxygens, so 4 × 16.00 = 64.00.
Total: 2.016 + 32.07 + 64.00 = 98.086 g/mol.
#### 98.09 g/mol

## Step 3: Compress to CoD

For each CoT step, extract ONLY the calculation or state change. Remove:
- Context words ("Hydrogen has atomic mass...")
- Explanatory phrases ("There are 2 hydrogens, so...")
- Transitional language ("Therefore...", "This means...")
- Redundant information

**Compressed CoD:**
A: 2×1.008+32.07+4×16=98.09. #### 98.09 g/mol

## Step 4: Verify correctness

Re-read each CoD draft and confirm:
- [ ] Can you derive the correct answer from the draft alone?
- [ ] Is each step ≤5 words? (guideline, not hard limit)
- [ ] Are there no unnecessary words?
- [ ] Would a human "get it" from the shorthand?

## Step 5: Test with unseen problems

Before using in production, test the few-shot set on 5-10 problems from the domain
that were NOT used to create the examples. Check:
- Does the model consistently produce concise drafts?
- Is accuracy comparable to full CoT?
- Are output tokens significantly reduced?

If accuracy drops, add 1-2 more diverse few-shot examples.

## Template for new reference files

```markdown
# [Domain Name] — CoD Few-Shot Examples

Use these examples for [brief description of when to use].

## Few-shot set (pick 3-4)

### Example 1: [short description]
Q: [problem text]
A: [minimal CoD draft]. #### [answer]

### Example 2: [short description]
Q: [problem text]
A: [minimal CoD draft]. #### [answer]

### Example 3: [short description]
Q: [problem text]
A: [minimal CoD draft]. #### [answer]

## Usage notes

- [Domain-specific tips for writing good CoD drafts]
- [Common notation or abbreviations to use]
- [Pitfalls to avoid]
```

## Notation conventions across domains

| Pattern | Use for | Example |
|---------|---------|---------|
| `→` | State transitions | `H→flip:T` |
| `;` | Step separator | `step1; step2; step3` |
| `=` | Calculation result | `5×3=15` |
| `∧ ∨ ¬` | Boolean logic | `T∧F=F` |
| Abbreviations | Common terms | `qty`, `avg`, `max`, `vol` |
| Units inline | Keep context | `375$`, `0.2lots`, `16%` |
