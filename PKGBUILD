# Maintainer: Jan Houben <jan@nexttrex.de>
# Contributor: Jesus Alvarez <jeezusjr at gmail dot com>
#
# This PKGBUILD was generated by the archzfs build scripts located at
#
# http://github.com/archzfs/archzfs
#
pkgname="zfs-utils"

pkgver=2.0.4
pkgrel=1
pkgdesc="Kernel module support files for the Zettabyte File System."
makedepends=("python" "python-setuptools" "python-cffi")
optdepends=("python: pyzfs and extra utilities", "python-cffi: pyzfs")
arch=("x86_64")
url="http://zfsonlinux.org/"
source=("https://github.com/zfsonlinux/zfs/releases/download/zfs-${pkgver}/zfs-${pkgver}.tar.gz"
        "zfs-utils.initcpio.install"
        "zfs-utils.initcpio.hook"
        "zfs-utils.initcpio.zfsencryptssh.install")
sha256sums=("7d1344c5433b91823f02c2e40b33d181fa6faf286bea5591f4b1965f23d45f6c"
            "29a8a6d76fff01b71ef1990526785405d9c9410bdea417b08b56107210d00b10"
            "449a6db4abd3f166562bb67a63950af053e9ec07eabbfcdff827c5ed0113a2d6"
            "29080a84e5d7e36e63c4412b98646043724621245b36e5288f5fed6914da5b68")
license=("CDDL")
groups=("archzfs-linux")
provides=("zfs-utils" "spl-utils")
install=zfs-utils.install
conflicts=("zfs-utils" "spl-utils")
replaces=("zfs-utils-linux" "zfs-utils-linux-lts" "zfs-utils-common")
backup=('etc/zfs/zed.d/zed.rc' 'etc/default/zfs' 'etc/modules-load.d/zfs.conf' 'etc/sudoers.d/zfs')

build() {
    cd "${srcdir}/zfs-${pkgver}"
    ./autogen.sh
    ./configure --prefix=/usr --sysconfdir=/etc --sbindir=/usr/bin --with-mounthelperdir=/usr/bin \
                --libdir=/usr/lib --datadir=/usr/share --includedir=/usr/include \
                --with-udevdir=/usr/lib/udev --libexecdir=/usr/lib \
                --with-config=user --enable-systemd --enable-pyzfs
    make
}

package() {
    cd "${srcdir}/zfs-${pkgver}"
    make DESTDIR="${pkgdir}" install
    # Remove uneeded files
    rm -r "${pkgdir}"/etc/init.d
    rm -r "${pkgdir}"/usr/share/initramfs-tools
    rm -r "${pkgdir}"/usr/lib/modules-load.d
    # Autoload the zfs module at boot
    mkdir -p "${pkgdir}/etc/modules-load.d"
    printf "%s\n" "zfs" > "${pkgdir}/etc/modules-load.d/zfs.conf"
    # fix permissions
    chmod 750 ${pkgdir}/etc/sudoers.d
    chmod 440 ${pkgdir}/etc/sudoers.d/zfs
    # Install the support files
    install -D -m644 "${srcdir}"/zfs-utils.initcpio.hook "${pkgdir}"/usr/lib/initcpio/hooks/zfs
    install -D -m644 "${srcdir}"/zfs-utils.initcpio.install "${pkgdir}"/usr/lib/initcpio/install/zfs
    install -D -m644 "${srcdir}"/zfs-utils.initcpio.zfsencryptssh.install "${pkgdir}"/usr/lib/initcpio/install/zfsencryptssh
    install -D -m644 contrib/bash_completion.d/zfs "${pkgdir}"/usr/share/bash-completion/completions/zfs
}
