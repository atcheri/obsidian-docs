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

Next [[Cycle 3 - Add More contents]]