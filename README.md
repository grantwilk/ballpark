# Ballpark

A one-page guessing game. It asks what percent of American adults belong to
each of 45 groups, one question at a time. At the end, it puts each guess on a
chart next to the average American guess and the true figure.

Live site: https://grantwilk.com/ballpark/

## Where the numbers come from

The questions, the average guesses, and the true figures come from a YouGov
poll of US adult citizens, January 14 to 20, 2022:

- Article: [From millionaires to Muslims, small subgroups of the population
  seem much larger to many Americans](https://yougov.com/en-us/articles/41556-americans-misestimate-small-subgroups-population),
  Taylor Orth, March 15, 2022.
- Chart: "Americans overestimate the size of minority groups and underestimate
  the size of most majority groups" (Datawrapper `gwI93`).

Each figure was transcribed from the chart and checked against the article
text. The chart rounds every figure to a whole percent. The one exception on
this page is transgender adults: the article gives the true figure as 0.6%, and
the page shows 0.6%.

YouGov drew the true figures from the US Census Bureau, the Bureau of Labor
Statistics, and other polls. Most are from 2019 to 2021, so some have changed
since then.

## How it works

1. **Start.** The first screen has two choices: start guessing, or skip
   straight to the results. The results without guesses show the original
   chart: the average guess and the true figure.
2. **Guess.** Each question is one screen, with a slider from 0% to 100%. Tap
   the track, drag the thumb, or use the minus and plus buttons. On a keyboard,
   the arrow keys move the guess by 1, and Page Up and Page Down move it by 10.
   You can go back, skip a question, or finish early.
3. **Compare.** The results show three markers on each row: your guess (a
   diamond), the average guess (blue), and the true figure (red). Three numbers
   above the chart give your average miss, the average American's miss on the
   same questions, and how many times you came closer than the average.

The page does not show an answer until the results. The questions come in a
random order, because the chart's own order is sorted by the true figure and
would give the answers away.

## Share links

The results screen has a link to copy. The link carries the guesses itself,
after a `#`:

```
https://grantwilk.com/ballpark/#r=1lLoVUw3_hFEyOtUXhpdHKUJ7NkOWvGsIZkYo00KFPOgOnwQGVP2HoA
```

Nothing is stored anywhere. Browsers do not send the part after `#` to the
server, so GitHub Pages never sees the guesses. Only people who have the link
can see the results.

The code is 45 guesses in the chart's order, 7 bits each (0 to 100, or 127 for
a skipped question), packed into bytes and written as base64url. The leading
`1` is a format version. If the questions ever change, use a new version
number, so that old links do not decode into the wrong questions.

A shared link opens straight to the shared results. A box at the top invites
the visitor to take the quiz. A visitor who does gets a button at the end to go
back to the shared results and compare.

## Storage

Guesses in progress live in `sessionStorage`. A reload keeps your place, and
closing the tab clears it. The theme choice lives in `localStorage`.

## Files

| File | What it is |
|---|---|
| `index.html` | The whole site. All the CSS and JavaScript are inline. The data is the `DATA` array near the top of the script. |
| `favicon.svg` | The tab icon. It has two palettes and changes with the browser theme. |
| `favicon-32.png`, `apple-touch-icon.png` | Raster copies of the icon. |
| `social-card.png` | The 1200x630 preview image for shared links. It uses made-up positions, so it gives no answers away. |
| `.nojekyll` | Tells GitHub Pages to copy the files as they are. |
| `sitemap.xml` | One entry, for search engines. |

## Running it locally

The site has no build step. Serve the folder:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000/. Copy needs a secure context, so it uses a
fallback on a plain `file://` page.

## Deployment

GitHub Pages serves the `main` branch from the repository root. A push to `main`
publishes the site.

This repository is a project site. It inherits the custom domain of the user
site at `grantwilk.github.io`, so it is published under `grantwilk.com` and not
under `github.io`.

## Testing

The page was tested in a real browser at five sizes (320, 360, 390, 820, and
1280 pixels wide), in light and dark themes. The checks cover the whole flow:
skip to results, tap and drag on the slider, the step buttons, the keyboard,
back and skip, reload in the middle, the score arithmetic, sorting, copying
the share link, opening it as another person, both paths through a shared
link, no horizontal scrolling, and no script errors. The share code was
round-tripped on 3,000 random sets of guesses.
