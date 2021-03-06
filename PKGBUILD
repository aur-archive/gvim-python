# $Id$
# Contributor: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Maintainer: tobias [ tobias at archlinux org ]
# Maintainer: Daniel J Griffiths <ghost1227@archlinux.us>

pkgname=gvim-python
pkgdesc='Vi Improved, a highly configurable, improved version of the vi text editor (with advanced features, such as a GUI) (Compiled with Python 3).'
_topver=7.4
_patchlevel=274
_versiondir="vim${_topver//./}"
pkgver=${_topver}.${_patchlevel}
pkgrel=1
_pkgrel=1  # upstream version
arch=('i686' 'x86_64')
license=('custom:vim')
url="http://www.vim.org"
depends=("vim-runtime=${pkgver}-${_pkgrel}" 'gpm' 'ruby' 'libxt'
         'desktop-file-utils' 'gtk2' 'lua' 'python')
provides=("vim=${pkgver}-${_pkgrel}" "gvim=${pkgver}-${_pkgrel}")
conflicts=('vim' 'gvim')
install=gvim.install
source=("ftp://ftp.archlinux.org/other/vim/vim-${pkgver}.tar.xz"
        "ftp://ftp.archlinux.org/other/vim/vim-${pkgver}.tar.xz.sig"
        'vimrc'
        'archlinux.vim'
        'gvim.desktop')

md5sums=('98bf9f8d57b95715d08fcc42beae8761'
         'SKIP'
         'b9d4dcb9d3ee2e151dc4be1e94934f6a'
         '10353a61aadc3f276692d0e17db1478e'
         'd90413bd21f400313a785bb4010120cd')

build() {
  cp -a "${srcdir}/vim-${pkgver}" gvim-build
  cd "${srcdir}"/gvim-build

  ./configure \
    --prefix=/usr \
    --localstatedir=/var/lib/vim \
    --with-features=huge \
    --with-compiledby='Arch Linux' \
    --enable-gpm \
    --enable-acl \
    --with-x=yes \
    --enable-gui=gtk2 \
    --enable-multibyte \
    --enable-cscope \
    --enable-netbeans \
    --enable-perlinterp \
    --disable-pythoninterp \
    --enable-python3interp \
    --enable-rubyinterp \
    --enable-luainterp

  make
}

check() {
  # disable tests because they seem to freeze

  cd "${srcdir}"/gvim-build

  #make test
}

package() {

  cd "${srcdir}"/gvim-build
  make -j1 VIMRCLOC=/etc DESTDIR="${pkgdir}" install

  # provided by (n)vi in core
  rm "${pkgdir}"/usr/bin/{ex,view}

  # delete some manpages
  find "${pkgdir}"/usr/share/man -type d -name 'man1' 2>/dev/null | \
    while read _mandir; do
    cd ${_mandir}
    rm -f ex.1 view.1 # provided by (n)vi
  done

  # remove run-time
  rm -r "${pkgdir}"/usr/share/vim
 
  # freedesktop links
  install -Dm644 "${srcdir}"/gvim.desktop \
    "${pkgdir}"/usr/share/applications/gvim.desktop
  install -Dm644 runtime/vim48x48.png "${pkgdir}"/usr/share/pixmaps/gvim.png

  # license
   install -Dm644 "${srcdir}"/vim-${pkgver}/runtime/doc/uganda.txt \
     "${pkgdir}"/usr/share/licenses/${pkgname}/license.txt
}

# vim:set sw=2 sts=2 et:
