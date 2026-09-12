<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/banner-dark.jpg">
  <source media="(prefers-color-scheme: light)" srcset="assets/banner-light.jpg">
  <img src="assets/banner-light.jpg" alt="Y — Curiosity moves you forward. Slowly.">
</picture>

*The world's first Slow-Thinking AI.™*

[**ytho.co**](https://ytho.co) · Knoxville, Tennessee · Patent pending

</div>

---

## What this is

Ask Y anything. Y answers with a better question.

Usually "Why?" Usually within three to five business days.

Other products give you answers. Answers are where curiosity goes to nap. Y
runs a three-step loop instead:

| | |
|:--|:--|
| **1. Ask** | Why is the sky blue? Why do we have to go to bed? Why is it called a driveway? |
| **2. "Why?"** | Y considers your question carefully, then responds with the single most powerful word in science. Then waits. |
| **3. Repeat** | Every "why" opens a new path. Keep going until you figure it out or get called to dinner. |

The company is imaginary. The curiosity is not.

## The origin story

Y is the brainchild invention of **Avie Abell** of Knoxville, Tennessee.

It began the way most good ideas do: a kid asking *why* over and over until the
nearest adult ran out of answers. Somewhere in there came the realization that
the questions were more fun than the answers.

So Avie drew a snail. The snail's shell spiralled into a lowercase `y`. It got
a question mark over its head and a tagline underneath. This repository is
everything that happened after that.

## Running it

```bash
git clone https://github.com/alex-abell/y.git
cd y
open index.html
```

That's it. There is no build step, no toolchain, no dependencies, no lockfile,
no framework, and nothing to install. The entire site is one 47 KB HTML file
with its CSS and JavaScript inline.

This is not laziness. A page about a snail should not need a bundler.

## What's in here

| Path | What |
|:--|:--|
| `index.html` | The whole site. Markup, styles and the Y engine, all inline. |
| `assets/y-mark.png` | Avie's snail, lifted from the brand sheet. Used 5 times on the page. |
| `assets/y-app-icon.png` | Favicon. |
| `assets/y-*.jpg` | Six illustrations: the device, the app, HQ, the trail, the team, the founder portrait. |
| `vercel.json` | Static hosting. Declares that there is nothing to compile. |

Eleven images. One megabyte. Zero build output.

## Brand

The logo is exactly three colours. Not approximately three. Exactly.

| Swatch | Hex | Role | Pixels |
|:--|:--|:--|--:|
| 🟩 | `#A8E123` | Light lime | 30,017 |
| 🟢 | `#91CE09` | Mid lime | 32,420 |
| ⬛ | `#192120` | Pupils, and only the pupils | 708 |

The page palette extends that:

```
--lime       #A3E01F    the brand green
--lime2      #8DC63F    hover and accents
--lime-deep  #5E8F14    text on light backgrounds
--ink        #161C1F    the dark sections
--paper      #FAFBF7    the page ground
```

Values: **Curiosity / Clarity / Progress / A brighter tomorrow.**

## Three things this repo learned the hard way

Every one of these shipped broken first. They are written down so they don't
happen twice.

### 1. A trailing slash took down every image on the page

The page originally referenced its assets relatively, as `assets/y-mark.png`.
Vercel served it at both `/y` and `/y/` with no redirect between them. Relative
URLs resolve against the *directory* of the current URL, so:

| Page URL | `assets/y-mark.png` resolves to | Result |
|:--|:--|:--|
| `/y/` | `/y/assets/y-mark.png` | 200 |
| `/y` | `/assets/y-mark.png` | **404** |

One missing character and all eleven images plus the favicon failed together.
It looked like a flaky CDN or a corrupt file. It was neither.

**Now:** every asset path is absolute. The page does not care how you arrive.

### 2. The snail weighed 199 KB and should have weighed 14 KB

The logo was cropped from a brand sheet whose lime green carried JPEG grain. So
a flat two-colour drawing was storing **13,774 distinct colours** at 1.13 bytes
per pixel, and it was the most-requested asset on the page.

Snapping every pixel to the three colours the mark actually uses took it from
**199 KB to 14 KB**, a 13x reduction, with no visible difference at any size on
either background.

> **The trap:** the first attempt used automatic median-cut quantisation. Median
> cut allocates palette slots by volume, and the pupils are only 686 pixels out
> of 180,180. It deleted every one of them and turned the googly eyes solid
> green. Always check that the small important thing survived the optimisation
> that was measured on the big unimportant thing.

### 3. Rewrites lose to the filesystem

While the page lived in a subdirectory of a larger site, `ytho.co` needed its
root mapped to `/y/`. A host-scoped `rewrite` in `vercel.json` was the obvious
tool. It deployed cleanly and never fired once.

**Vercel applies `rewrites` only *after* checking the filesystem.** A request
for `/` matched the site's existing root `index.html`, so the static file won
every time and the rewrite was skipped silently. No error, no warning, no log.

`routes` are evaluated *before* the filesystem and worked immediately. They
cannot be combined with `trailingSlash`, `cleanUrls`, `redirects` or `headers`,
which is the cost.

**Now:** none of this applies. In its own repository the page *is* the root, so
`index.html` is served at `/` by default and the routing rules are gone.
Splitting the repo deleted the problem instead of configuring around it.

## Deployment

Pushes to `main` deploy to [ytho.co](https://ytho.co) via Vercel. No build
command, no install step, `outputDirectory` is the repo root.

To change something: edit `index.html`, push, done.

## Roadmap

| Quarter | Goal | Status |
|:--|:--|:--|
| Q1 | Ask why | Complete |
| Q2 | Still asking | Ongoing, strong momentum |
| Q3 | Getting somewhere | We can feel it |
| Q4 | A brighter tomorrow | Right on schedule |

## FAQ

**Is Y real?**
The idea is real and so is the kid who had it. The company is imaginary. The
snail is emotionally real to everyone involved.

**Why a snail?**
Snails carry their home with them, never rush, and always leave a trail showing
where they've been. Also the shell already looked like a question mark.

**How fast is Y?**
Yes.

**Can I add salt?**
No.

**What happens if I answer Y's question?**
Y asks why. Then you answer that. Then Y asks why. Somewhere in there you
figure something out. That part isn't a joke. That part is the whole idea.

## Credits

Invented, named, drawn and tagged by **Avie Abell**, Knoxville, Tennessee.

Built out by her dad, who mostly just kept asking why until it worked.

<div align="center">

<img src="assets/snail-progress.jpg" alt="A row of Y snails, each one a little further along than the last" width="620">

**Curiosity moves you forward.**

*Slowly.*

</div>
