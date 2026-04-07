# Symbolic / Logical Reasoning — CoD Few-Shot Examples

Use these for coin flips, state tracking, boolean logic, sequences, and rule-based reasoning.

## Few-shot set: Coin flip (pick 2-3)

### Example 1: Two flips
Q: A coin is heads up. Ka flips the coin. Sherrie flips the coin. Is the coin still heads up?
A: H→Ka:T→Sherrie:H. #### Yes

### Example 2: Flip + no-flip
Q: A coin is heads up. Jamey flips the coin. Teressa flips the coin. Noel does not flip the coin. Is the coin still heads up?
A: H→Jamey:T→Teressa:H→Noel:H. #### Yes

### Example 3: Three flips
Q: A coin is heads up. Maybelle flips the coin. Shalonda does not flip the coin. Jen flips the coin. Is the coin still heads up?
A: H→Maybelle:T→Shalonda:T→Jen:H. #### Yes

## Few-shot set: Boolean / logic (pick 2-3)

### Example 4: Compound condition
Q: If it's raining AND the temperature is below 5°C, the game is cancelled. It's raining and 3°C. Is the game cancelled?
A: Rain=T; 3<5→T; T∧T=T. #### Yes

### Example 5: Negation chain
Q: Statement A is true. If A then B. If B then not C. If not C then D. Is D true?
A: A→B:T; B→¬C:T; ¬C→D:T. #### Yes

### Example 6: Sequence pattern
Q: What is the next number in the sequence: 2, 6, 18, 54, ...?
A: ×3 pattern; 54×3=162. #### 162

## Usage notes

- Arrow notation (→) is ideal for state transitions
- Use single-letter abbreviations for states: H/T, T/F
- Chain operations with colons or semicolons
- For boolean logic, use standard symbols: ∧ (and), ∨ (or), ¬ (not)
- Each state change = one step, no narrative needed
