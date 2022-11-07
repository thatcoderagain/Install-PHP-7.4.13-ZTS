# Please run the following command to install for PHP 7.4.13 [ZTS]

```
sudo apt install build-essential pkg-config autoconf bison re2c libxml2-dev libssl-dev libsqlite3-dev \
libbz2-dev libcurl4-openssl-dev libpng-dev libjpeg-dev libonig-dev libfreetype6-dev \
libxslt1-dev libzip-dev libtidy-dev

VERSION=7.4.13
wget -qO- https://www.php.net/distributions/php-${VERSION}.tar.gz | tar -xz
cd php-${VERSION}/ext

git clone --depth=1 https://github.com/krakjoe/parallel.git
git clone --recursive --depth=1 https://github.com/kjdev/php-ext-brotli.git brotli

cd ..
./buildconf --force

./configure \
    --prefix=/etc/php7z \
    --with-config-file-path=/etc/php7z \
    --with-config-file-scan-dir=/etc/php7z/conf.d \
    --disable-cgi \
    --with-bz2 \
    --with-zlib \
    --with-zip \
    --enable-soap \
    --enable-intl \
    --with-openssl \
    --with-curl \
    --enable-mysqlnd \
    --with-mysqli=mysqlnd \
    --with-pdo-mysql=mysqlnd \
    --enable-pcntl \
    --enable-gd \
    --enable-exif \
    --with-jpeg \
    --with-freetype \
    --with-xsl \
    --enable-bcmath \
    --enable-mbstring \
    --enable-calendar \
    --with-tidy \
    --enable-maintainer-zts \
    --enable-parallel \
    --enable-brotli

make -j$(nproc)
sudo make install

sudo cp php.ini-development /etc/php7z/php.ini
sudo ln -s /etc/php7z/bin/php /usr/bin/phpz

sudo mkdir /etc/php7z/conf.d
sudo nano /etc/php7z/conf.d/additional.ini

memory_limit=-1

[opcache]
zend_extension=opcache
opcache.enable=1
opcache.enable_cli=1
opcache.memory_consumption=512
opcache.interned_strings_buffer=128
```
