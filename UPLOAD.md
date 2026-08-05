# what to upload where

Generated 2026-08-05. Everything below is already built, signed and verified
locally.

## 1. gx-repo (https://generatrixproject.github.io/gx-repo)

Upload the whole contents of `x86_64/` into the repository's `x86_64/`
directory, overwriting what is there.

- `APKINDEX` and `APKINDEX.tar.gz` are the merged index. They already contain
  the 79 packages, including the ones that did not change this round, so the
  old apks on the server stay valid. Do not regenerate them.
- `index.html` is the human readable listing for the same directory.
- The `.apk` files are the new or changed packages only.

Two of them are fixes that matter to existing users:

- `world-1.0-r6.apk` - the dependency walker skipped only `so:libc.musl-`, so
  any resolve that pulled our own libidn2 died with `package not found:
  libc.so`. Also carries the libstdc++ ban and the gdbm unban.
- `libidn2-2.3.7-r0.apk` - repackaged without the bogus `so:libc.so`
  dependency that caused the above.

The rest are the static libc++ rebuilds:

zstd, lzip, jemalloc, py3-greenlet, libyuv, tesseract-ocr, libflac++,
libplist++, libconfig++, uchardet-libs, x265-libs, openal-soft-libs, geos,
zopfli, libmarisa, libsass, libgpiod, py3-ujson, libtiffxx, proj, syslog-ng,
ghostscript, graphviz, nmap, php85-intl, php83-intl, py3-numpy, py3-pandas.

## 2. gitlab (gitlab.com/generatrix-linux/generatrix-linux)

Source changes in this round:

- `scripts/cxxbuild.sh` (new) - shared plumbing for the libc++ rebuilds.
- `build-cxx.sh` (new) - driver that runs the whole rebuild round.
- `scripts/59-*.sh` - the new per package rebuild scripts (flac, geos,
  ghostscript, gpiod, graphviz, greenlet, jemalloc, libconfig, lzip, marisa,
  nmap, numpy, openal, pandas, phpintl, plist, proj, sass, syslogng,
  tesseract, tiffxx, uchardet, ujson, x265, yuv, zopfli, zstd).
- `scripts/00-hostcheck.sh` - checks autoconf, automake, libtool, bison, flex,
  scons and pkg-config now.
- `scripts/60-rootfs.sh` - world is 1.0-r6.
- `scripts/69-idn2.sh` - no bogus libc dependency in the package.
- `world/world.c` - skips `so:libc.` in general, libstdc++ in the ban list.
- `config.sh` - the new VER_* entries.
- `CLAUDE.md` - the notes for the round.

## 3. site (generatrixproject.github.io)

Nothing is required, but a changelog entry for this round would be accurate:
the repository grew from 52 to 80 packages and the number of Alpine packages
blocked by the libstdc++ ban fell from 7803 to 7239 out of 35983.

## 4. iso release

Rebuilt images carry world 1.0-r6:

- `~/generatrix-rolling-x86_64.iso` (12.98 MB)
- `~/generatrixgui-rolling-x86_64.iso` (18.27 MB)

Upload them to the `rolling` release and send me the new links so the download
section can be updated.
