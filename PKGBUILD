# Maintainer: Andrew Rose <hello@andrewrose.co.uk>
# Contributor: Andrew Rose <hello@andrewrose.co.uk>

pkgname=xphp
pkgdesc='CLI PHP build with ZTS enabled'
pkgver=5.6.4
pkgrel=1

arch=('x86_64' 'i686')
license=('PHP')
url='http://www.php.net'

makedepends=('sqlite' 'unixodbc' 'net-snmp' 'libzip' 'enchant' 'file' 'freetds'
             'libmcrypt' 'tidyhtml' 'aspell' 'libltdl' 'libpng' 'libjpeg' 'icu'
             'curl' 'libxslt' 'openssl' 'bzip2' 'db' 'gmp' 'freetype2' 'systemd')
source=("http://www.php.net/distributions/php-${pkgver}.tar.bz2")
md5sums=('d31629e9c2fb5f438ab2dc0aa597cd82')
depends=('pcre' 'libxml2' 'bzip2' 'curl' 'libxslt' 'tidyhtml' 'sqlite' 'net-snmp'
        'aspell' 'postgresql-libs' 'unixodbc' 'freetds' 'libmcrypt' 'libltdl' 'libldap'
        'libpng' 'libjpeg' 'freetype2' 'libvpx' 'enchant' 'icu')

backup=('etc/xphp/php.ini' 'etc/xphp/pear.conf')

build() {

	phpconfig="--srcdir=../php-${pkgver} \
		 --config-cache \
		 --prefix=/opt/xphp \
		 --bindir=/opt/xphp/bin \
		 --includedir=/opt/xphp/include \
		 --libdir=/opt/xphp/lib \
		 --sysconfdir=/etc/xphp \
		 --localstatedir=/var \
		 --with-layout=GNU \
		 --with-config-file-path=/etc/xphp \
		 --with-config-file-scan-dir=/etc/xphp/conf.d \
		 --disable-rpath \
		 --mandir=/opt/xphp/man \
		 --without-pear \
		 --enable-maintainer-zts \
		 --enable-inline-optimization
	"

	phpextensions="--enable-bcmath=shared \
		--enable-calendar=shared \
		--enable-dba=shared \
		--enable-exif=shared \
		--enable-ftp=shared \
		--enable-gd-native-ttf \
		--enable-intl=shared \
		--enable-mbstring \
		--enable-phar=shared \
		--enable-posix=shared \
		--enable-shmop=shared \
		--enable-soap=shared \
		--enable-sockets=shared \
		--enable-sysvmsg=shared \
		--enable-sysvsem=shared \
		--enable-sysvshm=shared \
		--enable-zip=shared \
		--with-bz2=shared \
		--with-curl=shared \
		--with-db4=/usr \
		--with-enchant=shared,/usr \
		--with-freetype-dir=/usr \
		--with-gd=shared \
		--with-gdbm \
		--with-gettext=shared \
		--with-gmp=shared \
		--with-iconv=shared \
		--with-icu-dir=/usr \
		--with-imap-ssl \
		--with-imap=shared \
		--with-jpeg-dir=/usr \
		--with-vpx-dir=/usr \
		--with-ldap=shared \
		--with-ldap-sasl \
		--with-mcrypt=shared \
		--with-mhash \
		--with-mssql=shared \
		--with-mysql-sock=/var/run/mysqld/mysqld.sock \
		--with-mysql=shared,mysqlnd \
		--with-mysqli=shared,mysqlnd \
		--with-openssl=shared \
		--with-pcre-regex=/usr \
		--with-pdo-mysql=shared,mysqlnd \
		--with-pdo-odbc=shared,unixODBC,/usr \
		--with-pdo-pgsql=shared \
		--with-pdo-sqlite=shared,/usr \
		--with-pgsql=shared \
		--with-png-dir=/usr \
		--with-pspell=shared \
		--with-snmp=shared \
		--with-sqlite3=shared,/usr \
		--with-tidy=shared \
		--with-unixODBC=shared,/usr \
		--with-xmlrpc=shared \
		--with-xsl=shared \
		--with-zlib \
	"

        EXTENSION_DIR=/opt/xphp/lib/modules
        export EXTENSION_DIR
        PEAR_INSTALLDIR=/opt/xphp/pear
        export PEAR_INSTALLDIR

        cd ${srcdir}/php-${pkgver}

        # php
        mkdir ${srcdir}/build-php
        cd ${srcdir}/build-php
        ln -s ../php-${pkgver}/configure
        ./configure ${phpconfig} \
                --disable-cgi \
                --with-readline \
                --enable-pcntl \
                ${phpextensions}
        make
}

package_xphp() {

	cd ${srcdir}/build-php
	make -j1 INSTALL_ROOT=${pkgdir} install

        install -d -m755 ${pkgdir}/opt/xphp/pear
        install -d -m755 ${pkgdir}/usr/bin
        # install php.ini
        install -D -m644 ${srcdir}/php-${pkgver}/php.ini-production ${pkgdir}/etc/xphp/php.ini
        install -d -m755 ${pkgdir}/etc/xphp/conf.d/

	ln -s /opt/xphp/bin/php ${pkgdir}/usr/bin/xphp

        mv ${pkgdir}/opt/xphp/man/man1/php.1 ${pkgdir}/opt/xphp/man/man1/xphp.1.gz
        mv ${pkgdir}/opt/xphp/man/man1/php-config.1 ${pkgdir}/opt/xphp/man/man1/xphp-config.1.gz
        mv ${pkgdir}/opt/xphp/man/man1/phpize.1 ${pkgdir}/opt/xphp/man/man1/xphp-phpize.1.gz
        mv ${pkgdir}/opt/xphp/man/man1/phar.1 ${pkgdir}/opt/xphp/man/man1/xphp-har.1.gz
        mv ${pkgdir}/opt/xphp/man/man1/phar.phar.1 ${pkgdir}/opt/xphp/man/man1/xphp-phar_phar.1.gz

	install -D -m 644 $startdir/extensions.ini ${pkgdir}/etc/xphp/conf.d/extensions.ini || return 1
}
