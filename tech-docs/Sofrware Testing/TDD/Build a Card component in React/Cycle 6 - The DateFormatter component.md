The DateFormatter component. Here is the first test 

```typescript
// DateFormatter.test.tsx

import { render } from "@testing-library/react";

import DateFormatter from "./DateFormatter";

describe("<DateFormatter />", () => {
  describe("Given no props", () => {
    it("renders the component", () => {
      render(<DateFormatter />);
    });
  });
});
```

❌  As of now the Component doesn't exist.

And the minimum code needed for the corresponding component

```typescript
// DateFormatter.tsximport { FC } from "react";

type DateFormatterProps = {};

const DateFormatter: FC<DateFormatterProps> = () => {
  return null;
};

export default DateFormatter;

```

✅ Ok it passes.

⚪ Nothing at this stage to refactor, let's continue