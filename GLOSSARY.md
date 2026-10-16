# Glossary

## Phase

The pomodoro clock's own state: `Focus` or `Break`. Driven purely by the timer,
alternating in a flat 25/5 loop. Independent of whether anyone is paying
attention to the page.

## Pose

A blobatar expression — the vocabulary blobatar's own docs use for a value
imported from `blobatar/expression` (`thinking`, `happy`, `sleepy`, …). We keep
this name rather than inventing a synonym, since it's the library's term for
the thing it renders.

## Expression driver

Whichever one of {Phase, session-end burst, Dozing} is currently deciding which
Pose the widget shows. Exactly one wins at a time: Dozing overrides the other
two whenever it's active, regardless of Phase.

## Session-end burst

The fixed ~2–3s window right after `Focus` ends where the driver is the burst
itself (showing `happy`), before `Break`'s `sleepy` takes over. Not part of
Phase — it's a one-shot interruption between phases.

## Dozing

The inactivity-driven Expression driver. Independent of Phase — it can become
active during either `Focus` or `Break`, including mid-session. Activated and
advanced by the absence of Activity; cleared the instant Activity resumes.

## Dozing stage

One step along Dozing's progression (e.g. sleepy-only → sleepy + 1 z → sleepy
+ 2 z's → …). The count of stages and their thresholds are tuning values,
decided in a prototype ticket, not fixed here. Note: Dozing's first stage
happens to reuse the same Pose (`sleepy`) that `Break` shows — same Pose,
different driver, by coincidence of the roster rather than by design.

## Activity

A mouse, keyboard, or touch event anywhere on the page — not scoped to
hovering or clicking the widget itself. The sole signal that resets Dozing to
its baseline (no dozing) and the sole signal that advances it (its absence,
over time).

## Decoration layer

The sibling DOM element the widget renders beside blobatar's `<svg>` to draw
the sleep z's. Purely presentational, positioned relative to blobatar's box,
and never reaches into blobatar's own markup — blobatar itself only ever shows
`sleepy` during Dozing; the z's are the widget's, not the library's.

## Readout

The plain visible text (Phase name + time remaining) that carries the widget's
real, non-decorative state. Required because Poses are decorative and don't
reach assistive technology — the Readout is what a screen reader (or anyone
not parsing a blob's mood) actually relies on.
