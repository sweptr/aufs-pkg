aufs-pkg
========
Scripts to manage `/usr/local` with aufs branches, to keep packages in their own directories. Inspired by [A UnionFS-based package management system](http://www.linuxfromscratch.org/hints/downloads/files/pkg_unionfs.txt) from Linux From Scratch Hints Project.

Installation
------------
```bash
cp usr-local /etc/init.d # install the Gentoo initscript
cp upkg some_location_in_your_PATH
```

Preparation
-----------
```bash
mkdir -p /usr/package/old
mkdir /usr/package/@enabled

cd /usr/package/old
mv /usr/local/* .
cp /usr/local/.keep_sys-apps_baselayout-0 .

/etc/init.d/usr-local start
rc-update add usr-local default
upkg enable old

# Check if /usr/local looks the same as before
ls -l /usr/local
```

Usage
-----
To install a new package (e.g. `megatools`):
```bash
mkdir /usr/package/megatools
upkg enable megatools
upkg rw megatools # Might not work currently

# install the package
sudo make install # Assuming it installs everything into /usr/local

upkg ro megatools
```

Now you can make the files from the package disappear from `/usr/local` with:
```bash
upkg disable megatools
```

To make them appear again:
```bash
upkg enable megatools
```

To uninstall the package:
```bash
upkg disable megatools
rm -rf /usr/package/megatools
```
