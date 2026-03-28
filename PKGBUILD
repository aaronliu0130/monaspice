# Maintainer: aliu <aaronliu0130@gm il.com>
pkgname='otf-monaspice-huhanme-nerd-font'
pkgver=1.400
pkgrel=1
pkgdesc="GitHub's Monaspace with different subfamilies for the normal, italic, and bold (& bold italic) variants"
arch=('any')
url='https://github.com/aaronliu0130/monaspice'
license=('OFL-1.1-RFN')
provides=('ttf-font-nerd')
makedepends=('python-fonttools')  # sed is in base
source=("https://github.com/githubnext/monaspace/releases/download/v$pkgver/monaspace-nerdfonts-v${pkgver}.zip"
	"https://github.com/githubnext/monaspace/raw/refs/tags/v$pkgver/LICENSE"
	'Monaspice HuHanMe NF generic.ttx')
sha256sums=('9b7f9505d977601d8819dfdb57fa4b49bdab61132b6d1131c2e8d092066edc5a'
	'0e84e5f7dd6f05e74a00f2fb828ca43e489d954f5509ff0fa439ea18c0d35fe9'
	'8b64a7f467709300864cb4a14d15a9a8864bcfd067e8021d4f5ae57324a21a20')

_patch() {
	local name="$1";local weight="$2";local pref_weight="$3";local style="$4"

	local style_replacement='s/$s/'"$pref_weight"'/'
	local filename_weight="${pref_weight}"
	if [[ -z $style ]]; then
		style_replacement='s/ $s/ /'
	elif [[ $pref_weight == 'Regular' ]]; then
		filename_weight=''
	fi
	sed "${srcdir}/hhm_ver.ttx" \
		-e 's/$w/'"$weight"'/' \
		-e 's/$p/'"$pref_weight"'/' \
		-e "$style_replacement" \
		> "${srcdir}/hhm_patch.ttx"
	ttx "${srcdir}/hhm_patch.ttx" -m "${srcdir}/NerdFonts/Monaspace ${name}/Monaspace${name}NF-${filename_weight}${style}.otf" \
		-o "${srcdir}/MonaspiceHuHanMeNF-${filename_weight}${style}.otf"
}

prepare() {
	sed "s/\$v/${pkgver}/g" 'Monaspice HuHanMe NF generic.ttx' > "hhm_ver.ttx"
}

build() {
	_patch 'Argon' 'Regular' 'Light' ''
	_patch 'Argon' 'Regular' 'Regular' ''

	_patch 'Radon' 'Regular' 'Light' 'Italic'
	_patch 'Radon' 'Regular' 'Regular' 'Italic'

	_patch 'Krypton' 'Bold' 'SemiBold' ''
	_patch 'Krypton' 'Bold' 'Bold' ''
	_patch 'Krypton' 'Bold' 'SemiBold' 'Italic'
	_patch 'Krypton' 'Bold' 'Bold' 'Italic'

	rm "${srcdir}/hhm_"{ver,patch}.ttx
}

package() {
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/monaspice"
	install -Dm644 MonaspiceHuHanMeNF* -t "${pkgdir}/usr/share/fonts/OTF"
}
