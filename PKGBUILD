#_                   _ _ _  _ _____ _  _
#| | _______   ____ _| | | || |___  | || |
#| |/ / _ \ \ / / _` | | | || |_ / /| || |_
#|   <  __/\ V / (_| | | |__   _/ / |__   _|
#|_|\_\___| \_/ \__,_|_|_|  |_|/_/     |_|

#Maintainer: kevall474 <kevall474@tuta.io> <https://github.com/kevall474>
#Credits: Laurent Carlier <lordheavym@gmail.com>

#######################################

#Set '1' to build with GCC
#Set '2' to build with CLANG
#Default is empty. It will build with GCC. To build with different compiler just use : env _compiler=(1 or 2) makepkg -s
if [ -z ${_compiler+x} ]; then
  _compiler=
fi

#######################################

pkgname=opencl-clhpp-git
pkgdesc='OpenCL C++ header files'
pkgver=2.0.14
pkgrel=1
arch=('x86_64')
url='https://github.com/KhronosGroup/OpenCL-CLHPP'
license=('Apache-2.0')
makedepends=('cmake' 'doxygen' 'graphviz' 'ninja' 'git' 'clang' 'llvm' 'llvm-libs' 'gcc' 'gcc-libs' 'python')
source=('OpenCL-CLHPP::git+https://github.com/KhronosGroup/OpenCL-CLHPP.git')
md5sums=('SKIP')
depends=('opencl-headers')
conflicts=('opencl-clhpp')
provides=('opencl-clhpp' 'opencl-clhpp-git')

pkgver(){
  cd OpenCL-CLHPP
  echo 2.0.14.$(date -I | sed 's/-/_/' | sed 's/-/_/').$(git rev-list --count HEAD).$(git rev-parse --short HEAD)
}

build(){
if [[ $_compiler = "1" ]]; then
  export CC="gcc"
  export CXX="g++"
elif [[ $_compiler = "2" ]]; then
  export CC="clang"
  export CXX="clang++"
else
  export CC="gcc"
  export CXX="g++"
fi

  cd OpenCL-CLHPP

  rm -rf build

  cmake -H. -G Ninja -Bbuild \
  -DCMAKE_C_FLAGS=-m64 \
  -DCMAKE_CXX_FLAGS=-m64 \
  -DCMAKE_INSTALL_PREFIX=/usr \
  -DBUILD_DOCS=ON \
  -DBUILD_EXAMPLES=OFF \
  -DBUILD_TESTS=OFF

  ninja -C build docs
}

package(){
  DESTDIR="$pkgdir" ninja -C OpenCL-CLHPP/build/ install

  # installing docs
  install -dm755 "${pkgdir}"/usr/share/doc/$pkgname/
  cp -r OpenCL-CLHPP/build/docs/html/* "${pkgdir}"/usr/share/doc/$pkgname/

  # install licence
  install -Dm644 "${srcdir}"/OpenCL-CLHPP/LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
