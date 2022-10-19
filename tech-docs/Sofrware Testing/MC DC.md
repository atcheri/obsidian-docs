>Test important combinations of conditions instead of all of them, and limit testing costs.

How? By extending branch and decision coverage with the requirement that "each condition should affect the decision outcome independentely".

### Example 1: a and b and c

| Test case | a | b | c | outcome |
| --------- | - | - | - | ------- |
| 1 |true | true | true | true |
| 2 | true | true | false | false |
| 3 | true | false | true | false |
| 4 | true | false | true | false |
| 5 | false | true | true | false |
| 6 | false | true | false | false |
| 7 | false | false | true | false |
| 8 | false | false | false | false |

First, we look at 2 test cases with different outcome where __only__ a is different.
We find Test cases 1 and 5.
Then we do the same looking at b. and find 1 and 3.
Finally, we iterate the process for c and find 1 and 2.
The relevant test cases are, __1 2 3 and 5__.

### Example 2: a or (b and c)

| Test case | a | b | c | outcome |
| --------- | - | - | - | ------- |
| 1 |true | true | true | true |
| 2 | true | true | false | true |
| 3 | true | false | true | true |
| 4 | true | false | false | true |
| 5 | false | true | true | true |
| 6 | false | true | false | false |
| 7 | false | false | true | false |
| 8 | false | false | false | false |

First, we look at 2 test cases with different outcome where __only__ a is different.
We find Test cases 4 and 8.
Then we do the same looking at b. and find 5 and 7.
Finally, we iterate the process for c and find 5 and 6.
The relevant test cases are, __4 5 6 and 8__.



Modified Condition & Decision Coverage
Structural coverage: how much of your design has been testes?
Extends decision and conditions coverage
How does a given input __independently__ affect output of logical expression?

Why is MC/DC used?
- Required for software developed at highest criticality level.
- Shows how each condition tested when it affects the output.
- Can improve effectiveness and efficiency of testing.

What to do when the requirement tests cases don't achieve full MC/DC
Question to ask: why it doesn't cover
1. Are there errors in the design?
	- Is there a dead logic
	- How do these errors affect the output of the expression
2.  Can we augment existing resuirements-based test cases?
	 - Extend existing test cases in time to achieve additional coverage
	- Make sure expected results are still valid, or updated accordingly
3. Are there derived requirements?
	-  Are there parts of your design with no highter-level requirements?
	- Document and write test cases for derived requirepents

