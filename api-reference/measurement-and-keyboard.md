# Measurement And Keyboard Protocol

Import path for every symbol on this page: `ink`

## `measureElement`

```ts
const measureElement = (node: DOMElement): {width: number; height: number} => ({ … })
```

Definition: `src/measure-element.ts:22`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `node` | `DOMElement` | — | Ink host node, typically obtained from a `<Box>` ref. |

Return type: `{width: number; height: number}` representing computed Yoga layout dimensions.

Behavioral details from source:

- Reads `node.yogaNode?.getComputedWidth()` and `getComputedHeight()` (`src/measure-element.ts:22-25`).
- Returns `{width: 0, height: 0}` when called before layout has been computed or when the node has no Yoga node (`src/measure-element.ts:20-25`).

Minimal example:

```tsx
import React, {useEffect, useRef} from 'react';
import {Box, Text, render, measureElement} from 'ink';
import type {DOMElement} from 'ink';

const App = () => {
	const ref = useRef<DOMElement>(null);

	useEffect(() => {
		if (ref.current) {
			const size = measureElement(ref.current);
			process.stdout.write(JSON.stringify(size) + '\n');
		}
	}, []);

	return (
		<Box ref={ref} paddingX={2}>
			<Text>measure me</Text>
		</Box>
	);
};

render(<App />);
```

Source: `src/measure-element.ts:22`

Related type: [`DOMElement`](../types.md#domelement)

## `kittyFlags`

```ts
export const kittyFlags = {
	disambiguateEscapeCodes: 1,
	reportEventTypes: 2,
	reportAlternateKeys: 4,
	reportAllKeysAsEscapeCodes: 8,
	reportAssociatedText: 16,
} as const;
```

Definition: `src/kitty-keyboard.ts:3`.

`kittyFlags` is the public bit-value map for kitty keyboard protocol flag names. It is primarily used with `RenderOptions.kittyKeyboard.flags` through the `KittyFlagName` union.

Minimal example:

```ts
import {kittyFlags} from 'ink';

const mask =
	kittyFlags.disambiguateEscapeCodes |
	kittyFlags.reportEventTypes;
```

Source: `src/kitty-keyboard.ts:3`

## `kittyModifiers`

```ts
export const kittyModifiers = {
	shift: 1,
	alt: 2,
	ctrl: 4,
	super: 8,
	hyper: 16,
	meta: 32,
	capsLock: 64,
	numLock: 128,
} as const;
```

Definition: `src/kitty-keyboard.ts:28`.

`kittyModifiers` is the public bit-value map for modifier bits in CSI `u` kitty keyboard sequences.

Minimal example:

```ts
import {kittyModifiers} from 'ink';

const hasCtrlAndAlt =
	Boolean(kittyModifiers.ctrl) && Boolean(kittyModifiers.alt);
```

Source: `src/kitty-keyboard.ts:28`

## `KittyKeyboardOptions`

```ts
export type KittyKeyboardOptions = {
	mode?: 'auto' | 'enabled' | 'disabled';
	flags?: KittyFlagName[];
};
```

Definition: `src/kitty-keyboard.ts:40`.

| Field | Type | Default | Description |
|---|---|---|---|
| `mode` | `'auto' \| 'enabled' \| 'disabled'` | implementation default is automatic detection | Controls whether Ink detects, forces, or disables kitty keyboard protocol support. |
| `flags` | `KittyFlagName[]` | library defaults are internal; no public constant is exported for them | Requested kitty protocol flag names. |

Accepted by:

- `RenderOptions.kittyKeyboard`

Source: `src/kitty-keyboard.ts:40`

Related references:

- [types](../types.md#kittykeyboardoptions)
- [configuration](../configuration.md#renderoptions)

## `KittyFlagName`

```ts
export type KittyFlagName = keyof typeof kittyFlags;
```

Definition: `src/kitty-keyboard.ts:12`.

Return and usage notes:

- Valid values are the exact keys of `kittyFlags`: `disambiguateEscapeCodes`, `reportEventTypes`, `reportAlternateKeys`, `reportAllKeysAsEscapeCodes`, and `reportAssociatedText`.
- `KittyFlagName` is accepted by `KittyKeyboardOptions.flags`.

Minimal example:

```ts
import type {KittyFlagName} from 'ink';

const flags: KittyFlagName[] = [
	'disambiguateEscapeCodes',
	'reportEventTypes',
];
```

Source: `src/kitty-keyboard.ts:12`
