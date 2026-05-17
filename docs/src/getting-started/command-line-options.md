# Command Line Options

## -n, --max-count \<NUMBER\>

Maximum number of commits to render.

If not specified, all commits will be rendered.
It behaves similarly to the `--max-count` option of `git log`.

## -p, --protocol \<TYPE\>

The protocol used to render commit graphs.

_Possible values:_ `auto`, `iterm`, `kitty`, `kitty-unicode`, `ascii`

By default `auto` will pick the best supported mode for the current terminal:

1. Kitty graphics protocol if `KITTY_WINDOW_ID`, `GHOSTTY_RESOURCES_DIR`, or `TERM=xterm-ghostty` is set (uses the Unicode-placeholder variant under tmux).
2. iTerm2 inline images if `TERM_PROGRAM` is `iTerm.app`, `WezTerm`, `mintty`, or `vscode`, or if `LC_TERMINAL=iTerm2`.
3. Otherwise the `ascii` fallback, which uses Unicode box-drawing characters and works in any terminal.

The `ascii` value can also be selected explicitly — useful if `auto` misidentifies your terminal or if you just want plain text. ASCII rendering honors `--graph-style rounded|angular` (corner glyphs) and `--graph-width single|double` (single character per branch column, or two characters with `●`/`○` and arrow markers on merge commits).

## -o, --order \<TYPE\>

Commit ordering algorithm.

_Possible values:_ `chrono`, `topo`

`chrono` will order commits by commit date if possible.

<img src="https://raw.githubusercontent.com/lusingander/serie/master/img/order-chrono.png" width=300>

`topo` will order commits on the same branch consecutively if possible.

<img src="https://raw.githubusercontent.com/lusingander/serie/master/img/order-topo.png" width=300>

## -g, --graph-width \<TYPE\>

The character width that a graph image unit cell occupies.

_Possible values:_ `auto`, `double`, `single`

If not specified or `auto` is specified, `double` will be used automatically if there is enough width to display it, `single` otherwise.

<img src="https://raw.githubusercontent.com/lusingander/serie/master/img/graph-width-double.png" width=300>

<img src="https://raw.githubusercontent.com/lusingander/serie/master/img/graph-width-single.png" width=300>

</details>

## -s, --graph-style \<TYPE\>

The commit graph image edge style.

_Possible values:_ `rounded`, `angular`

`rounded` will use rounded edges for the graph lines.

<img src="https://raw.githubusercontent.com/lusingander/serie/master/img/graph-width-double.png" width=300>

`angular` will use angular edges for the graph lines.

<img src="https://raw.githubusercontent.com/lusingander/serie/master/img/style-angular.png" width=300>

## -i, --initial-selection \<TYPE\>

The initial selection of commit when starting the application.

_Possible values:_ `latest`, `head`

`latest` will select the latest commit.

`head` will select the commit at HEAD.

## --watch \[\<SECONDS\>\]

Enable automatic refresh of the view on a fixed interval.

If passed without a value, the interval defaults to 30 seconds.
Values below 5 seconds are clamped to 5.

The refresh only re-reads local git state — it does not contact any remote.
External git activity (commits made in another terminal, fetches done by
another tool) is picked up automatically as soon as it lands on disk.

The current cursor / scroll position is preserved across refreshes.
Ticks that arrive while you are typing in the search prompt are silently
skipped so they do not interrupt input.

## --fetch

Run `git fetch --all --quiet` before each auto-refresh tick.

Implies `--watch`: if `--watch` is not specified, the interval defaults
to 30 seconds. Combine with `--watch <SECONDS>` to override.

Fetch errors (network unreachable, authentication failures, etc.) are
silently ignored so the refresh tick still fires with whatever local
state is available.
