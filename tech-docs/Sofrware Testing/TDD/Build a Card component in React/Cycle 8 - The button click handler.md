In this cycle, we'll implement the only "feature" that this card shipw with. Executing whatever action is given to the button to the `onClick`  prop

Let's then fist add a test that makes sure that the action or the `function` is execute when the button is clicked
*Note: need to install `@testing-library/user-event`*

```diff
// Card.test.tsx

import { render, screen } from "@testing-library/react";
+import userEvent from "@testing-library/user-event";
 
 import Card from "./Card";
 
+      const user = userEvent.setup();
+      const spy = jest.fn(() => console.log("click la mais comment"));
+      render(
+        <Card
+          title="Title"
+          date="invalid iso-8601 date"
+          content="some content"
+          buttonText="some button content"
+          handleClick={spy}
+        />
+      );
+      const button = screen.getByRole("button");
+      await user.click(button);
+      expect(spy).toHaveBeenCalledTimes(1);
     });
   });
 });

```

❌  As of now, the callback function is not given and used anywhere

And the minimum and sufficient code change in the Card component is as bellow

```diff
// Card.tsx

-import { FC } from "react";
+import { FC, MouseEvent } from "react";
 import DateFormatter from "./DateFormatter";
 
 type CardProps = {
   date?: string;
   content?: string;
   buttonText?: string;
+  handleClick?: (event: MouseEvent<HTMLButtonElement>) => void;
 };
 
-const Card: FC<CardProps> = ({ title, date = "", content, buttonText }) => {
+const Card: FC<CardProps> = ({
+  title,
+  date = "",
+  content,
+  buttonText,
+  handleClick = () => {},
+}) => {
   return (
     <div>
       <div>
         <div>{content}</div>
       </div>
       <div>
-        <button>{buttonText}</button>
+        <button onClick={handleClick}>{buttonText}</button>
       </div>
     </div>

```

✅  And that's enough to get back to Green. Well done.

⏱️ So far so good, I don't see anything particularly to refactor from there.

Is that's all ?

You guessed it, let's go to the next [[Cycle 9 - Styling the components with Tailwind]]