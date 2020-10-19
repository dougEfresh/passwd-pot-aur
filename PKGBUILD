# Maintainer: "Douglas Chimento" dougEfresh

pkgname=passwdpot-git
pkgver=r235.0b7e9c5
pkgrel=1
pkgdesc="${pkgname}"
arch=('any')
url="https://github.com/dougEfresh/passwd-pot-aur"
license=('custom')
options=('!strip')
makedepends=('json-c' 'go' 'fakeroot' 'make' 'gcc')
depend=('openssh')
source=( "git://github.com/dougEfresh/passwd-pot.git" "git://github.com/dougEfresh/sshd-passwd-pot.git" sshd-passwd-pot.service sshd.service passwd-pot-proxy.service passwd-pot.service )
sha256sums=('SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP')


pkgver() {
  cd "passwd-pot"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
 cd ${srcdir}/sshd-passwd-pot
 autoreconf
}

build() {
    cd ${srcdir}/passwd-pot
    go build -o passwd-pot
cd ${srcdir}/sshd-passwd-pot
  ./configure --disable-suid-ssh \
  --with-ssl-engine \
 --disable-lastlog \
 --disable-wtmp \
 --without-rsh \
 --with-privsep-user=nobody \
 --prefix=/opt/sshd-passwd-pot \
 --with-audit-passwd-url=yes
make
}
 
package() {
  cd "${srcdir}/passwd-pot"
  
  install -D -m755 passwd-pot "$pkgdir/usr/bin/passwd-pot"
  cd "${srcdir}/sshd-passwd-pot"
  make DESTDIR="${pkgdir}" install

  cd "$srcdir"
  install -D -m644 sshd.service "$pkgdir/etc/systemd/system/sshd.service"
  install -D -m644 sshd-passwd-pot.service "$pkgdir/etc/systemd/system/sshd-passwd-pot.service"

  install -D -m644 passwd-pot.service "$pkgdir/etc/systemd/system/passwd-pot.service"
  install -D -m644 passwd-pot-proxy.service "$pkgdir/etc/systemd/system/passwd-pot-proxy.service"

}
