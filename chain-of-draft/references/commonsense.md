# Commonsense Reasoning — CoD Few-Shot Examples

Use these for sports understanding, physical reasoning, social conventions, and general knowledge inference.

## Few-shot set: Sports understanding (pick 2-3)

### Example 1: Football
Q: Is the following sentence plausible? "Jamal Murray was called for a technical foul."
A: Murray=NBA player; tech fouls exist in NBA. #### Yes

### Example 2: Impossible action
Q: Is the following sentence plausible? "Saquon Barkley scored a bicycle kick goal."
A: Barkley=NFL; bicycle kicks=soccer only. #### No

### Example 3: Correct sport
Q: Is the following sentence plausible? "Jonas Valanciunas grabbed the rebound."
A: Valanciunas=NBA center; rebounds=basketball. #### Yes

## Few-shot set: Physical / world knowledge (pick 2-3)

### Example 4: Physical reasoning
Q: If I put a heavy ball on a table made of glass, what is likely to happen?
A: Heavy ball + glass = likely breaks. #### The table will likely break

### Example 5: Causal chain
Q: A candle is lit in a sealed room. After several hours, what happens to the flame?
A: Sealed→O₂ depletes→flame dies. #### The flame goes out

### Example 6: Common knowledge
Q: Can a penguin fly to another continent?
A: Penguins=flightless birds; cannot fly. #### No

## Usage notes

- For sports plausibility: pattern is [person=sport]; [action=valid_in_sport?]
- For physical reasoning: chain cause→effect with minimal notation
- Keep world knowledge assertions to the minimum needed for the conclusion
- Avoid explaining why something is common knowledge — just state the relevant fact
