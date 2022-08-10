# Maintainer: Justin Jagieniak <justin@jagieniak.net>
# Contributor: Nicky D

pkgname=firestorm-vr
_openvr_ver=1.6.10b
pkgver=6.5.6
_pkgver=-vr-alpha-0
pkgrel=1
pkgdesc="This is the Firestorm Viewer! - VR Mod"
arch=('i686' 'x86_64')

case "$CARCH" in
	i686)    _arch=linux32;;
	x86_64)  _arch=linux64;;
esac

url=https://www.firestormviewer.org
license=('LGPL' 'custom')
depends=(dbus-glib gconf glu gtk2 lib32-gcc-libs lib32-libidn lib32-libsndfile lib32-util-linux lib32-zlib libgl libidn libjpeg-turbo libpng libxss libxml2 mesa nss openal sdl vlc zlib libxcrypt-compat)
optdepends=(
  'alsa-lib: for ALSA support'
  'pepper-flash: for inworld Flash support'
  'freealut: for OpenAL support'
  'lib32-libidn11: for voice support'
  'libpulse: for PulseAudio support'
  'mesa-libgl: For Intel, Radeon, Nouveau support'
  'nvidia-libgl: for NVIDIA support'
  'nvidia-utils: for NVIDIA support')
makedepends=('cmake' 'gcc' 'make' 'python-virtualenv' 'python-pip' 'git' 'boost' 'xz' 'jq')
options=('debug' '!strip')
# conflicts=('firestorm-bin' 'firestorm-nightly' 'firestorm-beta-bin')
# provides=('firestorm')

[[ " ${options[*]} " =~ "debug" ]] && _buildtype='RelWithDebInfoFS_open' || _buildtype='ReleaseFS_open'

source=("$pkgname"::"git+https://vcs.firestormviewer.org/phoenix-firestorm#branch=Firestorm_$pkgver"
		"https://github.com/humbletim/firestorm-gha/raw/v${pkgver}${_pkgver}/indra/newview/llviewerdisplay.cpp"
		"https://github.com/humbletim/firestorm-gha/raw/v${pkgver}${_pkgver}/indra/newview/llviewerVR.cpp"
		"https://github.com/humbletim/firestorm-gha/raw/v${pkgver}${_pkgver}/indra/newview/llviewerVR.h"
		"https://github.com/humbletim/firestorm-gha/raw/v${pkgver}${_pkgver}/.github/3p/_autobuild-json2xml.jq"
		"https://github.com/humbletim/firestorm-gha/raw/v${pkgver}${_pkgver}/.github/3p/build-cmds/3p-openvr.autobuild.json"
		"https://github.com/humbletim/firestorm-gha/raw/v${pkgver}${_pkgver}/.github/3p/build-cmds/3p-openvr.build-cmd.sh"
		"fs-build-variables"::'git+https://vcs.firestormviewer.org/fs-build-variables' 
		"3p-openvr"::"git+https://github.com/ValveSoftware/openvr.git#tag=v${_openvr_ver}" 
		'openvr.patch' 
		'firestorm-vr.patch' 
		'firestorm-vr.desktop' 
		'firestorm-vr.launcher')
sha512sums=('SKIP'
            '39522b11525a6e727ff0067645dae74045a709a0700af1a6796a40a4239df7f00859b7649ef70ef0249610664462da2923c390fa79b2aca6d056f4911a74a48f'
            '6d280781597c0fc84866e73fb7c225f291632a6d23ea414b6006eff33474ca9050292582b5c6d77d1d5d6c2f6b0bca72f3ad3cf0fa806db3d0573234631fe4e7'
            '6a9a2051a44968c98793991e5c0ef1cd27fe561942af4bdf761e4a74b44c4cded360ff31e7c5bd5d0f41794c3ad5aa2a4190a0fd0a6d785a8577c7bb5ec6daaf'
            'cbad481d81caa197f1457ce96d7f4eddb5988eca3db0cffbea783b31a5eed0b61941b2fb0a9c2d0f417a3372306328171c0e8713757cf5528cdccd7ef3d01bbe'
            '9f1e28fe953b6bb33a13c0e06bd4860d37bc11fef76791b432b56174bc5e5586074acaedd36bb6199757c8fc0207ca71f81aa75e2bc8b25bfa760afcee53f67d'
            '7e2ac3e2795dd574bfa3776c018ae2c48b4e4168dbea906fc482c4c1d3eb01bf2cbdfb097b004d8a27b260463d1456f08188891c58a8c6f9b9e62dea3adf987e'
            'SKIP'
            'SKIP'
            '11962b5be2a88995c422271d6f5dd34b63897fbc0b7d987e036d5591d13f0d56b5c6e4a7337c2cce85a727ced20573bde9df8c52a73ce69c7ab70bd4b61597dd'
            '36b911e484dcf4e11605572c4b7ff6fe6b893a3ae15cc43ee45b472e195074973ea2b8f562ead80a1aacc1ff811f1b059352230434f87d0cdb5915c311fc716e'
            '182ad2ec0624e7fcaebdcdcbc7b015094e8523731e5786eae18f96bd7a5fe89474a4ba756590e6cb5dfe5e987190b08ea799f75b076f44b5e8964d2c1fb9e07f'
            'd00f4063ac3231135fada50715670832ef17d96e604916807900b6c6e360e062958ec25fe73a51ee2b263b9f5ba152bad01e3bf9d6f93d062e85f72e89c02e0d')


