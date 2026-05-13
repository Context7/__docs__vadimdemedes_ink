# Hooks: Lifecycle And Streams

Import path for every symbol on this page: `ink`

## `useInput`

```ts
const useInput = (
	inputHandler: (input: string, key: Key) => void,
	options: {isActive?: boolean} = {},
) => { … }
```

Definition: `src/hooks/use-input.ts:159`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `inputHandler` | `(input: string, key: Key) => void` | — | Called for each parsed input event. |
| `options` | `{isActive?: boolean}` | `{}` | Enables or disables the subscription; `false` skips raw mode and event listeners. |

Return type: `void`.

Behavioral details from source:

- Enables raw mode while active by calling Ink’s `setRawMode(true)` in an effect (`src/hooks/use-input.ts:164-174`).
- Parses terminal data through `parseKeypress()` and normalizes printable characters, control keys, kitty keyboard modifiers, and event types into `Key` (`src/hooks/use-input.ts:176-255`).
- Suppresses listener invocation for Ctrl+C when `internal_exitOnCtrlC` is enabled (`src/hooks/use-input.ts:244-247`).
- Uses `reconciler.discreteUpdates()` so state updates from input are scheduled with discrete event priority (`src/hooks/use-input.ts:249-255`).

Throws:

| Error type | Condition |
|---|---|
| `Error` | When the hook activates raw mode on an unsupported stdin. Ink throws one of two messages depending on whether the stream is `process.stdin` or a custom stdin (`src/components/App.tsx:315-327`). |

Minimal example:

```tsx
import React, {useState} from 'react';
import {Box, Text, render, useApp, useInput} from 'ink';

const App = () => {
	const {exit} = useApp();
	const [value, setValue] = useState('');

	useInput((input, key) => {
		if (key.return) {
			exit();
			return;
		}

		if (!key.ctrl && !key.meta && input) {
			setValue(previous => previous + input);
		}
	});

	return (
		<Box flexDirection="column">
			<Text>Type and press Enter to exit.</Text>
			<Text>{value}</Text>
		</Box>
	);
};

render(<App />);
```

Source: `src/hooks/use-input.ts:159`

Related type: [`Key`](../types.md#key)

## `usePaste`

```ts
const usePaste = (
	handler: (text: string) => void,
	options: {isActive?: boolean} = {},
): void => { … }
```

Definition: `src/hooks/use-paste.ts:39`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `handler` | `(text: string) => void` | — | Receives the entire pasted text payload. |
| `options` | `{isActive?: boolean}` | `{}` | Enables or disables the paste listener. |

Return type: `void`.

Behavioral details from source:

- Activates both raw mode and bracketed-paste mode while active (`src/hooks/use-paste.ts:47-59`).
- Subscribes to the internal `'paste'` channel instead of the `'input'` channel, so paste content is not forwarded to `useInput()` when paste listeners exist (`src/components/App.tsx:288-296`, `src/hooks/use-paste.ts:70-80`).
- Also uses `reconciler.discreteUpdates()` for handler execution (`src/hooks/use-paste.ts:61-68`).

Throws:

| Error type | Condition |
|---|---|
| `Error` | Same raw-mode requirement as `useInput()` when the hook becomes active (`src/components/App.tsx:315-327`). |

Minimal example:

```tsx
import React, {useState} from 'react';
import {Text, render, usePaste} from 'ink';

const App = () => {
	const [text, setText] = useState('');

	usePaste(value => {
		setText(value);
	});

	return <Text>{text || 'Paste something'}</Text>;
};

render(<App />);
```

Source: `src/hooks/use-paste.ts:39`

## `useApp`

```ts
const useApp = () => useContext(AppContext)
```

Definition: `src/hooks/use-app.ts:7`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| — | — | — | `useApp()` takes no arguments. |

Return type: `AppProps`.

| Member | Type | Description |
|---|---|---|
| `exit` | `(errorOrResult?: Error \| unknown) => void` | Unmounts the app and settles `waitUntilExit()` with either a resolved value or a rejection. |
| `waitUntilRenderFlush` | `() => Promise<void>` | Resolves after pending render output has flushed. |

Minimal example:

```tsx
import React, {useEffect} from 'react';
import {Text, render, useApp} from 'ink';

const App = () => {
	const {exit, waitUntilRenderFlush} = useApp();

	useEffect(() => {
		void (async () => {
			await waitUntilRenderFlush();
			exit(0);
		})();
	}, [exit, waitUntilRenderFlush]);

	return <Text>done</Text>;
};

const app = render(<App />);
await app.waitUntilExit();
```

Source: `src/hooks/use-app.ts:7`

Related type: [`AppProps`](../types.md#appprops)

## `useStdin`

```ts
const useStdin = (): PublicProps => useContext(StdinContext)
```

Definition: `src/hooks/use-stdin.ts:10`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| — | — | — | `useStdin()` takes no arguments. |

Return type: `StdinProps`.

| Member | Type | Description |
|---|---|---|
| `stdin` | `NodeJS.ReadStream` | Active input stream for the current Ink app. |
| `setRawMode` | `(value: boolean) => void` | Ink-managed raw-mode toggle. |
| `isRawModeSupported` | `boolean` | Whether the active stdin supports `setRawMode`. |

Minimal example:

```tsx
import React, {useEffect} from 'react';
import {Text, render, useStdin} from 'ink';

const App = () => {
	const {isRawModeSupported, setRawMode} = useStdin();

	useEffect(() => {
		if (isRawModeSupported) {
			setRawMode(true);
			return () => setRawMode(false);
		}
	}, [isRawModeSupported, setRawMode]);

	return <Text>stdin ready</Text>;
};

render(<App />);
```

Source: `src/hooks/use-stdin.ts:10`

Related type: [`StdinProps`](../types.md#stdinprops)

## `useStdout`

```ts
const useStdout = () => useContext(StdoutContext)
```

Definition: `src/hooks/use-stdout.ts:7`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| — | — | — | `useStdout()` takes no arguments. |

Return type: `StdoutProps`.

| Member | Type | Description |
|---|---|---|
| `stdout` | `NodeJS.WriteStream` | Active render output stream. |
| `write` | `(data: string) => void` | Writes a string while preserving Ink’s terminal output bookkeeping. |

Minimal example:

```tsx
import React, {useEffect} from 'react';
import {Text, render, useStdout} from 'ink';

const App = () => {
	const {write} = useStdout();

	useEffect(() => {
		write('external stdout line\n');
	}, [write]);

	return <Text>main view</Text>;
};

render(<App />);
```

Source: `src/hooks/use-stdout.ts:7`

Related type: [`StdoutProps`](../types.md#stdoutprops)

## `useStderr`

```ts
const useStderr = () => useContext(StderrContext)
```

Definition: `src/hooks/use-stderr.ts:7`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| — | — | — | `useStderr()` takes no arguments. |

Return type: `StderrProps`.

| Member | Type | Description |
|---|---|---|
| `stderr` | `NodeJS.WriteStream` | Active error stream. |
| `write` | `(data: string) => void` | Writes a string while preserving Ink’s terminal output bookkeeping. |

Minimal example:

```tsx
import React, {useEffect} from 'react';
import {Text, render, useStderr} from 'ink';

const App = () => {
	const {write} = useStderr();

	useEffect(() => {
		write('warning: something happened\n');
	}, [write]);

	return <Text>running</Text>;
};

render(<App />);
```

Source: `src/hooks/use-stderr.ts:7`

Related type: [`StderrProps`](../types.md#stderrprops)
