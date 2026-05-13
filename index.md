# ink

React renderer for building command-line interfaces with Flexbox-based layout and terminal input/output hooks.

Install:

```sh
npm install ink react
```

Top-level entry point: `ink` (`src/index.ts` re-exporting the symbols below).

| Symbol | Kind | Source file | One-line description |
|---|---|---|---|
| `render` | function | `src/render.ts` | Mounts a React tree into a terminal session and returns an `Instance` controller. |
| `RenderOptions` | type | `src/render.ts` | Options object for `render()`, including streams, interactivity, throttling, and keyboard support. |
| `Instance` | type | `src/render.ts` | Control surface returned by `render()`. |
| `renderToString` | function | `src/render-to-string.ts` | Synchronously renders a React tree to a string without a live terminal session. |
| `RenderToStringOptions` | type | `src/render-to-string.ts` | Options object for `renderToString()`. |
| `Box` | component | `src/components/Box.tsx` | Flexbox container component for layout. |
| `BoxProps` | type | `src/components/Box.tsx` | Public prop type for `<Box>`. |
| `Text` | component | `src/components/Text.tsx` | Text output component with Chalk-backed styling and wrapping. |
| `TextProps` | type | `src/components/Text.tsx` | Public prop type for `<Text>`. |
| `AppProps` | type | `src/components/AppContext.ts` | Return shape of `useApp()`. |
| `StdinProps` | type | `src/components/StdinContext.ts` | Public return shape of `useStdin()`. |
| `StdoutProps` | type | `src/components/StdoutContext.ts` | Return shape of `useStdout()`. |
| `StderrProps` | type | `src/components/StderrContext.ts` | Return shape of `useStderr()`. |
| `Static` | component | `src/components/Static.tsx` | Renders append-only historical output above the live tree. |
| `StaticProps` | type | `src/components/Static.tsx` | Public prop type for `<Static>`. |
| `Transform` | component | `src/components/Transform.tsx` | Applies a string transform to rendered children output. |
| `TransformProps` | type | `src/components/Transform.tsx` | Public prop type for `<Transform>`. |
| `Newline` | component | `src/components/Newline.tsx` | Inserts one or more newline characters inside `<Text>`. |
| `NewlineProps` | type | `src/components/Newline.tsx` | Public prop type for `<Newline>`. |
| `Spacer` | component | `src/components/Spacer.tsx` | Expanding flex spacer shorthand. |
| `useInput` | hook | `src/hooks/use-input.ts` | Subscribes to parsed keyboard input. |
| `Key` | type | `src/hooks/use-input.ts` | Parsed key metadata passed to `useInput()` handlers. |
| `usePaste` | hook | `src/hooks/use-paste.ts` | Subscribes to terminal bracketed-paste events. |
| `useApp` | hook | `src/hooks/use-app.ts` | Reads app lifecycle controls from context. |
| `useStdin` | hook | `src/hooks/use-stdin.ts` | Reads the input stream and raw-mode helpers. |
| `useStdout` | hook | `src/hooks/use-stdout.ts` | Reads the render output stream and safe string writer. |
| `useStderr` | hook | `src/hooks/use-stderr.ts` | Reads the error stream and safe string writer. |
| `useFocus` | hook | `src/hooks/use-focus.ts` | Registers a component in Ink’s focus system. |
| `useFocusManager` | hook | `src/hooks/use-focus-manager.ts` | Returns imperative focus-management methods. |
| `useIsScreenReaderEnabled` | hook | `src/hooks/use-is-screen-reader-enabled.ts` | Returns the resolved screen-reader mode flag. |
| `useCursor` | hook | `src/hooks/use-cursor.ts` | Exposes cursor-position control for the current render commit. |
| `useAnimation` | hook | `src/hooks/use-animation.ts` | Returns frame/time counters driven by Ink’s shared animation scheduler. |
| `AnimationResult` | type | `src/hooks/use-animation.ts` | Return type of `useAnimation()`. |
| `useWindowSize` | hook | `src/hooks/use-window-size.ts` | Tracks terminal `columns` and `rows`. |
| `WindowSize` | type | `src/hooks/use-window-size.ts` | Terminal size object returned by `useWindowSize()`. |
| `useBoxMetrics` | hook | `src/hooks/use-box-metrics.ts` | Tracks computed layout metrics for a `<Box>` ref. |
| `BoxMetrics` | type | `src/hooks/use-box-metrics.ts` | Width/height/position metrics for a box. |
| `UseBoxMetricsResult` | type | `src/hooks/use-box-metrics.ts` | `BoxMetrics` plus `hasMeasured`. |
| `measureElement` | function | `src/measure-element.ts` | Reads computed `width` and `height` from a `DOMElement`. |
| `CursorPosition` | type | `src/cursor-helpers.ts` via `src/log-update.ts` | Cursor coordinates relative to Ink output origin. |
| `DOMElement` | type | `src/dom.ts` | Internal host node shape exposed for refs and measurements. |
| `kittyFlags` | constant | `src/kitty-keyboard.ts` | Named kitty keyboard protocol flag bit values. |
| `kittyModifiers` | constant | `src/kitty-keyboard.ts` | Named kitty keyboard protocol modifier bit values. |
| `KittyKeyboardOptions` | type | `src/kitty-keyboard.ts` | `RenderOptions.kittyKeyboard` configuration. |
| `KittyFlagName` | type | `src/kitty-keyboard.ts` | Valid string keys of `kittyFlags`. |

Reference pages:

- [render](./api-reference/render.md)
- [renderToString](./api-reference/render-to-string.md)
- [components](./api-reference/components.md)
- [hooks: lifecycle and streams](./api-reference/hooks-lifecycle-and-streams.md)
- [hooks: focus, layout, animation, cursor](./api-reference/hooks-focus-layout-animation.md)
- [measurement and keyboard protocol](./api-reference/measurement-and-keyboard.md)
- [types](./types.md)
- [configuration](./configuration.md)
