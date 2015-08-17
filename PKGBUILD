# Maintainer: Jörg Hansen (joerg dot hansen at ymail dot com)

pkgname=osm2pgsql-svn
pkgver=29383
pkgrel=1
pkgdesc="Utility program that converts OpenStreetMap (.OSM) data into a format that can be loaded into PostgreSQL."
url="http://wiki.openstreetmap.org/wiki/Osm2pgsql"
arch=(i686 x86_64)
license=('LGPL')
depends=('postgis' 'libxml2' 'bzip2' 'zlib' 'protobuf-c')
makedepends=('subversion' 'libtool')

_svntrunk=http://svn.openstreetmap.org/applications/utils/export/osm2pgsql
_svnname=osm2pgsql

build() {

    cd $srcdir

    msg "SVN checking out..."

    if [ -d $_svnname/.svn ]; then
        (cd $_svnname && svn up -r $pkgver)
    else
        svn co $_svntrunk --config-dir ./ -r $pkgver $_svnname
    fi

    msg "SVN checkout done"

    if [[ -d $srcdir/$_svnname-build ]]; then
        msg "Removing previous build directory..."
        rm -rf $srcdir/$_svnname-build
    fi

    msg "Creating build directory..."
    cp -r $srcdir/$_svnname $srcdir/$_svnname-build

    msg "Configuring..."
    cd $srcdir/$_svnname-build
    ./autogen.sh
    LDFLAGS='' ./configure --prefix=/usr

    msg "Compiling..."
    make

}

package() {

    msg "Installing..."
    cd $srcdir/$_svnname-build
    make DESTDIR="$pkgdir" install

    msg "Additional files..."
    mkdir -p ${pkgdir}/usr/share/doc/osm2pgsql
    install -m644 README ${pkgdir}/usr/share/doc/osm2pgsql
    install -m644 900913.sql ${pkgdir}/usr/share/osm2pgsql

    msg "Cleaning up..."
    find $pkgdir -type f -name "*\.la" -exec rm {} \;

}
