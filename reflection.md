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
The issue stems from inconsistent data types in the comparison logic, mixing numerical and string operations. To fix it, the code would need to ensure consistent types (e.g., always compare as integers), but since you asked not to change the code, this explains the root cause.

After fixing the code based on suggested suggestion I ran the code again and played the game again to produce the bug and the bug was fixed 
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

- there is a glitch when trying to start over after one round. it doesn't start over 
AI suggested:
  New Game button, you don’t reset it back to "playing".So when Streamlit reruns, this block triggers.
  didnt clear history . So even if the game restarted, it would feel “stuck” because old data persists. 
  based on the suggestion I reset the histroy, score and status of the game
---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
  - I asked for the logic behind the bug . and aked to fix the code . then i restarted the game and investigated if the bug continued.
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
  - after fixing the bug "not restarting the game" I attempted to play again with the code fixed and didnt see the bug. 
- Did AI help you design or understand any tests? How?
yes. 

---

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
- What change did you make that finally gave the game a stable secret number?

n the original app, the secret number kept changing because Streamlit reruns the entire script every time the user interacts with the app (like clicking a button or typing input).

Since the secret number was being generated using random.randint(low, high)

without storing it anywhere persistent, a new random number was created on every rerun. That meant the “target” number wasn’t stable the game was unintentionally resetting itself every time the user made a guess.

I changed the code by storing the secret number inside st.session_state and only initializing it once. this ensured persistency across code

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
learned:

- always give context in promts
- try to understand the bug and sugetion before accepting

one thing i learned is that AI code gives us good draft and helps us to direct our thinking but we should not blindly follow as it can be wrong at times
