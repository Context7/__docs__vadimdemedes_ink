# Hooks: Focus, Layout, Animation, Cursor

Import path for every symbol on this page: `ink`

## `useFocus`

```ts
const useFocus = ({
	isActive = true,
	autoFocus = false,
	id: customId,
}: {
	isActive?: boolean;
	autoFocus?: boolean;
	id?: string;
} = {}): {
	isFocused: boolean;
	focus: (id: string) => void;
} => { … }
```

Definition: `src/hooks/use-focus.ts:38`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `options` | `{isActive?: boolean; autoFocus?: boolean; id?: string}` | `{isActive: true, autoFocus: false}` | Focus registration options. |

Return type: `{isFocused: boolean; focus: (id: string) => void}`.

Behavioral details from source:

- Registers the component in `FocusContext` on mount and unregisters on unmount (`src/hooks/use-focus.ts:51-57`).
- Generates a random ID when `id` is omitted (`src/hooks/use-focus.ts:47-49`).
- Calls `activate(id)` or `deactivate(id)` when `isActive` changes (`src/hooks/use-focus.ts:59-65`).
- Enables raw mode while active if stdin supports it, so Tab/Shift+Tab focus cycling can work (`src/hooks/use-focus.ts:67-77`).

Throws:

| Error type | Condition |
|---|---|
| `Error` | Activating focus on an unsupported stdin causes the same raw-mode error path as `useInput()` (`src/components/App.tsx:315-327`). |

Minimal example:

```tsx
import React from 'react';
import {Box, Text, render, useFocus} from 'ink';

const Item = ({label}: {label: string}) => {
	const {isFocused} = useFocus();
	return <Text>{label}{isFocused ? ' *' : ''}</Text>;
};

render(
	<Box flexDirection="column">
		<Item label="first" />
		<Item label="second" />
	</Box>,
);
```

Source: `src/hooks/use-focus.ts:38`

## `useFocusManager`

```ts
const useFocusManager = (): {
	enableFocus: () => void;
	disableFocus: () => void;
	focusNext: () => void;
	focusPrevious: () => void;
	focus: (id: string) => void;
	activeId?: string;
} => { … }
```

Definition: `src/hooks/use-focus-manager.ts:50`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| — | — | — | `useFocusManager()` takes no arguments. |

Return type: object with global focus-management methods and `activeId`.

Minimal example:

```tsx
import React from 'react';
import {Text, render, useFocusManager, useInput} from 'ink';

const App = () => {
	const {focusNext, activeId} = useFocusManager();

	useInput(input => {
		if (input === 'n') {
			focusNext();
		}
	});

	return <Text>focused: {activeId ?? 'none'}</Text>;
};

render(<App />);
```

Source: `src/hooks/use-focus-manager.ts:50`

## `useIsScreenReaderEnabled`

```ts
const useIsScreenReaderEnabled = (): boolean => { … }
```

Definition: `src/hooks/use-is-screen-reader-enabled.ts:8`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| — | — | — | `useIsScreenReaderEnabled()` takes no arguments. |

Return type: `boolean`, taken from `accessibilityContext`.

Minimal example:

```tsx
import React from 'react';
import {Text, render, useIsScreenReaderEnabled} from 'ink';

const App = () => {
	const enabled = useIsScreenReaderEnabled();
	return <Text>{enabled ? 'screen reader mode' : 'visual mode'}</Text>;
};

render(<App />);
```

Source: `src/hooks/use-is-screen-reader-enabled.ts:8`

## `useCursor`

```ts
const useCursor = () => {
	return {setCursorPosition};
}
```

Definition: `src/hooks/use-cursor.ts:12`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| — | — | — | `useCursor()` takes no arguments. |

Return type: `{setCursorPosition: (position: CursorPosition | undefined) => void}`.

Behavioral details from source:

- Stores the requested cursor position in a ref (`src/hooks/use-cursor.ts:13-21`).
- Propagates the ref value into `CursorContext` during `useInsertionEffect`, which means the cursor update is tied to the committed render, not abandoned concurrent renders (`src/hooks/use-cursor.ts:23-32`).
- Passing `undefined` hides the cursor (`src/components/CursorContext.ts:5-10`).

Minimal example:

