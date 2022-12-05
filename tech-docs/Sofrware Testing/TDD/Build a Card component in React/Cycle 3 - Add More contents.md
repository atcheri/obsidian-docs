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

Next [[Cycle 4 - Date in ISO-8601]]