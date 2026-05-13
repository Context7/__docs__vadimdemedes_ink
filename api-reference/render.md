# `render`

Import path: `ink`

## `render`

```ts
const render = (
	node: ReactNode,
	options?: NodeJS.WriteStream | RenderOptions,
): Instance => { … }
```

Mounts a React tree into an Ink terminal session. Definition: `src/render.ts:199`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `node` | `ReactNode` | — | Root React tree to render. |
| `options` | `NodeJS.WriteStream \| RenderOptions` | `process.stdout` when a stream is passed; otherwise `RenderOptions` defaults are applied | Either a shorthand stdout stream or the full render configuration object described in [configuration.md](../configuration.md) and [types.md](../types.md#renderoptions). |

Return type: `Instance`. The returned object exposes `rerender`, `unmount`, `waitUntilExit`, `waitUntilRenderFlush`, `cleanup`, and `clear`. See [types.md](../types.md#instance).

Throws or rejects:

| Error type | Condition |
|---|---|
| `Error` | Rendering a raw string outside `<Text>` triggers `Text string "…" must be rendered inside <Text> component` (`src/reconciler.ts:252`). |
| `Error` | Nesting `<Box>` inside `<Text>` triggers `<Box> can’t be nested inside <Text> component` (`src/reconciler.ts:205`). |
| `Promise` rejection from `waitUntilExit()` | `exit(error)` is called from `useApp()` with an `Error` or a thrown render error reaches Ink’s exit path. |

Minimal example:

```tsx
import React from 'react';
import {render, Text} from 'ink';

const App = () => <Text color="green">ready</Text>;

const app = render(<App />, {maxFps: 60});

await app.waitUntilRenderFlush();
app.unmount();
await app.waitUntilExit();
```

Source: `src/render.ts:199`

## `RenderOptions`

```ts
export type RenderOptions = {
	stdout?: NodeJS.WriteStream;
	stdin?: NodeJS.ReadStream;
	stderr?: NodeJS.WriteStream;
	debug?: boolean;
	exitOnCtrlC?: boolean;
	patchConsole?: boolean;
	onRender?: (metrics: RenderMetrics) => void;
	isScreenReaderEnabled?: boolean;
	maxFps?: number;
	incrementalRendering?: boolean;
	concurrent?: boolean;
	kittyKeyboard?: KittyKeyboardOptions;
	interactive?: boolean;
	alternateScreen?: boolean;
};
```

Definition: `src/render.ts:8`.

`RenderOptions` is the full public configuration type accepted by `render()`. The complete option table, defaults, and the `INK_SCREEN_READER` environment variable are documented in [configuration.md](../configuration.md). The type is also listed in [types.md](../types.md#renderoptions).

Accepted by:

- `render(node, options)`

## `Instance`

```ts
export type Instance = {
	rerender: Ink['render'];
	unmount: Ink['unmount'];
	waitUntilExit: Ink['waitUntilExit'];
	waitUntilRenderFlush: Ink['waitUntilRenderFlush'];
	cleanup: () => void;
	clear: () => void;
};
```

Definition: `src/render.ts:138`.

`Instance` is the control API returned by `render()`.

| Member | Type | Description |
|---|---|---|
| `rerender` | `Ink['render']` | Replaces the current root node or updates its props. |
| `unmount` | `Ink['unmount']` | Unmounts the current app. |
| `waitUntilExit` | `Ink['waitUntilExit']` | Resolves with `exit(value)`, rejects with `exit(error)`, or resolves after manual unmount completes. |
| `waitUntilRenderFlush` | `Ink['waitUntilRenderFlush']` | Resolves after pending render output has flushed to stdout. |
| `cleanup` | `() => void` | Public alias that unmounts and lets Ink release the current stdout-bound instance. |
| `clear` | `() => void` | Clears Ink-managed output from the terminal. |

Minimal example:

```tsx
import React from 'react';
import {render, Text} from 'ink';

const app = render(<Text>step 1</Text>);

app.rerender(<Text>step 2</Text>);
await app.waitUntilRenderFlush();

app.clear();
app.unmount();
await app.waitUntilExit();
```

Source: `src/render.ts:138`
