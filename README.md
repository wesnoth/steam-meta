Wesnoth Steam Store i18n Kit
============================

This repository contains the Wesnoth Steam metadata files and the toolchain
required to localize them into other languages. You will need to have **GNU
gettext**, **GNU make**, **GNU sed**, **po4a**, and **dos2unix** installed
in order to build the metadata translations.

**Translators need not worry about building metadata themselves.** For more
information about the process used to add or update translations, see
`CONTRIBUTING.md`.

Dependencies
------------

Debian:

```
sudo apt-get install make po4a gettext dos2unix
```

Build instructions
------------------

```
# Does all of the tasks below (except clean and distclean) in a single step.
make

# Updates the catalogue template.
make update-pot

# Merges translation catalogues with the updated template.
make update-po

# Rebuilds translated text files from catalogues.
make update-txt

# Cleans files used to keep track of the build status, as well as any backups
# of translation catalogues left behind by po4a.
make clean

# Deletes translated text files. Only used for toolchain debugging.
make distclean
```
