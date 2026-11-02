# Homepage layout

The homepage (`index.html`) is one justified paragraph, so its `max-width` in `_sass/_home.scss` is tuned to the exact content. Any change to homepage text, sections, order or font size invalidates it: re-measure the width in the same change.

## Measuring the width

Measure on the served page (`rake watch`, http://localhost:1982) across a range of `ch` widths. For each width, keep it justified and measure the widest gap between adjacent words or items on each line, in `ch` (a plain space is 1ch). Ignore the last line. Measure the gaps themselves, not the line's total leftover space: a line with few gaps shows its leftover as a few big holes. Pick the width with the smallest worst gap, staying at or below about 60ch (wider reads as too wide), then confirm with screenshots of the top candidates.

## Hyphenation

Hyphenation is on, so reject a width that splits a name or tool (`her-dr`); the numbers don't show this. Wrap tool names, emails and other literals in `<span class="nohyphen">`.

## Phones

Phones stay justified, but their line length is set by the device, so the width tuning is desktop-only. Still check phone widths (320–430px) for horizontal overflow and broken literals; headless Chrome on macOS won't shrink its viewport that far, so render the page in iframes of those widths.