```tsx
import React, {useState} from 'react';
import stringWidth from 'string-width';
import {Box, Text, render, useCursor, useInput} from 'ink';

const App = () => {
	const [text, setText] = useState('');
	const {setCursorPosition} = useCursor();
	const prompt = '> ';

	useInput(input => {
		if (input) {
			setText(previous => previous + input);
		}
	});

	setCursorPosition({x: stringWidth(prompt + text), y: 1});

	return (
		<Box flexDirection="column">
			<Text>Type:</Text>
			<Text>{prompt}{text}</Text>
		</Box>
	);
};

render(<App />);
```

Source: `src/hooks/use-cursor.ts:12`

Related type: [`CursorPosition`](../types.md#cursorposition)

## `useAnimation`

```ts
export default function useAnimation(
	options?: {interval?: number; isActive?: boolean},
): AnimationResult
```

Definition: `src/hooks/use-animation.ts:67`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `options` | `{interval?: number; isActive?: boolean}` | `{interval: 100, isActive: true}` | Animation scheduling options. |

Return type: `AnimationResult`.

Behavioral details from source:

- Uses a shared scheduler from `AnimationContext`, so multiple animations consolidate onto one timer (`src/hooks/use-animation.ts:67-143`).
- `interval` is normalized: non-finite values fall back to `100`, and values are clamped to `[1, 2147483647]` (`src/hooks/use-animation.ts:145-150`).
- When `isActive` toggles from `false` to `true`, or when `interval` or the internal `resetKey` changes, the returned state resets to `{frame: 0, time: 0, delta: 0}` before new ticks arrive (`src/hooks/use-animation.ts:75-82`, `138-142`).

Minimal example:

```tsx
import React from 'react';
import {Text, render, useAnimation} from 'ink';

const frames = ['-', '\\\\', '|', '/'];

const App = () => {
	const {frame} = useAnimation({interval: 80});
	return <Text>{frames[frame % frames.length]}</Text>;
};

render(<App />);
```

Source: `src/hooks/use-animation.ts:67`

Related type: [`AnimationResult`](../types.md#animationresult)

## `useWindowSize`

```ts
const useWindowSize = (): WindowSize => { … }
```

Definition: `src/hooks/use-window-size.ts:23`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| — | — | — | `useWindowSize()` takes no arguments. |

Return type: `WindowSize`.

Behavioral details from source:

- Initializes from `getWindowSize(stdout)` (`src/hooks/use-window-size.ts:24-25`).
- Subscribes to the active stdout stream’s `'resize'` event and updates local state on each resize (`src/hooks/use-window-size.ts:27-37`).

Minimal example:

```tsx
import React from 'react';
import {Text, render, useWindowSize} from 'ink';

const App = () => {
	const {columns, rows} = useWindowSize();
	return <Text>{columns}x{rows}</Text>;
};

render(<App />);
```

Source: `src/hooks/use-window-size.ts:23`

Related type: [`WindowSize`](../types.md#windowsize)

## `useBoxMetrics`

```ts
const useBoxMetrics = (
	ref: RefObject<DOMElement | null>,
): UseBoxMetricsResult => { … }
```

Definition: `src/hooks/use-box-metrics.ts:86`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `ref` | `RefObject<DOMElement \| null>` | — | Ref attached to a `<Box>` so Ink can read its computed Yoga layout. |

Return type: `UseBoxMetricsResult`.

Behavioral details from source:

- Returns zeros until a layout pass has measured the current element (`src/hooks/use-box-metrics.ts:64-67`, `92-93`).
- Updates metrics after every render, on root layout-listener callbacks, and on stdout resize events (`src/hooks/use-box-metrics.ts:112-138`).
- `hasMeasured` becomes `false` again when the tracked ref is detached (`src/hooks/use-box-metrics.ts:109`).

Minimal example:

```tsx
import React, {useRef} from 'react';
import {Box, Text, render, useBoxMetrics} from 'ink';
import type {DOMElement} from 'ink';

const App = () => {
	const ref = useRef<DOMElement>(null);
	const {width, height, hasMeasured} = useBoxMetrics(ref);

	return (
		<Box ref={ref} borderStyle="single">
			<Text>{hasMeasured ? `${width}x${height}` : 'measuring'}</Text>
		</Box>
	);
};

render(<App />);
```

Source: `src/hooks/use-box-metrics.ts:86`

Related types:

- [`BoxMetrics`](../types.md#boxmetrics)
- [`UseBoxMetricsResult`](../types.md#useboxmetricsresult)
- [`DOMElement`](../types.md#domelement)
