# Maintainer: Stella Benware (enterthevoidx86@gmail.com)

_pkgname="calamares"
pkgname="$_pkgname-odysen"
pkgver=3.3.14.r16.g296be44
pkgrel=1
pkgdesc="Distribution-independent installer framework"
url="https://github.com/calamares/calamares"
license=("GPL-3.0-or-later")
arch=('i686' 'x86_64')

depends=(
  'boost-libs'
  'ckbcomp' # AUR
  'hwinfo'
  'kconfig'
  'kcoreaddons'
  'ki18n'
  'kiconthemes'
  'kpmcore'
  'libpwquality'
  'polkit-qt6'
  'python'
  'python-jsonschema'
  'python-yaml'
  'qt6-svg'
  'solid'
  'squashfs-tools'
  'yaml-cpp'
)
makedepends=(
  'boost'
  'extra-cmake-modules'
  'git'
  'ninja'
  'qt6-tools'
  'qt6-translations'
)

provides=("$_pkgname=${pkgver%%.r*}")
conflicts=("$_pkgname")

backup=(
  #'usr/share/calamares/modules/bootloader.conf'
  #'usr/share/calamares/modules/displaymanager.conf'
  #'usr/share/calamares/modules/initcpio.conf'
  #'usr/share/calamares/modules/unpackfs.conf'
)

_pkgsrc="$_pkgname"
source=(
  "$_pkgsrc"::"git+$url.git"
  "yay-support.patch"
)
sha256sums=(
  'SKIP'
  'SKIP'
)

pkgver() {
  cd "$_pkgsrc"
  git describe --long --tags --abbrev=7 --exclude='*[a-zA-Z][a-zA-Z]*' \
    | sed -E 's/^[^0-9]*//;s/([^-]*-g)/r\1/;s/-/./g'
}

prepare() {
  cd "$_pkgsrc"
  git apply ../yay-support.patch
}

build() {
  local _skip_modules=(
    dracut
    dracutlukscfg
    dummycpp
    dummyprocess
    dummypython
    dummypythonqt
    initramfs
    initramfscfg
    interactiveterminal
    packagechooser
    packagechooserq
    services-openrc
  )

  local _cmake_options=(
    -B build
    -S "$_pkgsrc"
    -G Ninja
    -DCMAKE_BUILD_TYPE=Release
    -DCMAKE_INSTALL_PREFIX='/usr'
    -DCMAKE_INSTALL_LIBDIR='lib'
    -DWITH_QT6=ON
    -DINSTALL_CONFIG=ON
    -DSKIP_MODULES="${_skip_modules[*]}"
    -DBUILD_TESTING=OFF
    -Wno-dev
  )

  cmake "${_cmake_options[@]}"
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build
  install -Dm644 "${srcdir}/../calamares.desktop" "$pkgdir/usr/share/applications/calamares.desktop"
  install -Dm644 "${srcdir}/../com.github.calamares.calamares.policy" "$pkgdir/usr/share/polkit-1/actions/com.github.calamares.calamares.policy"
}
