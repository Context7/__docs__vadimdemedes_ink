# Configuration

## `RenderOptions`

Configuration type for `render(node, options)`. Source: `src/render.ts:8`.

| Key | Type | Required | Default | Description |
|---|---|---|---|---|
| `stdout` | `NodeJS.WriteStream` | no | `process.stdout` | Output stream where Ink renders the app. |
| `stdin` | `NodeJS.ReadStream` | no | `process.stdin` | Input stream used for hooks such as `useInput()` and `usePaste()`. |
| `stderr` | `NodeJS.WriteStream` | no | `process.stderr` | Error stream exposed by `useStderr()`. |
| `debug` | `boolean` | no | `false` | Disables frame replacement so each update is written as separate output. |
| `exitOnCtrlC` | `boolean` | no | `true` | Lets Ink handle Ctrl+C in raw mode by exiting the app. |
| `patchConsole` | `boolean` | no | `true` | Patches `console.*` so console writes do not corrupt Ink’s managed output. |
| `onRender` | `(metrics: RenderMetrics) => void` | no | — | Called after each committed render; does not wait for stream flush. |
| `isScreenReaderEnabled` | `boolean` | no | `process.env['INK_SCREEN_READER'] === 'true'` | Enables screen-reader-specific behavior. |
| `maxFps` | `number` | no | `30` | Maximum render frequency in frames per second. |
| `incrementalRendering` | `boolean` | no | `false` | Updates only changed lines instead of redrawing the full output. |
| `concurrent` | `boolean` | no | `false` | Enables React concurrent rendering mode. |
| `kittyKeyboard` | `KittyKeyboardOptions` | no | — | Configures kitty keyboard protocol support. |
| `interactive` | `boolean` | no | auto-detected from CI status and `stdout.isTTY` | Overrides Ink’s interactive/non-interactive mode detection. |
| `alternateScreen` | `boolean` | no | `false` | Renders into the terminal’s alternate screen buffer when interactive. |

Notes from source:

- Passing a `NodeJS.WriteStream` directly as the second argument to `render()` is shorthand for `{stdout, stdin: process.stdin}` (`src/render.ts:239-249`).
- In non-interactive mode, Ink disables erase sequences, cursor movement, synchronized output, resize handling, and kitty keyboard auto-detection (`src/render.ts:107-120` comments, `src/ink.tsx` constructor behavior).
- `alternateScreen` is ignored when Ink resolves to non-interactive mode (`src/render.ts:129-135` comments).

## `RenderToStringOptions`

Configuration type for `renderToString(node, options)`. Source: `src/render-to-string.ts:8`.

| Key | Type | Required | Default | Description |
|---|---|---|---|---|
| `columns` | `number` | no | `80` | Virtual terminal width used for Yoga layout during string rendering. |

## Environment variables

Only one public environment variable is referenced by the public API surface.

| Env var | Type | Default | Description |
|---|---|---|---|
| `INK_SCREEN_READER` | string (`'true'` recognized) | unset | When `isScreenReaderEnabled` is not passed, Ink enables screen-reader mode when this variable is exactly `'true'` (`src/ink.tsx:326-328`). |

No public config files or HTTP endpoint configuration were found in the repository. Internal development flags such as `process.env['DEV']` are used inside the implementation but are not part of the package’s exported API surface.
