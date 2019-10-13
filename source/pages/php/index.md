title: 我的文档
---

## Prepare For Installation

1. Install Openssl
```
if [ ! -f "/usr/local/openssl/bin/openssl" ];then
    cd /root
    wget -O openssl.tar.gz https://www.openssl.org/source/openssl-1.0.2t.tar.gz
    tar -zxf openssl.tar.gz
    cd openssl
    ./config --openssldir=/usr/local/openssl zlib-dynamic shared
    make
    make install
    echo  "/usr/local/openssl/lib" > /etc/ld.so.conf.d/zopenssl.conf
    ldconfig
    cd ..
    #rm -f openssl.tar.gz
    #rm -rf openssl
fi
```

2. Install Curl
```
CURL_OPENSSL_LIB_VERSION=$(curl -V|grep -oE "OpenSSL.*[0-9][a-z]"|cut -f 2 -d "/")
OPENSSL_LIB_VERSION=$(openssl version|awk '{print $2}')
if [ "${CURL_OPENSSL_LIB_VERSION}" != "${OPENSSL_LIB_VERSION}" ];then
    cd /root
    wget -O curl.tar.gz https://curl.haxx.se/download/curl-7.66.0.tar.gz
    tar -zxf curl.tar.gz
    cd curl
    ./configure --prefix=/usr/local/curl --enable-ares --without-nss --with-ssl=/usr/local/openssl
    make
    make install
    cd ..
    rm -f curl.tar.gz
    rm -rf curl
fi
```

3. Install Icu4c
```
icu4cVer=$(/usr/bin/icu-config --version)
if [ ! -f "/usr/bin/icu-config" ] || [ "${icu4cVer:0:2}" -gt "60" ];then
    cd /root
    wget -O icu4c.tgz https://github.com/unicode-org/icu/releases/download/release-64-2/icu4c-64_2-src.tgz
    tar -xvf icu4c.tgz
    cd icu4c/source
    ./configure --prefix=/usr/local/icu
    make
    make install
    [ -f "/usr/bin/icu-config" ] && mv /usr/bin/icu-config /usr/bin/icu-config.bak 
    ln -sf /usr/local/icu/bin/icu-config /usr/bin/icu-config
    echo "/usr/local/icu/lib" > /etc/ld.so.conf.d/zicu.conf
    ldconfig
    cd ../../
    rm -rf icu4c
    rm -f icu4c.tgz 
fi
```

4. Install PHP_56
```
    cd /root
    php_version="56"
    /etc/init.d/php-fpm-56 stop
    php_setup_path=/usr/local/php/56
    
    mkdir -p ${php_setup_path}
    rm -rf ${php_setup_path}/*
    cd ${php_setup_path}
    wget -O ${php_setup_path}/php56.tar.gz https://www.php.net/distributions/php-5.6.40.tar.gz -T20
    
    tar zxf php56.tar.gz
    mv php-${php_56} src
    cd src
    ./configure --prefix=${php_setup_path} --with-config-file-path=${php_setup_path}/etc --enable-fpm --with-fpm-user=www --with-fpm-group=www --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-iconv-dir --with-freetype-dir=/usr/local/freetype --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl=/usr/local/curl --enable-mbregex --enable-mbstring --with-mcrypt --enable-ftp --with-gd --enable-gd-native-ttf --with-openssl=/usr/local/openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --with-gettext --disable-fileinfo --enable-opcache --enable-intl
    if [ "${Is_64bit}" = "32" ];then
        sed -i 's/lcrypt$/lcrypt -lresolv/' Makefile
    fi
    make -j2
    make install

    if [ ! -f "${php_setup_path}/bin/php" ];then
        echo '========================================================'
        GetSysInfo
        echo -e "ERROR: php-5.6 installation failed.";
        rm -rf ${php_setup_path}
        exit 0;
    fi
    
    Ln_PHP_Bin

    echo "Copy new php configure file..."
    mkdir -p ${php_setup_path}/etc
    \cp php.ini-production ${php_setup_path}/etc/php.ini

    cd ${php_setup_path}
    # php extensions
    Set_Phpini
    Pear_Pecl_Set
    Install_Composer

    echo "Install ZendGuardLoader for PHP 5.6..."
    mkdir -p /usr/local/zend/php56
    if [ "${Is_64bit}" = "64" ] ; then
        wget ${download_Url}/src/zend-loader-php5.6-linux-x86_64.tar.gz -T20
        tar zxf zend-loader-php5.6-linux-x86_64.tar.gz
        \cp zend-loader-php5.6-linux-x86_64/ZendGuardLoader.so /usr/local/zend/php56/
        rm -rf zend-loader-php5.6-linux-x86_64
        rm -f zend-loader-php5.6-linux-x86_64.tar.gz
    else
        wget ${download_Url}/src/zend-loader-php5.6-linux-i386.tar.gz -T20
        tar zxf zend-loader-php5.6-linux-i386.tar.gz
        \cp zend-loader-php5.6-linux-i386/ZendGuardLoader.so /usr/local/zend/php56/
        rm -rf zend-loader-php5.6-linux-i386
        rm -f zend-loader-php5.6-linux-i386.tar.gz
    fi

    echo "Write ZendGuardLoader to php.ini..."
cat >>${php_setup_path}/etc/php.ini<<EOF

;eaccelerator

;ionCube

;opcache

[Zend ZendGuard Loader]
zend_extension=/usr/local/zend/php56/ZendGuardLoader.so
zend_loader.enable=1
zend_loader.disable_licensing=0
zend_loader.obfuscation_level_support=3
zend_loader.license_path=

;xcache

EOF

    Create_Fpm
    Set_PHP_FPM_Opt
    echo "Copy php-fpm init.d file..."
    \cp ${php_setup_path}/src/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm-56
    chmod +x /etc/init.d/php-fpm-56
    
    Service_Add

    rm -f /tmp/php-cgi-56.sock
    /etc/init.d/php-fpm-56 start
    rm -f ${php_setup_path}/src.tar.gz
    echo "${php_56}" > ${php_setup_path}/version.pl
}
```