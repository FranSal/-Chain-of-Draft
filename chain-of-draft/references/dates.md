# Date / Time Reasoning — CoD Few-Shot Examples

Use these examples for date arithmetic, day-of-week calculations, relative dates, and temporal reasoning.

## Few-shot set (pick 3-4)

### Example 1: Birthday + relative date
Q: Jane was born on the last day of February in 2001. Today is her 16-year-old birthday. What is the date yesterday in MM/DD/YYYY?
A: Born 02/28/2001; +16y=02/28/2017; yesterday=02/27/2017. #### 02/27/2017

### Example 2: Delay + past date
Q: The concert was scheduled to be on 06/01/1943, but was delayed by one day to today. What is the date 10 days ago in MM/DD/YYYY?
A: 06/01+1d=06/02/1943; -10d=05/23/1943. #### 05/23/1943

### Example 3: Future date calculation
Q: Today is 03/15/2024. What date will it be 45 days from now in MM/DD/YYYY?
A: Mar has 16d left; Apr=30d; 16+30=46>45; 45-16=29; 04/29/2024. #### 04/29/2024

### Example 4: Day of week
Q: Today is Wednesday, March 5, 2025. What day of the week was it 10 days ago?
A: 10d back; 10%7=3d; Wed-3=Sunday. #### Sunday

### Example 5: Leap year
Q: Was February 29, 2000 a valid date?
A: 2000÷400=5; divisible→leap year; valid. #### Yes

## Usage notes

- Use compact date notation: MM/DD, +Nd, -Ny
- Arrow notation works well for date chains: date→+offset=result
- For month-boundary calculations, show the day counting briefly
- Keep intermediate dates in the same format as the expected output
