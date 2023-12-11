# portage overlay

### Listing the available overlays

To see the list of overlays available, run:

`root #``layman -L`

### Installing an overlay

To install an overlay on your computer, run:

`root #``layman -a <overlay-name>`

For example, to install the PHP overlay, run:

`root #``layman -a php`

### Installing packages from an overlay

After installing an overlay, you can install packages from it by running:

`root #``emerge -av <category>/<package>`

Portage automatically searches your Gentoo main tree (in /usr/portage ) and all of the overlays that you've installed, and picks the latest version of the package that it can find.

If Portage isn't picking up the package from the overlay, that's normally because the package is marked ~arch, where "arch" is the architecture of your computer. You'll need to keyword the package as explained in the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page).

### Updating an overlay

To keep your installed overlays up to date, run:

`root #``layman -S`
