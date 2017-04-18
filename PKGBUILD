# Maintainer: Det <nimetonmaili at gmail a-dot com>
# Contributors: t3ddy, Lex Rivera aka x-demon, ruario

# Check for new Linux releases in: http://googlechromereleases.blogspot.com/search/label/Dev%20updates
# or use: $ curl -s https://dl.google.com/linux/chrome/rpm/stable/x86_64/repodata/other.xml.gz | gzip -df | awk -F\" '/pkgid/{ sub(".*-","",$4); print $4": "$10 }'

pkgname=google-chrome-dev
pkgver=59.0.3071.9
pkgrel=1
pkgdesc="An attempt at creating a safer, faster, and more stable browser (Dev Channel)"
arch=('x86_64')
url="https://www.google.com/chrome/index.html"
license=('custom:chrome')
depends=('alsa-lib' 'desktop-file-utils' 'flac' 'gconf' 'gtk2' 'harfbuzz' 'harfbuzz-icu' 'hicolor-icon-theme'
         'icu' 'libpng' 'libxss' 'libxtst' 'nss' 'opus' 'snappy' 'speech-dispatcher' 'ttf-font' 'xdg-utils'
         'shared-mime-info')
optdepends=('kdebase-kdialog: needed for file dialogs in KDE'
            'ttf-liberation: fix fonts for some PDFs')
provides=("google-chrome=$pkgver")
options=('!emptydirs' '!strip')
_channel=unstable
_arch=amd64
source=(
	"google-chrome-${_channel}_${pkgver}_amd64.deb::http://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-${_channel}/google-chrome-${_channel}_${pkgver}-1_amd64.deb"
	'eula_text.html'
)
sha256sums=('535cdaab79a5265d446c6de7df73fd2750e07abde8340bffa834ec3c294ee225'
            'af48d6467196286e5450f52fd4fd819f9f5c631b322eeac3e23944403d06fcff')
noextract=(
	"google-chrome-${_channel}_${pkgver}_amd64.deb"
)

prepare() {
	ar x "google-chrome-${_channel}_${pkgver}_${_arch}.deb"
}

package() {
  bsdtar -xf data.tar.xz -C "$pkgdir/"

  # Icons
  for i in 16 22 24 32 48 64 128 256; do
    install -Dm644 "$pkgdir"/opt/google/chrome-$_channel/product_logo_$i.png \
                   "$pkgdir"/usr/share/icons/hicolor/${i}x$i/apps/google-chrome-$_channel.png
  done

  # Man page
  gzip "$pkgdir"/usr/share/man/man1/google-chrome-$_channel.1

  # License
  install -Dm644 eula_text.html "$pkgdir"/usr/share/licenses/google-chrome-$_channel/eula_text.html

	# fix chrome to use libudev.so.1
	sed -e 's/libudev.so.0/libudev.so.1/g' \
		-i "$pkgdir/opt/google/chrome-$_channel/google-chrome-$_channel"

  _name=$(echo ${source/_*} | sed 's/.*/\u&/')
  sed -i "/Exec=/i\StartupWMClass=$_name" "$pkgdir"/usr/share/applications/google-chrome-$_channel.desktop

  sed -i 's/ "$@"/"$CHROMIUM_USER_FLAGS" "$@"/' "$pkgdir"/opt/google/chrome-$_channel/google-chrome-$_channel

  ln -sf "/opt/google/chrome-$_channel/google-chrome-$_channel" \
      "$pkgdir/usr/bin/google-chrome-$_channel"

  rm -r "$pkgdir"/etc/cron.daily/ "$pkgdir"/opt/google/chrome-$_channel/cron/
  rm "$pkgdir"/opt/google/chrome-$_channel/product_logo_*.png
}

