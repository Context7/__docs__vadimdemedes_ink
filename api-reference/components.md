# Components

Import path for every symbol on this page: `ink`

## `Box`

```ts
const Box = forwardRef<DOMElement, PropsWithChildren<Props>>((props, ref) => { … })
```

Definition: `src/components/Box.tsx:61`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `props` | `PropsWithChildren<BoxProps>` | style defaults are applied internally | Flexbox container props plus optional accessibility props. |
| `ref` | `Ref<DOMElement>` | — | Ref to the underlying Ink host node. |

Return type: forwarded React element or `null`. When screen-reader mode is enabled and `'aria-hidden'` is `true`, `Box` returns `null` instead of rendering (`src/components/Box.tsx:74-78`).

Throws:

| Error type | Condition |
|---|---|
| `Error` | A `<Box>` rendered inside `<Text>` fails reconciliation with `<Box> can’t be nested inside <Text> component` (`src/reconciler.ts:205-206`). |

Minimal example:

```tsx
import React, {useRef, useEffect} from 'react';
import {Box, Text, render, measureElement} from 'ink';
import type {DOMElement} from 'ink';

const App = () => {
	const ref = useRef<DOMElement>(null);

	useEffect(() => {
		if (ref.current) {
			const {width, height} = measureElement(ref.current);
			process.stdout.write(`box=${width}x${height}\n`);
		}
	}, []);

	return (
		<Box ref={ref} borderStyle="round" paddingX={1}>
			<Text>Hello</Text>
		</Box>
	);
};

render(<App />);
```

Source: `src/components/Box.tsx:61`

Public props type: [`BoxProps`](../types.md#boxprops)

## `Text`

```ts
export default function Text(props: Props) { … }
```

Definition: `src/components/Text.tsx:71`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `props` | `TextProps` | `dimColor=false`, `bold=false`, `italic=false`, `underline=false`, `strikethrough=false`, `inverse=false`, `wrap='wrap'`, `'aria-hidden'=false` | Styled text props plus optional accessibility labels. |

Return type: Ink text element or `null`. `Text` returns `null` when `children` is `undefined` or `null`, and also returns `null` in screen-reader mode when `'aria-hidden'` is `true` (`src/components/Text.tsx:90-92`, `133-135`).

Throws:

| Error type | Condition |
|---|---|
| `Error` | Raw string children outside `<Text>` are invalid, but normal `<Text>` usage is the required containment that prevents that reconciler error (`src/reconciler.ts:252-256`). |

Minimal example:

```tsx
import React from 'react';
import {Text, render} from 'ink';

render(
	<Text color="green" bold wrap="truncate-end">
		Build finished successfully
	</Text>,
);
```

Source: `src/components/Text.tsx:71`

Public props type: [`TextProps`](../types.md#textprops)

## `Static`

```ts
export default function Static<T>(props: Props<T>) { … }
```

Definition: `src/components/Static.tsx:28`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `props` | `StaticProps<T>` | — | Append-only item list, optional container style, and an item renderer. |

Return type: Ink box element containing the items not yet emitted as static output.

Behavioral notes from source:

- `Static` stores an internal `index` and renders only `items.slice(index)` on each commit (`src/components/Static.tsx:30-34`).
- After each layout commit, it advances `index` to `items.length`, so previous items stay above the live tree and are not re-rendered (`src/components/Static.tsx:36-38`).
- Its internal style always sets `position: 'absolute'` and `flexDirection: 'column'` before merging `style` (`src/components/Static.tsx:44-50`).

Minimal example:

```tsx
import React, {useEffect, useState} from 'react';
import {Box, Static, Text, render} from 'ink';

const App = () => {
	const [lines, setLines] = useState<string[]>([]);

	useEffect(() => {
		const timer = setInterval(() => {
			setLines(previous => [...previous, `step ${previous.length + 1}`]);
		}, 100);

		return () => clearInterval(timer);
	}, []);

	return (
		<>
			<Static items={lines}>
				{line => (
					<Box key={line}>
						<Text>{line}</Text>
					</Box>
				)}
			</Static>
			<Text>completed: {lines.length}</Text>
		</>
	);
};

render(<App />);
```

Source: `src/components/Static.tsx:28`

Public props type: [`StaticProps<T>`](../types.md#staticpropst)

## `Transform`

```ts
export default function Transform({
	children,
	transform,
	accessibilityLabel,
}: Props) { … }
```

Definition: `src/components/Transform.tsx:21`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `props` | `TransformProps` | — | String transform callback, optional accessibility label, and children to render first. |

Return type: Ink text element or `null`. It returns `null` when `children` is `undefined` or `null` (`src/components/Transform.tsx:28-30`).

Minimal example:

```tsx
import React from 'react';
import {Transform, Text, render} from 'ink';

render(
	<Transform transform={(output, index) => `${index}: ${output.toUpperCase()}`}>
		<Text>status</Text>
	</Transform>,
);
```

Source: `src/components/Transform.tsx:21`

Public props type: [`TransformProps`](../types.md#transformprops)

## `Newline`

```ts
export default function Newline({count = 1}: Props) { … }
```

Definition: `src/components/Newline.tsx:15`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `props` | `NewlineProps` | `{count: 1}` | Number of newline characters to emit. |

Return type: Ink text element containing `'\n'.repeat(count)`.

Throws:

| Error type | Condition |
|---|---|
| `Error` | Like any text node, the rendered newlines must be inside `<Text>`; using `<Newline>` outside text content is invalid by reconciler rules (`src/reconciler.ts:252-256`). |

Minimal example:

```tsx
import React from 'react';
import {Text, Newline, render} from 'ink';

render(
	<Text>
		line 1
		<Newline count={2} />
		line 3
	</Text>,
);
```

Source: `src/components/Newline.tsx:15`

Public props type: [`NewlineProps`](../types.md#newlineprops)

## `Spacer`

```ts
export default function Spacer() { … }
```

Definition: `src/components/Spacer.tsx:9`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| — | — | — | `Spacer` takes no props. |

Return type: `<Box flexGrow={1} />`.

Minimal example:

```tsx
import React from 'react';
import {Box, Spacer, Text, render} from 'ink';

render(
	<Box width={30}>
		<Text>left</Text>
		<Spacer />
		<Text>right</Text>
	</Box>,
);
```

Source: `src/components/Spacer.tsx:9`
