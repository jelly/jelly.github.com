---
layout: post
title: "Makepkg-template"
date: 2013-11-11 19:15
comments: false
categories: archlinux pacman
---

Since around March pacman's git repo has a new tool called makepkg-template created by Florian Pritz. Below is a description from man makepkg-template:  
*makepkg-template is a script to ease the work of maintaining multiple similar PKGBUILDs.
It allows you to move most of the code from the PKGBUILD into a template file and uses markers to allow in-place updating of existing PKGBUILDs if the template has been changed.*  
With makepkg-template you can define a template file, which can replace parts of a PKGBUILD and keep them consistent.  

{% highlight bash %}

build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    runhaskell Setup configure -O --enable-split-objs --enable-shared \
       --prefix=/usr --docdir=/usr/share/doc/${pkgname} --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register   --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}
package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 LICENSE ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/LICENSE
}
{% endhighlight %}

Now you can use this .template file by saving it as "$template_name-$version.template", then you create a symlink to "$template_name.template" 
{% highlight bash %}
ln -s "$template_name-$version.template" "$template_name.template"
{% endhighlight %}

After saving the template and symlinking, you edit the PKGBUILD to use the template.
{% highlight bash %}
# Maintainer: Arch Haskell Team <arch-haskell@haskell.org>
_hkgname=mime
pkgname=haskell-mime
pkgver=0.3.4
pkgrel=1
pkgdesc="Working with MIME types."
url="http://hackage.haskell.org/package/${_hkgname}"
license=('custom:BSD3')
arch=('i686' 'x86_64')
makedepends=()
depends=('ghc')
options=('staticlibs')
install=${pkgname}.install
source=(http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz)
md5sums=('77e4fcae38b52b694c00eeafb4c70a1e')

# template start; name=haskell-module; version=1.0

# template end;
{% endhighlight %}

Now we can run makepkg-template on the PKGBUILD and it will replace the contents between the template's start and end definition with the template file's contents. 
{% highlight bash %}
makepkg-template --template-dir ~/Projects/makepkg-templates -p PKGBUILD
{% endhighlight %}

With templates for Perl, Python and other packages we can make sure they all use the same build() and package() functions. The creation of new packages should be easier with these templates too. Currently makepkg-template is only available in [pacman-git](https://aur.archlinux.org/packages/pacman-git/).
