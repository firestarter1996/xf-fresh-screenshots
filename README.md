# 📸⚡🖼️ Fresh Screenshots

A plugin for Nagram XF. Photos and screenshots you just took show up in the attach sheet right away, first in line with their thumbnail, instead of several seconds later.

## Why I made it

Right after taking a screenshot I would open the attach sheet in Nagram XF and the newest one or two shots were missing. They showed up seconds later, or only after closing and opening the sheet again.

## The cause

Nagram XF (like Telegram) only learns about a new photo through a full rescan of the whole gallery:

* Its gallery observer waits 2 seconds after the last change before doing anything, and every write of a screenshot starts that wait over.
* Then a lowest priority thread reads every photo and video on the phone and sorts every album again. With tens of thousands of photos that takes seconds.
* Every time you come back to the app it also compares the photo count with the photo plus video count. Those never match when you have any video, so it rescans everything on every return too.

## What the plugin does

* Its own gallery observer reacts 120 ms after a change and reads only the newest rows, then puts the new photos and videos at the top of the gallery and of their album, the same way the app publishes a rescan.
* The same quick check runs every time the attach sheet opens.
* On returning to the app, the full rescan only runs when the number of photos and videos really changed.
* The app's own rescan still runs after a change, so deleted and edited photos behave exactly as before.

## Performance

No polling and no timers: the work runs only when the gallery changes or the sheet opens, on its own background thread, and reads at most 32 rows. The screen thread only swaps the lists. The quick update took 20 to 40 ms on my test phone. Logcat tag `XFFresh`, one line per new photo.

## Measured

Pixel 6 Pro, Nagram XF 12.10.1 plugin build, attach sheet open, a real screenshot taken with the side keys:

* Before: the new screenshot appeared in the sheet about 3.5 to 4 seconds after the press (on a phone with about 7,000 photos; more photos means longer)
* After: about 1.6 seconds after the press, with its thumbnail by 2 seconds. Most of that is Android saving the screenshot file; the plugin adds it about 20 ms after Android saves it

Two screenshots taken 2.5 seconds apart both showed up at the top, newest first.

## Install

1. Use the plugin build of Nagram XF and turn on the plugin engine (Settings, N Settings, Plugins).
2. Download `fresh_screenshots.plugin` from the latest release.
3. Send it to your Saved Messages, tap it and choose install.

Switching the plugin off removes its observer and hooks and the gallery works as stock.

## License

MIT

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=firestarter1996/xf-fresh-screenshots&type=Date)](https://star-history.com/#firestarter1996/xf-fresh-screenshots&Date)
