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

NExt [[Cycle 5 - What about a non ISO-8601 date ?]]