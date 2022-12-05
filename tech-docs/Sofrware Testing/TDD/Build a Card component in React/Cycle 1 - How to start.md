As we're doing outside-in TDD, we could start by writing the smallest test script that will "render" the card with a `title` , a `date` a `content` and a `Read More` button.

```typescript
// Card.test.tsx

import { render } from "@testing-library/react";

describe("<Card />", () => {
	describe("Given no props", () => {
		it("displays an empty card", () => {
			render(<Card />);
		});
	});
});
```

❌  At this stage the component doesn't exist so the test fails 

```shell
// Card.test.tsx

    ReferenceError: Card is not defined

      4 |   describe("Given a title & a date & a content & a button with content", () => {
      5 |     it("displays all information in their respective block and format", () => {
    > 6 |       render(<Card />);
        |               ^
      7 |     });
      8 |   });
      9 | });
```

✅  We just create the Card component 

```typescript
// Card.tsx

import { FC } from "react";

const Card: FC = () => {
	return <div>Card</div>;
};

export default Card;
```

and import it in the test file

```diff
// Card.test.ts

 import { render } from "@testing-library/react";
 
+import Card from "./Card";
+
 describe("<Card />", () => {
   describe("Given no props", () => {
     it("displays an empty card", () => {

```

The test should now pass

```shell
PASS  src/Card.test.tsx
  <Card />
    Given no props
      ✓ displays an empty card (13 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        2.093 s
```

⚪  I personaly don't see anything to refactor here, let's go to the next cycle

Next [[Cycle 2 - The "title" prop]]
