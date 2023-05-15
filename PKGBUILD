# Maintainer: Chris Mustola <chris.mustola@gmail.com>

_vendor=oneplus
_codename=oneplus3
pkgname=$_vendor-$_codename-adaptation
provides=(halium9-post-install)
conflicts=(halium9-post-install)
pkgver=1$(date +%y%m%d)
pkgrel=1
pkgdesc="Manjaro libhybris adaptation for OnePlus 3/3T"
arch=('any')
url="https://github.com/Bettehem/$pkgname"
license=('GPL')
depends=('gzip' 'glibc-locales' 'wget' 'systemd')
makedepends=('git')
source=("$pkgname::git+https://github.com/Bettehem/$pkgname.git")
install=$pkgname.install
md5sums=('SKIP')

package() {
    mv "${srcdir}/$pkgname/$pkgname.sh" "${srcdir}/$pkgname/$pkgname"

    mkdir -p "${pkgdir}/usr/bin/"
    install -Dm755 "${srcdir}/$pkgname/$pkgname" -t "${pkgdir}/usr/bin/"

    mkdir -p "${pkgdir}/usr/lib/systemd/system/"
    install -Dm644 "${srcdir}/$pkgname/$pkgname.service" -t "${pkgdir}/usr/lib/systemd/system/"
    install -Dm644 "${srcdir}/$pkgname/oneplus3-dashd.service" -t "${pkgdir}/usr/lib/systemd/system/"
    install -Dm644 "${srcdir}/$pkgname/oneplus3-wifi-fix.service" -t "${pkgdir}/usr/lib/systemd/system/"

    cp -r "${srcdir}/$pkgname/droid-vendor-overlay" "${pkgdir}/usr/lib/droid-vendor-overlay"

    mkdir -p "${pkgdir}/usr/lib/sysusers.d/"
    install -Dm644 "${srcdir}/$pkgname/android.conf" -t "${pkgdir}/usr/lib/sysusers.d/"

    mkdir -p ${pkgdir}/etc/phosh/
    install -Dm644 "${srcdir}/$pkgname/phoc.ini" -t "${pkgdir}/etc/phosh/"

    mkdir -p "${pkgdir}/etc/udev/rules.d/"
    # Create rules file for the device
    cat /var/lib/lxc/android/rootfs/ueventd*.rc /vendor/ueventd*.rc | grep ^/dev | sed -e 's/^\/dev\///' | awk '{printf "ACTION==\"add\", KERNEL==\"%s\", OWNER=\"%s\", GROUP=\"%s\", MODE=\"%s\"\n",$1,$3,$4,$2}' | sed -e 's/\r//' > ${srcdir}/$pkgname/70-$_codename.rules
    install -Dm644 "${srcdir}/$pkgname/70-oneplus3.rules" -t "${pkgdir}/etc/udev/rules.d/"
    install -Dm644 "${srcdir}/$pkgname/90-backlight.rules" -t "${pkgdir}/etc/udev/rules.d/"

    mkdir -p "${pkgdir}/etc/systemd/system/bluebinder.service.d"
    install -Dm644 "${srcdir}/$pkgname/override.conf" -t "${pkgdir}/etc/systemd/system/bluebinder.service.d/"

    mkdir -p "${pkgdir}/boot/"
    install -Dm644 "${srcdir}/$pkgname/halium-boot.img" -T "${pkgdir}/boot/boot.img"

    mkdir -p "${pkgdir}/usr/share/glib-2.0/schemas/"
    install -Dm644 "${srcdir}/$pkgname/90_manjaro.gschema.override" -t "${pkgdir}/usr/share/glib-2.0/schemas/"

    mkdir -p "${pkgdir}/usr/lib/oneplus3-adaptation/vendor-sbin"
    install -Dm755 "${srcdir}/$pkgname/dashd" -t "${pkgdir}/usr/lib/oneplus3-adaptation/vendor-sbin/"
}
