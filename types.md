# Types

## `RenderOptions`

Import path: `ink`

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

Source: `src/render.ts:8`

| Field | Type | Required | Description |
|---|---|---|---|
| `stdout` | `NodeJS.WriteStream` | no | Output stream for Ink rendering. |
| `stdin` | `NodeJS.ReadStream` | no | Input stream for terminal hooks. |
| `stderr` | `NodeJS.WriteStream` | no | Error stream exposed by `useStderr()`. |
| `debug` | `boolean` | no | Writes each update as separate output instead of replacing prior frames. |
| `exitOnCtrlC` | `boolean` | no | Enables Ink’s Ctrl+C exit handling while in raw mode. |
| `patchConsole` | `boolean` | no | Routes `console.*` through Ink-safe output handling. |
| `onRender` | `(metrics: RenderMetrics) => void` | no | Commit callback invoked after each render. |
| `isScreenReaderEnabled` | `boolean` | no | Enables accessibility-specific rendering behavior. |
| `maxFps` | `number` | no | Maximum frame rate for render updates. |
| `incrementalRendering` | `boolean` | no | Uses changed-line rendering instead of full redraws. |
| `concurrent` | `boolean` | no | Uses React concurrent rendering. |
| `kittyKeyboard` | `KittyKeyboardOptions` | no | Kitty keyboard protocol configuration. |
| `interactive` | `boolean` | no | Overrides interactive-mode detection. |
| `alternateScreen` | `boolean` | no | Uses the terminal alternate screen buffer. |

Accepted by:

