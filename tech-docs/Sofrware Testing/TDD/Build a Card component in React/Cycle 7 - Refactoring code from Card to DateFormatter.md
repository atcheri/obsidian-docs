We now want to give it a non, ISO-8601 date as a prop. The test can help

```typescript
// DateFormatter.test.tsx

...
  describe("Given a non ISO-8601 date", () => {
    it("returns an empty component", () => {
      render(<DateFormatter date="non-iso-date" />);
      const date = screen.getByRole("heading", { level: 3 });
      expect(date).toBeInTheDocument();
    });
  });
```

❌  The component is no doing anything but returning null. We can now start adding some code

To make this test pass we need this piece of code

```diff
// DateFormatter.tsx

-type DateFormatterProps = {};
+type DateFormatterProps = { date: string };
...
-  return null;
+  return (
+    <div role="heading" aria-level={3}>
+      DateFormatter
+    </div>
+  );

```

⚪ Still nothing to refactor? Ok let's continue

Now we can add a new test with a ISO-8601 compliant date

```typescript
// DateFormatter.test.tsx
...
  describe("Given a ISO-8601 compliant date", () => {
    it("displays a formatted date", () => {
      render(<DateFormatter date="2016-04-01T23:59:59" />);
      screen.getByText("April 01, 2016");
    });
  });
```

❌  The component is not displaying anything but it's original text in a div

✅  Fixed with this naive implementation

```diff
// DateFormatter.tsx

     <div role="heading" aria-level={3}>
-      DateFormatter
+      April 01, 2016
     </div>
```

⚪ We could now refactor at this stage by importing the date related logic from Card.tsx into DateFormatter.tsx

```diff
// DateFormatter.tsx

+import { format, isValid, parseISO } from "date-fns";
 import { FC } from "react";
 
-type DateFormatterProps = {};
+type DateFormatterProps = { date: string };
 
-const DateFormatter: FC<DateFormatterProps> = () => {
+const DateFormatter: FC<DateFormatterProps> = ({ date }) => {
+  const parsedISODate = parseISO(date);
+  const isValidDate = isValid(parsedISODate);
+  const formattedDate = isValidDate && format(parsedISODate, "MMMM dd, yyyy");
   return (
     <div role="heading" aria-level={3}>
-      DateFormatter
+      {formattedDate}
     </div>
   );
 };
```

and 

```diff
// Card.tsx

-import { format, isValid, parseISO } from "date-fns";
 import { FC } from "react";
+import DateFormatter from "./DateFormatter";
 
 type CardProps = {
   title?: string;
 };
 
 const Card: FC<CardProps> = ({ title, date = "", content, buttonText }) => {
-  const parsedISODate = parseISO(date);
-  const isValidDate = isValid(parsedISODate);
-  const formattedDate = isValidDate && format(parsedISODate, "MMMM dd, yyyy");
   return (
     <div>
       <div>
         <h2>{title}</h2>
-        {isValidDate && (
-          <div role="heading" aria-level={3}>
-            {formattedDate}
-          </div>
-        )}
+        <DateFormatter date={date} />
       </div>
       <div>
         <div>{content}</div>
```

✅  And we're still green! Refactoring was successful

⏱️ Let's see at this stage how our component looks like. Let's just import it in our `App.tsx` file

```diff
// App.tsx

+import Card from "./Card";
 
 function App() {
   return (
     <div className="App">
+      <Card
+        title="Title"
+        date="2022-11-27T23:59:59"
+        content="Lorem ipsum content"
+        buttonText="Read More"
+      />
     </div>
   );
 }
```

Let's have a look at how our state of the art minimalistic card component looks like

![[raw-card.png]]

But before styling , we could add the button click functionality and move onto that right after.

Next [[Cycle 8 - The button click handler]]