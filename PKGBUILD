# Maintainer: Det <nimetonmaili g-mail>
# Contributors: t3ddy, Lex Rivera aka x-demon, ruario

# Check for new Linux releases in: http://googlechromereleases.blogspot.com/search/label/Dev%20updates
# or use: $ curl -s https://dl.google.com/linux/chrome/rpm/stable/x86_64/repodata/other.xml.gz | gzip -df | awk -F\" '/pkgid/{ sub(".*-","",$4); print $4": "$10 }'

pkgname=google-chrome-dev
pkgver=73.0.3683.10
pkgrel=1
pkgdesc="The popular and trusted web browser by Google (Dev Channel)"
arch=('x86_64')
url="https://www.google.com/chrome"
license=('custom:chrome')
depends=('alsa-lib' 'gconf' 'gtk3' 'libcups' 'libxss' 'libxtst' 'nss')
optdepends=('kdialog: for file dialogs in KDE'
            'gnome-keyring: for storing passwords in GNOME keyring'
            'kwallet: for storing passwords in KWallet'
            'gtk3-print-backends: for printing'
            'libunity: for download progress on KDE'
            'ttf-liberation: fix fonts for some PDFs (CRBug #369991)'
            'xdg-utils')
provides=('google-chrome')
options=('!emptydirs' '!strip')
_channel=unstable
source=(
	"google-chrome-${_channel}_${pkgver}_amd64.deb::http://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-${_channel}/google-chrome-${_channel}_${pkgver}-1_amd64.deb"
	'eula_text.html'
)
sha256sums=('b19e167522597fad571750a3c956a5bfafb0864f5e3f6c7c25fdee9d3f8b1208'
            'af48d6467196286e5450f52fd4fd819f9f5c631b322eeac3e23944403d06fcff')
package() {
  bsdtar -xf data.tar.xz -C "$pkgdir/"

  # Icons
  for i in 16x16 22x22 24x24 32x32 48x48 64x64 128x128 256x256; do
      install -Dm644 "$pkgdir"/opt/google/chrome-$_channel/product_logo_${i/x*}_${pkgname/*-}.png \
          "$pkgdir"/usr/share/icons/hicolor/$i/apps/google-chrome-$_channel.png
  done

  # Man page
  if [[ -f "$pkgdir"/usr/share/man/man1/google-chrome-$_channel.1 ]]; then
      gzip "$pkgdir"/usr/share/man/man1/google-chrome-$_channel.1
  fi

  # License
  install -Dm644 eula_text.html "$pkgdir"/usr/share/licenses/google-chrome-$_channel/eula_text.html

  sed -i "/Exec=/i\StartupWMClass=Google-chrome-$_channel" "$pkgdir"/usr/share/applications/google-chrome-$_channel.desktop

  rm -r "$pkgdir"/etc/cron.daily/ "$pkgdir"/opt/google/chrome-$_channel/cron/
  rm "$pkgdir"/opt/google/chrome-$_channel/product_logo_*.png
}

