# ThumbFree for iPhone

**Type with your voice, not your thumbs. 100% offline.**

A voice keyboard for iPhone: tap the mic, speak, tap again, and your words are typed into whatever app you're using,
cleaned up and punctuated. Speech is recognised on the phone; nothing you say is uploaded, and there's no account.

**Status: planned.** Work starts after the Mac app. The design is in [docs/ios.md](docs/ios.md).

ThumbFree also runs on [Android](https://github.com/shivam098/thumbfree-android) and
[Windows](https://github.com/shivam098/thumbfree-windows), and a [Mac](https://github.com/shivam098/thumbfree-mac)
version is in development. Every app follows the same feature reference:
[PLAN.md, section 3](https://github.com/shivam098/thumbfree-android/blob/main/docs/PLAN.md#3-feature-reference-built-on-android-copy-to-every-platform).

## Test data

`tests/vectors` (how each test sentence must come out) and `tests/audio` (speech recordings) are the same files in
every ThumbFree repo. The **Test data in sync** check fails while this copy differs from the others: when you change
one, copy the change to all four repos.