- [`render()`](./api-reference/render.md#render)

## `Instance`

Import path: `ink`

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

Source: `src/render.ts:138`

| Field | Type | Required | Description |
|---|---|---|---|
| `rerender` | `Ink['render']` | yes | Re-renders or replaces the root tree. |
| `unmount` | `Ink['unmount']` | yes | Unmounts the current app. |
| `waitUntilExit` | `Ink['waitUntilExit']` | yes | Promise-based exit completion API. |
| `waitUntilRenderFlush` | `Ink['waitUntilRenderFlush']` | yes | Resolves when pending output has flushed. |
| `cleanup` | `() => void` | yes | Cleanup helper exposed by `render()`. |
| `clear` | `() => void` | yes | Clears Ink-managed output. |

Returned by:

- [`render()`](./api-reference/render.md#render)

## `RenderToStringOptions`

Import path: `ink`

```ts
export type RenderToStringOptions = {
	columns?: number;
};
```

Source: `src/render-to-string.ts:8`

| Field | Type | Required | Description |
|---|---|---|---|
| `columns` | `number` | no | Width of the virtual terminal. |

Accepted by:

- [`renderToString()`](./api-reference/render-to-string.md#rendertostring)

## `BoxProps`

Import path: `ink`

```ts
export type Props = Except<Styles, 'textWrap'> & {
	readonly 'aria-label'?: string;
	readonly 'aria-hidden'?: boolean;
	readonly 'aria-role'?:
		| 'button'
		| 'checkbox'
		| 'combobox'
		| 'list'
		| 'listbox'
		| 'listitem'
		| 'menu'
		| 'menuitem'
		| 'option'
		| 'progressbar'
		| 'radio'
		| 'radiogroup'
		| 'tab'
		| 'tablist'
		| 'table'
		| 'textbox'
		| 'timer'
		| 'toolbar';
	readonly 'aria-state'?: {
		readonly busy?: boolean;
		readonly checked?: boolean;
		readonly disabled?: boolean;
		readonly expanded?: boolean;
		readonly multiline?: boolean;
		readonly multiselectable?: boolean;
		readonly readonly?: boolean;
		readonly required?: boolean;
		readonly selected?: boolean;
	};
};
```

Source: `src/components/Box.tsx:8`

Field summary:

| Field | Type | Required | Description |
|---|---|---|---|
| `…Styles except textWrap` | `Except<Styles, 'textWrap'>` | no | All Ink layout, spacing, border, sizing, positioning, and color props from `Styles`, except `textWrap`. |
| `'aria-label'` | `string` | no | Screen-reader label. |
| `'aria-hidden'` | `boolean` | no | Hides the box from screen readers. |
| `'aria-role'` | role union | no | Accessibility role metadata attached to the host node. |
| `'aria-state'` | object | no | Accessibility state flags attached to the host node. |

Accepted by:

- [`<Box>`](./api-reference/components.md#box)

## `TextProps`

Import path: `ink`

```ts
export type Props = {
	readonly 'aria-label'?: string;
	readonly 'aria-hidden'?: boolean;
	readonly color?: LiteralUnion<ForegroundColorName, string>;
	readonly backgroundColor?: LiteralUnion<ForegroundColorName, string>;
	readonly dimColor?: boolean;
	readonly bold?: boolean;
	readonly italic?: boolean;
	readonly underline?: boolean;
	readonly strikethrough?: boolean;
	readonly inverse?: boolean;
	readonly wrap?: Styles['textWrap'];
	readonly children?: ReactNode;
};
```

Source: `src/components/Text.tsx:9`

| Field | Type | Required | Description |
|---|---|---|---|
| `'aria-label'` | `string` | no | Screen-reader text override. |
| `'aria-hidden'` | `boolean` | no | Hides the text from screen readers. |
| `color` | `LiteralUnion<ForegroundColorName, string>` | no | Foreground color. |
| `backgroundColor` | `LiteralUnion<ForegroundColorName, string>` | no | Background color. |
| `dimColor` | `boolean` | no | Applies Chalk dim styling. |
| `bold` | `boolean` | no | Applies Chalk bold styling. |
| `italic` | `boolean` | no | Applies Chalk italic styling. |
| `underline` | `boolean` | no | Applies Chalk underline styling. |
| `strikethrough` | `boolean` | no | Applies Chalk strikethrough styling. |
| `inverse` | `boolean` | no | Swaps foreground and background. |
| `wrap` | `Styles['textWrap']` | no | Text wrapping or truncation mode. |
| `children` | `ReactNode` | no | Text content or nested `<Text>`. |

Accepted by:

- [`<Text>`](./api-reference/components.md#text)

## `AppProps`

Import path: `ink`

```ts
export type Props = {
	readonly exit: (errorOrResult?: Error | unknown) => void;
	readonly waitUntilRenderFlush: () => Promise<void>;
};
```

Source: `src/components/AppContext.ts:3`

| Field | Type | Required | Description |
|---|---|---|---|
| `exit` | `(errorOrResult?: Error \| unknown) => void` | yes | Unmounts and settles `waitUntilExit()`. |
| `waitUntilRenderFlush` | `() => Promise<void>` | yes | Resolves after pending output flushes. |

Returned by:

- [`useApp()`](./api-reference/hooks-lifecycle-and-streams.md#useapp)

## `StdinProps`

Import path: `ink`

```ts
export type PublicProps = {
	readonly stdin: NodeJS.ReadStream;
	readonly setRawMode: (value: boolean) => void;
	readonly isRawModeSupported: boolean;
};
```

Source: `src/components/StdinContext.ts:5`

| Field | Type | Required | Description |
|---|---|---|---|
| `stdin` | `NodeJS.ReadStream` | yes | Active input stream. |
| `setRawMode` | `(value: boolean) => void` | yes | Ink-managed raw-mode toggle. |
| `isRawModeSupported` | `boolean` | yes | Whether `setRawMode` can be used. |

Returned by:

- [`useStdin()`](./api-reference/hooks-lifecycle-and-streams.md#usestdin)

## `StdoutProps`

Import path: `ink`

```ts
export type Props = {
	readonly stdout: NodeJS.WriteStream;
	readonly write: (data: string) => void;
};
```

Source: `src/components/StdoutContext.ts:4`

| Field | Type | Required | Description |
|---|---|---|---|
| `stdout` | `NodeJS.WriteStream` | yes | Active stdout stream. |
| `write` | `(data: string) => void` | yes | Writes a string without corrupting Ink output. |

Returned by:

- [`useStdout()`](./api-reference/hooks-lifecycle-and-streams.md#usestdout)

## `StderrProps`

Import path: `ink`

```ts
export type Props = {
	readonly stderr: NodeJS.WriteStream;
	readonly write: (data: string) => void;
};
```

Source: `src/components/StderrContext.ts:4`

| Field | Type | Required | Description |
|---|---|---|---|
| `stderr` | `NodeJS.WriteStream` | yes | Active stderr stream. |
| `write` | `(data: string) => void` | yes | Writes a string without corrupting Ink output. |

Returned by:

- [`useStderr()`](./api-reference/hooks-lifecycle-and-streams.md#usestderr)

## `StaticProps<T>`

Import path: `ink`

```ts
export type Props<T> = {
	readonly items: T[];
	readonly style?: Styles;
	readonly children: (item: T, index: number) => ReactNode;
};
```

Source: `src/components/Static.tsx:4`

| Field | Type | Required | Description |
|---|---|---|---|
| `items` | `T[]` | yes | Items to emit into static output. |
| `style` | `Styles` | no | Container style merged with internal absolute/column layout. |
| `children` | `(item: T, index: number) => ReactNode` | yes | Item renderer. |

Accepted by:

- [`<Static>`](./api-reference/components.md#static)

## `TransformProps`

Import path: `ink`

```ts
export type Props = {
	readonly accessibilityLabel?: string;
	readonly transform: (children: string, index: number) => string;
	readonly children?: ReactNode;
};
```

Source: `src/components/Transform.tsx:4`

| Field | Type | Required | Description |
|---|---|---|---|
| `accessibilityLabel` | `string` | no | Alternate text used in screen-reader mode. |
| `transform` | `(children: string, index: number) => string` | yes | String transform applied to rendered child output. |
| `children` | `ReactNode` | no | Source content to render before transformation. |

Accepted by:

- [`<Transform>`](./api-reference/components.md#transform)

## `NewlineProps`

Import path: `ink`

```ts
export type Props = {
	readonly count?: number;
};
```

Source: `src/components/Newline.tsx:3`

| Field | Type | Required | Description |
|---|---|---|---|
| `count` | `number` | no | Number of `\n` characters to emit. |

Accepted by:

- [`<Newline>`](./api-reference/components.md#newline)

## `Key`

Import path: `ink`

```ts
export type Key = {
	upArrow: boolean;
	downArrow: boolean;
	leftArrow: boolean;
	rightArrow: boolean;
	pageDown: boolean;
	pageUp: boolean;
	home: boolean;
	end: boolean;
	return: boolean;
	escape: boolean;
	ctrl: boolean;
	shift: boolean;
	tab: boolean;
	backspace: boolean;
	delete: boolean;
	meta: boolean;
	super: boolean;
	hyper: boolean;
	capsLock: boolean;
	numLock: boolean;
	eventType?: 'press' | 'repeat' | 'release';
};
```

Source: `src/hooks/use-input.ts:9`

| Field | Type | Required | Description |
|---|---|---|---|
| `upArrow` through `numLock` | `boolean` | yes | Parsed modifier and special-key flags for the current key event. |
| `eventType` | `'press' \| 'repeat' \| 'release'` | no | Kitty protocol event type when available. |

Used by:

- [`useInput()`](./api-reference/hooks-lifecycle-and-streams.md#useinput) callback parameter `key`

## `AnimationResult`

Import path: `ink`

```ts
export type AnimationResult = {
	readonly frame: number;
	readonly time: number;
	readonly delta: number;
	readonly reset: () => void;
};
```

Source: `src/hooks/use-animation.ts:30`

| Field | Type | Required | Description |
|---|---|---|---|
| `frame` | `number` | yes | Integer frame counter. |
| `time` | `number` | yes | Elapsed time in milliseconds. |
| `delta` | `number` | yes | Milliseconds since the previous rendered tick. |
| `reset` | `() => void` | yes | Resets frame/time state to zero. |

Returned by:

- [`useAnimation()`](./api-reference/hooks-focus-layout-animation.md#useanimation)

## `WindowSize`

Import path: `ink`

```ts
export type WindowSize = {
	readonly columns: number;
	readonly rows: number;
};
```

Source: `src/hooks/use-window-size.ts:8`

| Field | Type | Required | Description |
|---|---|---|---|
| `columns` | `number` | yes | Terminal width in character cells. |
| `rows` | `number` | yes | Terminal height in rows. |

Returned by:

- [`useWindowSize()`](./api-reference/hooks-focus-layout-animation.md#usewindowsize)

## `BoxMetrics`

Import path: `ink`

```ts
export type BoxMetrics = {
	readonly width: number;
	readonly height: number;
	readonly left: number;
	readonly top: number;
};
```

Source: `src/hooks/use-box-metrics.ts:11`

| Field | Type | Required | Description |
|---|---|---|---|
| `width` | `number` | yes | Computed width. |
| `height` | `number` | yes | Computed height. |
| `left` | `number` | yes | Left offset relative to the parent. |
| `top` | `number` | yes | Top offset relative to the parent. |

Returned by:

- [`useBoxMetrics()`](./api-reference/hooks-focus-layout-animation.md#useboxmetrics)

## `UseBoxMetricsResult`

Import path: `ink`

```ts
export type UseBoxMetricsResult = BoxMetrics & {
	readonly hasMeasured: boolean;
};
```

Source: `src/hooks/use-box-metrics.ts:33`

| Field | Type | Required | Description |
|---|---|---|---|
| `width` | `number` | yes | Forwarded from `BoxMetrics`. |
| `height` | `number` | yes | Forwarded from `BoxMetrics`. |
| `left` | `number` | yes | Forwarded from `BoxMetrics`. |
| `top` | `number` | yes | Forwarded from `BoxMetrics`. |
| `hasMeasured` | `boolean` | yes | Whether the current ref has been measured in the latest layout pass. |

Returned by:

- [`useBoxMetrics()`](./api-reference/hooks-focus-layout-animation.md#useboxmetrics)

## `CursorPosition`

Import path: `ink`

```ts
export type CursorPosition = {
	x: number;
	y: number;
};
```

Source: `src/cursor-helpers.ts:3`

| Field | Type | Required | Description |
|---|---|---|---|
| `x` | `number` | yes | Cursor column relative to Ink output origin. |
| `y` | `number` | yes | Cursor row relative to Ink output origin. |

Used by:

- [`useCursor()`](./api-reference/hooks-focus-layout-animation.md#usecursor) through `setCursorPosition(position)`

## `DOMElement`

Import path: `ink`

```ts
export type DOMElement = {
	nodeName: ElementNames;
	attributes: Record<string, DOMNodeAttribute>;
	childNodes: DOMNode[];
	internal_transform?: OutputTransformer;
	internal_accessibility?: {
		role?:
			| 'button'
			| 'checkbox'
			| 'combobox'
			| 'list'
			| 'listbox'
			| 'listitem'
			| 'menu'
			| 'menuitem'
			| 'option'
			| 'progressbar'
			| 'radio'
			| 'radiogroup'
			| 'tab'
			| 'tablist'
			| 'table'
			| 'textbox'
			| 'timer'
			| 'toolbar';
		state?: {
			busy?: boolean;
			checked?: boolean;
			disabled?: boolean;
			expanded?: boolean;
			multiline?: boolean;
			multiselectable?: boolean;
			readonly?: boolean;
			required?: boolean;
			selected?: boolean;
		};
	};
	isStaticDirty?: boolean;
	staticNode?: DOMElement;
	previousStaticNode?: DOMElement;
	onComputeLayout?: () => void;
	onRender?: () => void;
	onImmediateRender?: () => void;
	onStaticChange?: () => void;
	internal_layoutListeners?: Set<LayoutListener>;
	parentNode: DOMElement | undefined;
	yogaNode?: YogaNode;
	internal_static?: boolean;
	style: Styles;
} & InkNode;
```

Source: `src/dom.ts:27`

Field summary:

| Field | Type | Required | Description |
|---|---|---|---|
| `nodeName` | `ElementNames` | yes | Host node kind such as `ink-box` or `ink-text`. |
| `attributes` | `Record<string, DOMNodeAttribute>` | yes | Assigned host attributes. |
| `childNodes` | `DOMNode[]` | yes | Child host nodes. |
| `style` | `Styles` | yes | Applied Ink layout and visual style. |
| `parentNode` | `DOMElement \| undefined` | yes | Parent host node. |
| `yogaNode` | `YogaNode \| undefined` | no | Backing Yoga layout node when present. |
| `internal_transform` and remaining `internal_*` fields | internal helper fields | no | Internal renderer and bookkeeping fields that still exist on the public type. |

Accepted or returned by:

- `<Box ref={...}>`
- [`measureElement()`](./api-reference/measurement-and-keyboard.md#measureelement)
- [`useBoxMetrics()`](./api-reference/hooks-focus-layout-animation.md#useboxmetrics)

## `KittyKeyboardOptions`

Import path: `ink`

```ts
export type KittyKeyboardOptions = {
	mode?: 'auto' | 'enabled' | 'disabled';
	flags?: KittyFlagName[];
};
```

Source: `src/kitty-keyboard.ts:40`

| Field | Type | Required | Description |
|---|---|---|---|
| `mode` | `'auto' \| 'enabled' \| 'disabled'` | no | Detection or override mode. |
| `flags` | `KittyFlagName[]` | no | Requested kitty protocol flags. |

Accepted by:

- `RenderOptions.kittyKeyboard`

## `KittyFlagName`

Import path: `ink`

```ts
export type KittyFlagName = keyof typeof kittyFlags;
```

Source: `src/kitty-keyboard.ts:12`

| Field | Type | Required | Description |
|---|---|---|---|
| value | `keyof typeof kittyFlags` | yes | Any exact key from `kittyFlags`. |

Used by:

- `KittyKeyboardOptions.flags`
