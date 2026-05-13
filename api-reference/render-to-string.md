# `renderToString`

Import path: `ink`

## `renderToString`

```ts
const renderToString = (
	node: ReactNode,
	options?: RenderToStringOptions,
): string => { … }
```

Synchronously renders a React tree to a string without attaching to `stdout`, `stdin`, resize listeners, or terminal raw mode. Definition: `src/render-to-string.ts:44`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `node` | `ReactNode` | — | Root React tree to render. |
| `options` | `RenderToStringOptions` | `{columns: 80}` | Virtual terminal settings. |

Return type: `string`. The returned string contains the final rendered output. If `<Static>` emitted content during rendering, Ink prepends that static output to the dynamic output with a newline separator when both are present.

Additional source-backed behavior:

- Ink creates an isolated `ink-root` host node and sets its Yoga width from `options?.columns ?? 80` before layout calculation (`src/render-to-string.ts:48-68`).
- `rootNode.onImmediateRender` captures transient `<Static>` output before the reconciler clears it on the next render pass (`src/render-to-string.ts:53-75`).
- Yoga memory is freed explicitly on both success and failure paths because the Yoga nodes are WASM-backed and not garbage collected (`src/render-to-string.ts:120-155`).
- This API never installs stdout writers, input listeners, resize handlers, or raw-mode management; it only returns a synchronous string.

Throws:

| Error type | Condition |
|---|---|
| `Error` | A component throws during rendering; Ink captures the first uncaught error, finishes reconciler cleanup, then re-throws it (`src/render-to-string.ts:77-129`). |
| `Error` | A non-`Error` thrown value is coerced to `new Error(String(value))` before rethrow (`src/render-to-string.ts:124-128`). |
| `Error` | Invalid host-tree structure such as raw strings outside `<Text>` or `<Box>` inside `<Text>` still trips reconciler validation (`src/reconciler.ts:205-256`). |

Minimal example:

```tsx
import React from 'react';
import {renderToString, Box, Text} from 'ink';

const output = renderToString(
	<Box padding={1}>
		<Text color="cyan">report</Text>
	</Box>,
	{columns: 40},
);

process.stdout.write(output + '\n');
```

Source: `src/render-to-string.ts:44`

## `RenderToStringOptions`

```ts
export type RenderToStringOptions = {
	columns?: number;
};
```

Definition: `src/render-to-string.ts:8`.

| Field | Type | Default | Description |
|---|---|---|---|
| `columns` | `number` | `80` | Width of the virtual terminal used for Yoga layout. |

Accepted by:

- `renderToString(node, options)`

Related references:

- [render](./render.md)
- [configuration](../configuration.md#rendertostringoptions)
- [types](../types.md#rendertostringoptions)
