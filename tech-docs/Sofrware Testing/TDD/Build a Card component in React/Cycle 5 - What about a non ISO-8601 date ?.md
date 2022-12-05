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

⚪ Why not even moving all this *date* part into its' own component ? Let's create a new one with it's own tests

Nexy [[Cycle 6 - The DateFormatter component]]