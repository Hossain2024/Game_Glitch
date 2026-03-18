# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the secret number kept changing" or "the hints were backwards").

- The hints are not right . when i enetered a number less than the target/correct number the hint says to go lower
- The number of attempts displayed on the hint is wrong 1 lower than actual attempts
- there is a glitch when trying to start over after one round. it doesn't start over 

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
  - I used claude Code
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).

to solve the bug "the hints are not right . when i enetered a number less than the target/correct number the hint says to go lower"
AI suggested: 
  On odd attempts (1st, 3rd, etc.), the secret number is treated as an integer, and comparisons work correctly (numerical).
On even attempts (2nd, 4th, etc.), the secret number is converted to a string (secret = str(st.session_state.secret)), but the guess remains an integer.
When comparing an integer to a string in check_guess(), it triggers a TypeError, falling back to string comparison in the except block.
String comparison is lexicographical (alphabetical order), not numerical. For example:
If the secret is 10 (as string "10") and you guess 5 (as string "5"), "5" > "10" lexicographically (because '5' > '1'), so it incorrectly says "Too High" and suggests "Go HIGHER!" when it should say "Too Low" and "Go LOWER!".
This causes the hints to be reversed on even attempts, leading to wrong guidance.
The issue stems from inconsistent data types in the comparison logic, mixing numerical and string operations. To fix it, the code would need to ensure consistent types (e.g