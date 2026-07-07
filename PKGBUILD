# Maintainer: Markus Pesch <markus.pesch plus apps at cryptic.systems>

pkgbase=artifacthub
pkgname=(
  'artifacthub-cli'
  'artifacthub-hub'
  'artifacthub-scanner'
  'artifacthub-tracker'
)
pkgver=1.23.0 # renovate: datasource=github-tags depName=artifacthub/hub extractVersion='^v?(?<version>.*)$'
pkgrel=1
pkgdesc="Find, install and publish Cloud Native packages"
arch=('aarch64' 'x86_64')
url="https://github.com/artifacthub/hub"
license=('Apache 2.0')
makedepends=("go" "tensorflow")
source=(
  "$url/archive/refs/tags/v${pkgver}.zip"
)
sha512sums=('20e554c5fc83b7f7346714c50e4059cd20e6ff1d4c511e0a38514ff9e0f3b1cb41de0f3ea7ddc37a70ed4e435cefb430e56987cbb41640f42923d0d11523a674')
b2sums=('340dd3f40efec108d94d0630755da1ed384171b810616aaafa672fbed2f1f71c1784a2d739df126fe0e846e57139fbe61aaa76e2be8100fbacacca780b98107f')

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
