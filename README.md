# Debian package for MathLM

## Description

Debian rules to generate a Debian package for [MathLM](https://reference.wolfram.com/language/tutorial/WhatIsMathLM.html) for amd64 architecture. The MathLM installer `M-UNIX-LM-*.sh` corresponding to the version in changelog has to be provided to build the package.

The generated Debian package:
- installs MathLM executables in `/usr/sbin`
- installs man pages
- adds mathlm user
- adds systemd service

## Package build

1. Define the desired version:

        export VERSION=13.2.0

1. Clone the git repository containing the packages rules in an empty directory:

        git clone -b $VERSION https://github.com/guillod/deb-mathlm.git mathlm-$VERSION
        cd mathlm-$VERSION

2. Put the MathLM installer `M-UNIX-LM-*.sh` in the current directory (`mathlm-$VERSION`).
3. Generate the package for example with:

        debuild --no-lintian -i -b -uc -us

    or `sbuild`.

## Installation

Once generated, the package `mathlm_$VERSION_amd64.deb` can be installed for example with:
```
sudo apt install ../mathlm_$VERSION_amd64.deb
```

A valid license file has to be provided in `/etc/mathlm/mathpass`.
To generate this file on Wolfram website, the MathID has to be provided:

    mathlm -mathid

Once the license file is added, the mathlm service can be restared using:

    sudo systemctl restart mathlm

The port `16286` is used by MathLM so has to be opened, for example with:

    sudo ufw allow from 127.0.1.0/24 port 16286 to any


## Debugging

The log of mathlm are handled by journald, so can be shown using:
```
journalctl -u mathlm
```
The status of the license server can be checked by running `monitorlm`.

## Contact

For comments, issues, bug-reports and requests, please use the issue tracker of the current repository. Otherwise the principal author can be reached at:

    Julien Guillod
    julien.guillod CHEZ sorbonne-universite.fr
    https://guillod.org/
    Department of Mathematics
    Sorbonne University
    France
