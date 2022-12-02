### Introduction
The exercice here consists of building a card component like in the image below
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

### How to start

Ad we're doping outside-in TDD, we could start by writing the smallest test script that will "render" the card with a `title` , a `date` a `content` and a `Read More` button.
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

#### Cycle 1

❌  At this stage the component doesn't exist so the test fails 

```shell
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

#### Cycle 2

We now decide to add a new test that asserts the presence of the "title" in the rendered component

```typescript
// Card.test.tsx
...
  describe("Given a title prop", () => {
	it("displays the title in the header", () => {
      render(<Card title="Title" />);
      const title = screen.getByRole("heading", { level: 2 });
      expect(title).toHaveTextContent("Title");
    });
  });
```

❌  The title props doesn't exist in the Card component and the title is not displayed

The minimum change would be to add a `<h2>` html element and write the word "Title" in it

```diff
// Card.tsx
-const Card: FC = () => {
-  return <div>Card</div>;
+type CardProps = {
+  title?: string;
+};
+
+const Card: FC<CardProps> = () => {
+  return (
+    <div>
+      <h2>Title</h2>
+    </div>
+  );
 };
 
 export default Card;

```

✅  And the test should pass now

```shell
 PASS  src/Card.test.tsx
  <Card />
    Given no props
      ✓ displays an empty card (17 ms)
    Given a title prop
      ✓ displays the title in the header (56 ms)

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        2.039 s
```

⚪   Now let's use the props "title" in the component

```diff
// Card.tsx
-const Card: FC<CardProps> = () => {
+const Card: FC<CardProps> = ({ title }) => {
   return (
     <div>
-      <h2>Title</h2>
+      <h2>{title}</h2>
     </div>
   );
 };
```

Should still be green after this step.

#### Cycle 3

We understood how to add and check the presence of a field in the component.
Let's allow us a shortcut here and do the same for the following fields all together
- formatted date as a subtitle
- main content text
- the text in the button
The challenge here will be to display a formatted date. For that, we're going to add a new dependency, namely [date-fns](https://date-fns.org/) that is very useful to work with dates in javascript/typescript.

We add new test that asserts 

```typescript
// Card.test.tsx
...
  describe("Given a title, date, content and button text", () => {
    it("displays all given fields", () => {
      render(
        <Card
          title="Title"
          date="some date"
          content="some content"
          buttonText="some button content"
        />
      );
      screen.getByText("some date");
      screen.getByText("some content");
      screen.getByText("some button content");
    });
  });

```

❌  The new date, content buttonText props don't exist in the Card component

```shell
 FAIL  src/Card.test.tsx
  <Card />
    Given no props
      ✓ displays an empty card (13 ms)
    Given a title prop
      ✓ displays the title in the header (38 ms)
    Given a title, date, content and button text
      ✕ displays all given fields (6 ms)
```

Let's hardcode each test in some respective html elements

```diff
 // Card.tsx
 
 type CardProps = {
   title?: string;
+  date?: string;
+  content?: string;
+  buttonText?: string;
 };
 
 const Card: FC<CardProps> = ({ title }) => {
   return (
     <div>
-      <h2>{title}</h2>
+      <div>
+        <h2>{title}</h2>
+        <div>some date</div>
+      </div>
+      <div>
+        <div>some content</div>
+      </div>
+      <div>
+        <button>some button content</button>
+      </div>
     </div>
```

>Q: Why did we put all the props optional with the question mark❔

> A: Just thought that we could also give the possibility to display a card without any content. An empty card

✅  What about the test output now?

```shell
 PASS  src/Card.test.tsx
  <Card />
    Given no props
      ✓ displays an empty card (13 ms)
    Given a title prop
      ✓ displays the title in the header (35 ms)
    Given a title, date, content and button text
      ✓ displays all given fields (4 ms)

Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total
Snapshots:   0 total
Time:        0.809 s, estimated 1 s
```

⚪   Now let's refactor the same way as before

```diff
// Card.tsx

-const Card: FC<CardProps> = ({ title }) => {
+const Card: FC<CardProps> = ({ title, date, content, buttonText }) => {
   return (
     <div>
       <div>
         <h2>{title}</h2>
-        <div>some date</div>
+        <div>{date}</div>
       </div>
       <div>
-        <div>some content</div>
+        <div>{content}</div>
       </div>
       <div>
-        <button>some button content</button>
+        <button>{buttonText}</button>
       </div>
     </div>
   );
```

And it still passes. Good

But are we satisfied here? Surely not !

#### Cycle 4

Let's see what we can do with the date part. As of now, <Card /> is  just receiving a `date` as pure string. Which could be the case if we were getting some data from an enternal `api`, which usually  comes in [ISO-8601](https://apiux.com/2013/03/20/5-laws-api-dates-and-times/) format (ex: `2013-03-01T23:59:59`)
So how can we convert this to display a date like in the picture ⬆️
Let's make use of the [`date-fns`](https://date-fns.org/) module that we spoke about earlier.

```shell
npm i date-fns
```

And then let's modify the `date`  prop value given to <Card />

```diff
// Card.tsx

       render(
         <Card
           title="Title"
-          date="some date"
+          date="2016-04-01T23:59:59"
           content="some content"
           buttonText="some button content"
         />
       );
-      screen.getByText("some date");
+      screen.getByText("April 01, 2016");
       screen.getByText("some content");
       screen.getByText("some button content");
     });
```

❌  The test fails as expected. The date is not show correctly.

```shell
FAIL  src/Card.test.tsx
  <Card />
    ...
    Given a title, date, content and button text
      ✕ displays all given fields (6 ms)
```

Let's fix that

```diff
// Card.tsx

+import { format, parseISO } from "date-fns";
 import { FC } from "react";
 
 type CardProps = {
@@ -12,7 +13,7 @@ const Card: FC<CardProps> = ({ title, date, content, buttonText }) => {
     <div>
       <div>
         <h2>{title}</h2>
-        <div>{date}</div>
+        <div>{date && format(parseISO(date), "MMMM dd, yyyy")}</div>
       </div>
       <div>
         <div>{content}</div>
```

✅  It passes now

⚪   Let's refactor the date formatting logic

```diff
// Card.tsx

 const Card: FC<CardProps> = ({ title, date, content, buttonText }) => {
+  const formattedDate = date && format(parseISO(date), "MMMM dd, yyyy");
   return (
     <div>
       <div>
         <h2>{title}</h2>
-        <div>{date && format(parseISO(date), "MMMM dd, yyyy")}</div>
+        <div>{formattedDate}</div>
       </div>
       <div>
         <div>{content}</div>
```

Sure, still green.

But hold on, what if the passed `date` is not in **ISO-8601** format? Say we give it `some-date` as a value. This is gowing to throw an `exception`!

What do we need to do? 

You guessed it. Let's add a test for that.

#### Cycle 5

Let's add a new test for the <Card /> and give it a `date`  that is not in ISO-8601 format

```typescript
// Card.test.tsx

  describe("Given all content props with in invalid date", () => {
    it("doesn't show any date", () => {
      render(
        <Card
          title="Title"
          date="invalid iso-8601 date"
          content="some content"
          buttonText="some button content"
        />
      );
      expect(
        screen.queryByRole("heading", { level: 3 })
      ).not.toBeInTheDocument();
    });
  });
```

❌  Kaboum 💥  ! Even worse, the app can crash !

This needs to be fixed asap.

Fortunately `date-fns` ships with a utility function that can help us here and with this minimum change:

```diff
// Card.tsx

-import { format, parseISO } from "date-fns";
+import { format, isValid, parseISO } from "date-fns";
 import { FC } from "react";
 
 ...
 
     <div>
       <div>
         <h2>{title}</h2>
-        <div>{date && format(parseISO(date), "MMMM dd, yyyy")}</div>
+        <div>
+          {date && isValid(parseISO(date)) && (
+            <div role="heading" aria-level={3}>
+              {format(parseISO(date), "MMMM dd, yyyy")}
+            </div>
+          )}
+        </div>
       </div>
       <div>
         <div>{content}</div>
```

✅ The tests now are back to green.

⚪   All the logic in the jsx part? Can't we do a bit better? Time for a refactoring. Let's move the validation, parsing and formatting logic out of the jsx part.

```diff
// Card.tsx

-const Card: FC<CardProps> = ({ title, date, content, buttonText }) => {
+const Card: FC<CardProps> = ({ title, date = "", content, buttonText }) => {
+  const parsedISODate = parseISO(date);
+  const isValidDate = isValid(parsedISODate);
+  const formattedDate = isValidDate && format(parsedISODate, "MMMM dd, yyyy");
   return (
     <div>
       <div>
         <h2>{title}</h2>
-        <div>
-          {date && isValid(parseISO(date)) && (
-            <div role="heading" aria-level={3}>
-              {format(parseISO(date), "MMMM dd, yyyy")}
-            </div>
-          )}
-        </div>
+        {isValidDate && (
+          <div role="heading" aria-level={3}>
+            {formattedDate}
+          </div>
+        )}
```

⚪   Why not even moving all this date part into its' own component ? Let's create a new one with it's own tests






Refactoring would be to have only one prop