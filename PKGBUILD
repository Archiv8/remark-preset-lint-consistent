# Maintainer: Archiv8 <archiv8@artisteducator.com>
# Contributor: Archiv8 <archiv8@artisteducator.com>

_relname="remark-preset-lint-consistent"

# pkgbase=
pkgname="nodejs-${_relname}"
pkgver=2.0.3
pkgrel=1
# epoch=
pkgdesc="A remark lint preset to enforce consistency."
arch=("any")
url="https://github.com/remarkjs/remark-lint/tree/master/packages/remark-preset-lint-consistent"
license=("MIT")
# groups=()
depends=("nodejs")
# optdepends=()
makedepends=("jq" "npm")
# checkdepends=()
# provides=()
# conflicts=()
# replaces=()
# backup=()
# options=()
# install=
changelog="CHANGELOG.md"
source=(
"https://registry.npmjs.org/$_relname/-/$_relname-$pkgver.tgz"
"CC-by-SA-v4.md"
"CHANGELOG.md"
"ISSUES.md"
"LICENSE"
"LICENSE.md"
"MIT.md"
"README.md"
)
noextract=("$_relname-$pkgver.tgz")
# validpgpkeys=()
sha512sums=('9abb46ce70229205ef0708107644f72c05a7e3bc101af16cf48b0bb818cd96ebfd2670aa92dd82a4e1b986d9444b9681dd89d5771cd24bb027b45e623714a5a4'
            '68450d3ba6fea9fa7db430716bbae323ab174cdaa1ab06b227293bb4266e8477270b596157d13c6b39d800f0ecc7366949442715ea1c221ae574df78391137d7'
            '89046eaf13e0ddef5d9a0b0090ea4f7602053f173a3fe9b39d69450a97a9ca57f4dc1a5531370cd53b26423a839be916a6f8ce949e7469c82ed37e5732b6b69b'
            'f2e2be841bfcd3ad533ab9f5655625fcd10ea7759de7fdcfb3894ab161300f23b5a9a114144ff0e72fedcbed0d1ec00bd9f0b8ca7e8c807cb3bf5d5eed3f4fe2'
            '0a033730e23dc9c031929022e9be6e6a0bae20229ce6f0ae9389494f563e3e31a2bc996c755af89ad9c45dffd8f32fd774947164afc44e8edb86a3f80a7e96b4'
            'cd220344c9b1e2934aa5c150aa82158a2a0a1feb2f5ec13025a16e578bf1ae91be142da6996c2639a45d3766d01b85df6f6c8203ed48f8310b84433c42bec767'
            'fb01eed648ffe8a3aca47f2ddfa083188ed80311a4d79259a4a18aeae231a316631fe89418b7362029427dafc92056e0ebbebc03b5270e3cf26bcd75c12e41aa'
            '10c01eeda6d4e38e1eb1e4e3d74199703370d20207b7a5336d9a8f7fa606fbc7a6b649c19b8bd36cf1b439bacfa24ec20f75e05e3ed06995fb2767e745974d47')

# prepare () {}

# build() {}

# check() {}

package() {
  # Ensure cache is set when install is run in order to avoid littering user's home directory
  printf "\e[1;32m==>\e[0m \e[38;5;248mBuilding nodejs package... \e[0m\n"
  printf "    This may take some time depending on the size of the package and number of dependencies to download... \e[0m\n"
  npm install --cache "$srcdir/npm-cache" -g --user root --prefix "$pkgdir"/usr "$srcdir"/$_relname-$pkgver.tgz

  # Fix incorrect user / group as highlighted by namcap
  printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible ownership issues\e[0m\n"
  while IFS= read -r fsitem; do
    chown -h root:root "$fsitem"
  done <   <(find "$pkgdir" -not -group root -not -user root)

  # Non-deterministic race in npm gives 777 permissions to random directories.
  # See https://github.com/npm/npm/issues/9359 for details.
  printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible permission issues on directories\e[0m\n"
  find "$pkgdir/usr" -type d -exec chmod 755 '{}' +

  # Remove references to $pkgdir
  printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$pkgdir\e[0m\n"
  find "$pkgdir" -type f -name package.json -print0 | xargs -0 sed -i "/_where/d"

  # Remove references to $srcdir
  printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$srcdir \e[0m\n"
  local tmppackage="$(mktemp)"
  local pkgjson="$pkgdir/usr/lib/node_modules/$_relname/package.json"
  jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
  mv "$tmppackage" "$pkgjson"
  chmod 644 "$pkgjson"

  # Install license
  install -Dm 644 "LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # Create Archiv8 documentation folder
  install -dvm 755 "$pkgdir/usr/share/doc/$pkgname/packaging/"

  # Install Archiv8 Documentation
  install -Dm 644 "CC-by-SA-v4.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/CC-by-SA-v4.md"
  install -Dm 644 "CHANGELOG.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CHANGELOG.md"
  install -Dm 644 "ISSUES.md" "$pkgdir/usr/share/doc/$pkgname/packaging/ISSUES.md"
  install -Dm 644 "LICENSE.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/LICENSE.md"
  install -Dm 644 "MIT.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/MIT.md"
  install -Dm 644 "README.md" "$pkgdir/usr/share/doc/$pkgname/packaging/README.md"
  }
