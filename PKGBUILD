# Maintainer: Baptiste Jonglez <baptiste--aur at jonglez dot org>
# Heavily inspired on Fedora package, see http://git.annexia.org/?p=fedora-mingw.git;a=tree;f=flexdll;hb=HEAD
# Note : this is an old version, but it works very well for its purpose (that is, allow building an OCaml cross-compilation chain, see mingw32-ocaml)
pkgname=mingw32-flexdll
pkgver=0.11
pkgrel=3
pkgdesc="FlexDLL Windows DLL plugin API"
arch=('i686' 'x86_64')
url="http://alain.frisch.fr/flexdll.html"
license=('zlib' 'libpng')
depends=()
makedepends=('ocaml' 'mingw32-binutils' 'mingw32-gcc')
options=('!strip')
source=( "http://alain.frisch.fr/flexdll/flexdll-${pkgver}.tar.gz" 
    "mingw32-flexdll-0.11-mingw-cross.patch.templ"
    "mingw32-flexdll-0.11-no-cygpath.patch.templ"   
    "mingw32-flexdll-0.11-no-directory.patch.templ"
    "mingw32-flexdll-0.11-real-objdump.patch.templ" )
md5sums=('1b49982b0e26530c7c0961481599dd71'
         '2a1e1a2bc65989e3c497633991989317'
         'ca59ba7ea176a940041c9f6a421a9d84'
         '4d5c2af6e879365b059e8a25d219290f'
         'e27bcb16cc04f0b7fbcb4329e525f53f')


build() {
  cd "$srcdir"
  
  _pkgname=flexdll  

  patch1="mingw32-flexdll-0.11-mingw-cross.patch"
  patch2="mingw32-flexdll-0.11-no-cygpath.patch"
  patch3="mingw32-flexdll-0.11-no-directory.patch"
  patch4="mingw32-flexdll-0.11-real-objdump.patch"

  mingw32prefix=i486-mingw32
  for patch in "$patch1" "$patch2" "$patch3" "$patch4"
  do
      sed -e "s#%%mingw32%%#$mingw32prefix#g" $patch.templ > "$_pkgname/$patch"
      rm -f $patch.templ
  done

  cd "$_pkgname"

  for patch in "$patch1" "$patch2" "$patch3" "$patch4"
  do
      patch -p1 < "$patch"
  done

  make TOOLCHAIN=mingw  MINCC="${mingw32prefix}-gcc" CC="${mingw32prefix}-gcc" flexlink.exe build_mingw
}

package() {
  _pkgname=flexdll  
  cd "$srcdir/$_pkgname"

  mkdir -p $pkgdir/usr/lib/flexdll

  install -m 0644 flexdll.h flexdll.c flexdll_initer.c default.manifest flexdll_*.o \
      $pkgdir/usr/lib/flexdll
  install -m 0755 flexlink.exe $pkgdir/usr/lib/flexdll

  mkdir -p $pkgdir/usr/bin

  echo '#!/bin/sh' > $pkgdir/usr/bin/flexlink.exe
  echo 'FLEXDIR=/usr/lib/flexdll /usr/lib/flexdll/flexlink.exe "$@"' >> $pkgdir/usr/bin/flexlink.exe

  chmod 0755 $pkgdir/usr/bin/flexlink.exe
  (cd $pkgdir/usr/bin && ln flexlink.exe flexlink)

}

# vim:set ts=2 sw=2 et:
