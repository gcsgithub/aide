# Maintainer: manjaro <manjaro-dev@garetech.com.au>
pkgname=aide
pkgver=0.16.2
pkgrel=54
epoch=
pkgdesc="aide - Advanced Intrusion Detection Environment"
arch=(aarch64 x86_64)
url="https://github.com/gcsgithub/aide.git"
license=('GPL-2.0')
groups=()
depends=(git pcre pcre2)
makedepends=()
checkdepends=()
optdepends=()
provides=("${pkgname}-${pkgver}")
conflicts=("${pkgname}")
replaces=()
backup=("etc/aide.conf")
options=()
install=
changelog=
branch="feature/soedev-manjaro-arm"
source=(git+${url}#branch=${branch})
noextract=()
md5sums=("SKIP")
validpgpkeys=()

pkgver() {
        pkgver="$( cd "${srcdir}/${pkgname}" && git describe  | cut -d\- -f1)"
        [ "${pkgver:0:1}" = "v" ]  && pkgver="${pkgver:1}"
        echo "${pkgver}"
}

prepare() {
        cd "${srcdir}/${pkgname}"
        bash autogen.sh
}

build() {
        cd "${srcdir}/${pkgname}"
        PREFIX=""
        EPREFIX="${PREFIX}/usr"
        LOCALSTATEDIR=""
        DATAROOTDIR=${EPREFIX}/share

        ./configure                                      \
                     --prefix=/                          \
                     --exec-prefix=/usr                  \
                     --bindir=${EPREFIX}/bin             \
                     --sbindir=${EPREFIX}/sbin           \
                     --libexecdir=${EPREFIX}/libexec     \
                     --sysconfdir=${PREFIX}/etc          \
                     --sharedstatedir=${PREFIX}/com      \
                     --localstatedir=${PREFIX}/var       \
                     --runstatedir=${LOCALSTATEDIR}/run  \
                     --libdir=${EPREFIX}/lib             \
                     --includedir=${EPREFIX}/include     \
                     --oldincludedir=/usr/include        \
                     --datarootdir=${EPREFIX}/share      \
                     --datadir=${DATAROOTDIR}            \
                     --infodir=${DATAROOTDIR}/info       \
                     --localedir=${DATAROOTDIR}/locale   \
                     --mandir=${DATAROOTDIR}/man         \
                     --docdir=${DATAROOTDIR}/doc/aide    \
                     \
                     --disable-static                    \
                     \
                     --with-config-file="/etc/aide.conf" \
                     --with-mmap                         \
                     --with-posix-acl                    \
                     --with-prelink                      \
                     --with-xattr                        \
                     --with-e2fsattrs                    \
                     --with-zlib                         \
                     --with-gcrypt                       \
                     --with-audit                        \
                     \
                     --with-locale                       \
                     --with-curl                         \
                     --without-mhash
        make
}

check() {
        cd "${srcdir}/${pkgname}"
        make -k check
}

package() {
        cd "${srcdir}/${pkgname}"
        make DESTDIR="$pkgdir/" install
        mkdir           "${pkgdir}/etc"
        chown root:root "${pkgdir}/etc"
        chmod 755       "${pkgdir}/etc"
        /usr/bin/install -c -m 644 "./contrib/aide.conf" "${pkgdir}/etc/aide.conf"
}
