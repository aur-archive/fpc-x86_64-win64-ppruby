pkgname=fpc-x86_64-win64-ppruby
pkgver=20130311
pkgrel=1
pkgdesc="Ruby binding units for FreePascal (x86_64-win64)"
url="https://github.com/shikhalev/ppruby"
license=("GPLv3")
arch=(any)
depends=(fpc-x86_64-win64-rtl)
makedepends=(mingw-w64-binutils git)
options=(!strip)
_gitroot="https://github.com/shikhalev/ppruby.git"
_gitname=ppruby
_unittgt=x86_64-win64
_fpcver=`fpc -iV`

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]
  then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."
  rm -rf "$srcdir/$_gitname-build"
  cp -r "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build/static"
  fpc -Twin64 ruby18.pp
}

package() {
  cd "$srcdir/$_gitname-build/static"
  find . -name '*.o' -o -name '*.ppu' -o -name '*.rst' -o -name '*.a' |
    xargs -rtl1 -I {} install -Dm644 {} "$pkgdir/usr/lib/fpc/$_fpcver/units/$_unittgt/ppruby/"{}
  find $pkgdir -name '*.o' -o -name '*.a' | xargs -rtl1 x86_64-w64-mingw32-strip -g
}
