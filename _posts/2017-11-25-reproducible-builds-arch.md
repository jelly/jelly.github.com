---
layout: post
title: "Reproducible Arch Linux?!"
date: 2017-11-26 13:37
comments: false
categories: [Arch, Linux, reproducible, builds, security ]
---

The reproducible build initiative has been started a long time ago by Debian and has been grown to include more projects. Arch is now also
in the process of getting reproducible build support, thanks to the of hard work of Anthraxx, Sangy, and many more
volunteers. In pacman git patches where landed to support reproducible builds which will be included in a hopefully soon next stable
release! Meanwhile with help of the reproducible-builds.org rebuild infrastructure rebuilds have been started!

Currently 77% of the 17% tested packages are reproducible as can be found
[here](https://tests.reproducible-builds.org/archlinux/archlinux.html). This page is fed by the work done by two [Jenkins
builders](https://jenkins.debian.net/job/reproducible_builder_archlinux_2/), which currently build the whole Arch repository.
The builder builds the package twice in different environments and then uses [diffoscope](https://diffoscope.org/) to find differences in
packages. Usually the differences are due to timestamps :-). Now that we have some results of rebuilds, we can start fixing our packages. The work I did so far:

* Fixing 404 sources of our packages, some of the source failures where due to ftp://kernel.org being used and not https://www.kernel.org.
This has been fixed in SVN. Also old pypi links needed to be [fixed](https://git.archlinux.org/svntogit/packages.git/commit/?id=c93ea7238a2f06d295ae4df375fa33e708b90bcf)

* One package's .install file contained a killall statement, I'm not sure why but it _shouldn't_ be required so it was [eradicated](https://git.archlinux.org/svntogit/packages.git/commit/?id=29c6fe344f1a7bb40f8646669b35901f750ba85d)

* Integrity mismatch, so upstream did a ninja re-release, annoying but [fixed](https://git.archlinux.org/svntogit/community.git/commit/?id=e7b0b7bcbf87196f29c196427450758d6be6a77c)

* Imagemagick's convert sets some metadata in the resized png's which makes [reproducible builds
fail](https://git.archlinux.org/svntogit/community.git/commit/?id=ef304fa65de904746ec0c53e777f5432e6b7f584). Since it does not adhere to
SOURCE_DATE_EPOCH.

* Missing checkdepends on pytest-runner, which is automatically downloaded by the build tools but that failed in the reproducible build.
Some simply adding the depdency to checkdepends [fixed
it](https://git.archlinux.org/svntogit/community.git/commit/?id=21de890413b72c914db993232e0ef604a4248524).

As you can see, only one of the bullet points was really an reproducible build issue the others where packaging issues. So I can conclude
that reproducible builds will increase the packaging quality in the Arch repository. Having the packages in our repository always build-able
will also help the Arch Linux 32 project.

The Arch reproducible project still needs a lot of work, to make it possible to verify a package build as a user against the repository
package.

P.S.: If you are at 34C3 this year and interested, visit the reproducible build assembly.