# pkgver() {
# 	cat "$srcdir/$pkgname/indra/newview/viewer_version.txt"
# }

prepare() {
	cd "$srcdir"
	cp 3p-openvr.build-cmd.sh 3p-openvr/build-cmd.sh
	cp 3p-openvr.autobuild.json 3p-openvr/
	cp llviewer* "$pkgname/indra/newview/"
	
	# Patch for linux build
	patch --directory="$srcdir/3p-openvr" --forward --strip=2 --input="${srcdir}/openvr.patch"
 	patch --directory="$srcdir/$pkgname" --forward --strip=2 --input="${srcdir}/firestorm-vr.patch"
	
# 	exit 0
	
	virtualenv ".venv" -p python3
	source .venv/bin/activate
	pip3 install git+https://vcs.firestormviewer.org/autobuild-3.0
	pip3 install llbase
	export AUTOBUILD_VARIABLES_FILE="$srcdir/fs-build-variables/variables"
	
	# Prepare OpenVR includes
	cd "$srcdir/3p-openvr"
	jq -L "$srcdir" -r "include \"_autobuild-json2xml\" ; llsd" "$srcdir/3p-openvr/3p-openvr.autobuild.json" > autobuild.xml
	chmod +x build-cmd.sh
	export AUTOBUILD_CONFIG_FILE=$srcdir/3p-openvr/autobuild.xml
    export AUTOBUILD_BUILD_ID=$(git rev-parse --short HEAD)

    
	autobuild install --verbose
	autobuild build --verbose
	autobuild package --results-file _results.env
    
	## Configure firestorm
	cd "$srcdir/$pkgname"
	unset AUTOBUILD_BUILD_ID
	export AUTOBUILD_CONFIG_FILE="$srcdir/$pkgname/autobuild.xml"
	
	export $(grep -v '^#' $srcdir/3p-openvr/_results.env | xargs)
	autobuild installables add openvr hash="$autobuild_package_md5" url="file://$autobuild_package_filename"
	unset $(grep -v '^#' $srcdir/3p-openvr/_results.env | sed -E 's/(.*)=.*/\1/' | xargs)
	
	export CXXFLAGS="$CXXFLAGS -Wno-error"
	export CFLAGS="$CFLAGS -Wno-error"
	
	autobuild configure -A 64 -c "${_buildtype}" -- -DLL_TESTS:BOOL=FALSE -DREVISION_FROM_VCS=ON -DPACKAGE:BOOL=OFF --chan="ArchLinux-VR"
}

build() {
	cd "$srcdir"
	source .venv/bin/activate
	export CXXFLAGS="$CXXFLAGS -Wno-error"
	export CFLAGS="$CFLAGS -Wno-error"
	cd "$srcdir/$pkgname/build-linux-x86_64"
	make
}

package() {
	mkdir -p "$pkgdir/opt/$pkgname"
	mkdir -p "$pkgdir/usr/share/applications"
	mkdir -p "$pkgdir/usr/share/licenses/$pkgname"
	
	cp -r "$pkgname/build-linux-x86_64/newview/packaged/." "$pkgdir/opt/$pkgname"
	
	install -Dm644 "3p-openvr/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/openvr.LICENSE"
	install -Dm644 "firestorm-vr.desktop" "$pkgdir/usr/share/applications/firestorm-vr.desktop"
	install -Dm755 "firestorm-vr.launcher" "$pkgdir/usr/bin/firestorm-vr"
}
