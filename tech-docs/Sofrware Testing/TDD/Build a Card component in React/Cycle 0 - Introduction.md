The exercice here consists of building a card component like in the image below.
And for that we're going to do Test-Driven-Development. And more specificaly [outside-in TDD](https://outsidein.dev/concepts/outside-in-tdd/)

![[react-card-component.png]]

Prerequisities:
- Having nodejs installed
- Having a ready to use react app with a test runner (jest or mocha for instance)

### Conventions used

❌  for the Red phase
✅  for the Green phase
⚪  for the Refactoring phase

### Features

It's mostly about displaying some data inside of a "Card"
The only "feature" in this card is that the "Read More" button in the card's footer should execute whatever action is given as a prop to the Card component (Dependency-Injection)

Let's see how to we can. Next [[Cycle 1 - How to start]]