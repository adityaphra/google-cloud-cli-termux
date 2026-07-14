# Maintainer: Aditya Phra <aditya.phra@gmail.com>
# Contributor: Josef Vybíhal <josef.vybihal@gmail.com>
# Contributor: Polarian <polarian@polarian.dev>
# Contributor: Benjamin Denhartog <ben@sudoforge.com>
# Contributor: Mansour Behabadi <mansour@oxplot.com>
# Contributor: Troy Engel <troyengel+arch@gmail.com>
# Contributor: Geoff Hill <geoff@geoffhill.org>
# Contributor: Sebastien Bariteau <numkem@numkem.org>
# Contributor: Justin Dray <justin@dray.be>
# Contributor: tobbik

#  shellcheck disable=SC2164,SC2154,SC2148,SC2034

# Release Notes: https://cloud.google.com/sdk/docs/release-notes
# Cloud Storage Bucket: https://console.cloud.google.com/storage/browser/cloud-sdk-release/for_packagers/linux
# deb pool:
#  - https://packages.cloud.google.com/apt/dists/cloud-sdk/main/binary-amd64/Packages
#  - https://packages.cloud.google.com/apt/pool/cloud-sdk/google-cloud-cli_516.0.0-0_amd64_e19fae4ce840c378a624e2cbdba2aa87.deb

_extracted_name='google-cloud-sdk'
pkgname='google-cloud-cli'
pkgver=576.0.0
pkgrel=1
pkgdesc="A core set of command-line tools for the Google Cloud Platform. Includes only gcloud core (with beta and alpha commands), gcloud-crc32c and man pages"
url="https://cloud.google.com/cli/"
license=('Apache-2.0')
arch=('aarch64')
depends=('python>=3.10')
options=('!strip' 'staticlibs' '!zipman' '!debug' '!lto')
source=(
  "$pkgname.sh"
)
source_aarch64=("$pkgname-$pkgver.orig_aarch64.tar.gz::https://dl.google.com/dl/cloudsdk/release/downloads/for_packagers/linux/${pkgname}_${pkgver}.orig_aarch64.tar.gz")
sha256sums=('2ed704199807a6a3bc5ba88f310c74adbb1bad0a28d1907a0ed5fe3582c303ca')
sha256sums_aarch64=('6ac95bcc5afa06e9c1e3bd402ecbe1a2092b963d70a8f314215dd4be27e16fc6')

package() {
  sdir="${srcdir}/${_extracted_name}"
  ddir="${terdir}/opt/${pkgname}"

  install -d -m 0755 \
    "${terdir}/opt" \
    "${terdir}/etc/bash_completion.d" \
    "${terdir}/usr/share/zsh/site-functions"

  cp -r "${sdir}" "${ddir}"

  # remove components
  rm -rf "$ddir/platform/gsutil"
  rm -f "$ddir/.install/gsutil"*
  rm -f "$ddir/bin/gsutil"
  rm -f "$ddir/data/cli/gsutil.json"

  rm -rf "$ddir/platform/bq"
  rm -f "$ddir/.install/bq"*
  rm -f "$ddir/bin/bq"
  rm -f "$ddir/data/cli/bq.json"

  install -D -m 0644 "${srcdir}/${source[0]}" \
    "${terdir}/etc/profile.d/google-cloud-cli.sh"

  ln -rsT "${ddir}/completion.bash.inc" \
    "${terdir}/etc/bash_completion.d/google-cloud-cli"

  for cmd in gcloud; do
    ln -rsT "${ddir}/completion.zsh.inc" \
      "${terdir}/usr/share/zsh/site-functions/_${cmd}"
  done

  install -d -m 0755 "${terdir}/usr/bin"
  for bin in gcloud \
    gcloud-crc32c \
    git-credential-gcloud.sh; do
    ln -rsT "${ddir}/bin/${bin}" \
      "${terdir}/usr/bin/${bin}"
  done

  install -d -m 0755 "${terdir}/usr/share"
  rm -rf "$ddir"/{deb,rpm,help}
}
