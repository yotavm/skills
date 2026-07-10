# CrossPoint Spotify "Now Playing" mode — patch delivery

This branch temporarily carries a feature patch for a **different repo**
(`crosspoint-reader`), because the Claude session that built it could not
push to the `yotavm/crosspoint-reader` fork (the repo was never added to
the session's authorized sources). Delete these two files once the patch
lands in the fork.

## What it is

A complete, self-contained commit adding a Spotify Now Playing mode to
CrossPoint Reader (Xteink X3/X4 e-ink firmware): album artwork dithered
for e-ink, track/artist, progress bar, QR-driven OAuth PKCE setup, and 12
host unit tests (all passing). Full docs are inside the patch at
`docs/spotify.md`.

- Base commit: `4b34a57` (crosspoint-reader `develop`, July 2026)
- Patch commit: `feat: add Spotify Now Playing mode with album artwork`
- 25 files, +1795 lines

## How to apply

```bash
git clone https://github.com/yotavm/crosspoint-reader.git
cd crosspoint-reader
git checkout -b feature/spotify-now-playing
git am /path/to/crosspoint-spotify-mode.patch
git push -u origin feature/spotify-now-playing
```

(`git am` preserves the commit message and authorship; if the base has
moved, `git am -3` does a three-way merge.)

## Verifying

```bash
# Host unit tests (needs cmake + ninja + g++)
cmake -S test -B build/test -G Ninja -DCMAKE_BUILD_TYPE=Release
cmake --build build/test
ctest --test-dir build/test --output-on-failure

# Firmware build (needs PlatformIO; also runs automatically in the repo's CI)
pio run
```

The easier path: ask Claude to add `yotavm/crosspoint-reader` to a session
(approve the permission prompt when it appears) and it can push the branch
and babysit CI directly.
