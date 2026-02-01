# Maintainer: Markus Pesch <markus.pesch plus apps at cryptic.systems>

pkgbase=artifacthub
pkgname=(
  'artifacthub-cli'
  'artifacthub-hub'
  'artifacthub-scanner'
  'artifacthub-tracker'
)
pkgver=1.22.0 # renovate: datasource=github-tags depName=artifacthub/hub extractVersion='^v?(?<version>.*)$'
pkgrel=1
pkgdesc="Find, install and publish Cloud Native packages"
arch=('aarch64' 'x86_64')
url="https://github.com/artifacthub/hub"
license=('Apache 2.0')
makedepends=("go" "tensorflow")
source=(
  "$url/archive/refs/tags/v${pkgver}.zip"
)
sha512sums=('1d636cbfd1dca3aba5764e70d52ad066b6eaf08d99bc9e69689984a1cbbac88e383633d9499bed99ad9c4c975ff9535988b522f0869f24bf9480b9371be57805')
b2sums=('50edb0b262cd168db3cdf113ffddd3d0c0f38ee43b17f82a9e125738363252de8f6b07bcb9963a699ffd60ae7e7c5b2aefd756df795056ca9e9660c797193c79')

prepare() {
  cd hub-${pkgver}

  export GONOSUMDB="${GONOSUMDB}"
  export GOPATH="${srcdir}"
  export GOPROXY="${GOPROXY}"

  env | sort | grep -E '^C?GO'

  go mod download -modcacherw
}

build() {
  # https://wiki.archlinux.org/title/Go_package_guidelines
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"

  export GONOSUMDB="${GONOSUMDB}"
  export GOPATH="${srcdir}"
  export GOPROXY="${GOPROXY}"

  env | sort | grep -E '^C?GO'

  for bin in ah scanner; do
    echo "building ${bin}..."

    cd "hub-${pkgver}/cmd/${bin}"

    go build -v \
      -buildmode=pie \
      -mod=readonly \
      -modcacherw \
      -ldflags "\
        -s -w
        -X main.version=${pkgver}
        -X main.gitCommit=${pkgver}
      " \
      -trimpath \
      -o "${bin}" .

    cd "${srcdir}"
  done
}

package_artifacthub-cli() {
  pkgdesc="Artifact Hub CLI tool"

  # binary
  install -D --mode 0755 "hub-${pkgver}/cmd/ah/ah" "$pkgdir/usr/bin/ah"

  # license
  install -D --mode 0755 "hub-${pkgver}/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

package_artifacthub-hub(){
  pkgdesc="Artifact Hub is a web-based application that enables finding, installing, and publishing Kubernetes packages"

  install -D --mode 0755 "hub-${pkgver}/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

package_artifacthub-scanner(){
  pkgdesc="Artifact Hub scanner tool"
  depends=(
    "artifacthub-hub"
  )

  install -D --mode 0755 "hub-${pkgver}/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

package_artifacthub-tracker(){
  pkgdesc="Artifact Hub tracker tool"
  depends=(
    "artifacthub-hub"
  )

  install -D --mode 0755 "hub-${pkgver}/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
