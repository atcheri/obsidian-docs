
Table of content
episode 1
episode 2
episode 3



## The Golden Circle Theory of TDD

### The What

*__Test Driven Development__ is a software development process that follows these steps:*

1.  Write a failing test for one requirement.
2.  Implement just enough code to make the test pass.
3.  Refactor with confidence (if it’s needed).

### The How

**TDD & the Scientific Method**

1.  Translate a requirement into a falsifiable test.
2.  **Predict** the expected output based on the requirement.
3.  Run the test to get the actual output. It should _fail_ the first time. If it passes, go to step 1.
4.  Write the implementation. The test should _pass_ this time. If it fails, repeat this step.
5.  Move on to the next requirement and repeat from step 1.

### The Why

**TDD Benefits**

1. **Eliminates fear of change.** If a code change introduces a bug, developers are alerted to it quickly, and TDD’s tight feedback loop will quickly notify them when it’s fixed.
2. **A safety net** which makes continuous deployment safer. Test failures halt the deployment process, allowing you to fix bugs before customers ever have the chance to see them.
3. [**40% — 80% fewer bugs**](https://www.researchgate.net/publication/3249271_Guest_Editors'_Introduction_TDD--The_Art_of_Fearless_Programming)**.**
4. [**Better code coverage**](https://ieeexplore.ieee.org/abstract/document/4343755) than writing tests after the fact. Because we create code to make a specific test pass, code coverage will be close to 100%.
5. **Faster developer feedback loop.** Without TDD, developers must manually test each change to ensure that it works. With TDD, unit tests can run on-change automatically, providing faster feedback during development and debugging sessions.
6. **Interface design aid** (iiii) 
7. [**KISS and YAGNI**](obsidian://open?vault=tech-docs&file=Sofrware%20Testing%2FTDD%2FKiss%20%26%20Yagni) — “Keep it Simple, Stupid”, and “You Ain’t Gonna Need It”

**TDD Benegits for Developers**

....


#### Cons

**TDD Costs**

Users without TDD experience may find they move 15% — 30% slower, but with 1–2 year’s practice, TDD’s realtime feedback process can enhance productivity.



### Simple (fictional) examples

FE: *show case a front-end example with react and testing library* [link](obsidian://open?vault=tech-docs&file=Sofrware%20Testing%2FTDD%2FSimple%20react%20example)
(clicking on a button does something and shows something new)

BE: show case a back-end example with a nodejs (utility function, or api or cli) [link](obsidian://open?vault=tech-docs&file=Sofrware%20Testing%2FTDD%2FSimple%20nodejs%20example)
(example ?)

### More complexe examples




### [Affinidi stories examples](obsidian://open?vault=tech-docs&file=Sofrware%20Testing%2FTDD%2FQuestions%20and%20Answers)



### Interested, join us #slack-channel-id 
