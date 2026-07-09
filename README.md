# Caffeine Half-Life Calculator

A single self-contained HTML tool that estimates how much caffeine is in your
system throughout the day, based on what you drink and when. Built to be
hosted on GitHub Pages and embedded directly in a Notion page.

## What it shows

- **In system now** — your estimated caffeine level at this exact moment
- **At bedtime** — projected level at the bedtime you set, with a clear
  ✓ / ! status message telling you whether you'll be under your target
- **Under target by** — if you won't be under target at bedtime, the
  calculator tells you the actual clock time it expects you to drop below it
- **Peak level** — the highest level your system is projected to reach today
- **A live decay curve** — a 24-hour chart of caffeine level over time, with
  your target threshold and bedtime marked, and a shaded area under the
  curve

## Why it's relevant

Caffeine doesn't just "wear off" — it follows a predictable pharmacokinetic
curve, and how much is still active in your body at bedtime is one of the
biggest levers on sleep quality. Two coffees at 8am are a non-issue by 11pm;
the same two coffees at 4pm can still leave 30–40mg circulating at midnight,
which is enough to measurably delay sleep onset and reduce deep sleep for
many people. This tool turns "how many hours before bed should I stop
drinking coffee" from a rule of thumb into an actual number, personalized to
what you drink and when.

## The model

Caffeine absorption and elimination is modeled as a standard one-compartment
pharmacokinetic curve with first-order absorption:

```
C(t) = Dose × [ka / (ka − ke)] × (e^(−ke·t) − e^(−ka·t))
```

- **ke** (elimination rate) is derived from the half-life: `ke = ln(2) / half-life`
- **ka** (absorption rate) is solved so that each dose peaks roughly 30–45
  minutes after intake, matching how quickly caffeine is actually absorbed
  after drinking coffee, tea, or soda
- Total system level at any time is the sum of every logged drink's
  individual curve

Defaults are set to a 5.7-hour half-life (a commonly cited average) and a
40mg target at bedtime — both fully adjustable, since actual half-life
varies by genetics, liver enzyme activity, pregnancy, smoking, and
medications, and "safe" bedtime levels vary by individual caffeine
sensitivity.

**Time handling:** each drink is anchored to a fixed real-world moment. If a
drink's clock time is still ahead of the current time, it's assumed to have
happened yesterday rather than later today — this lets the curve carry
leftover caffeine smoothly across midnight into the next day's view, rather
than resetting at midnight.

## Presets

Espresso, V60 drip coffee, and Coke are pinned to the top of the drink list
with custom mg values, plus three one-tap quick-add buttons that log a drink
at the exact moment you click — useful for logging in real time as you
drink.

## Using it

1. Set your half-life and target level at bedtime under **Parameters**
2. Log drinks either through **Quick add** (logs instantly, at the current
   time) or **+ Add drink** (lets you pick a preset/custom drink and any
   time)
3. Read the **Forecast** panel and chart to see your projected level and
   whether you'll hit your target by bedtime

## Hosting and embedding in Notion

1. Push `index.html` to a GitHub repository (the root filename must be
   `index.html` for GitHub Pages to serve it at the repo's root URL)
2. Enable GitHub Pages in the repo's **Settings → Pages**
3. In Notion, type `/embed` and paste the resulting Pages URL

## Disclaimer

This is an estimation tool, not medical advice. Actual caffeine metabolism
varies significantly between individuals. If you have specific health
concerns related to caffeine or sleep, consult a doctor.
