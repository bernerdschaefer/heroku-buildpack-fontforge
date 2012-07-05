# Heroku Buildpack: Fontforge

This is a Heroku buildpack for fontforge.

## Building Fontforge for Heroku

    curl -L 'http://downloads.sourceforge.net/project/fontforge/fontforge-source/fontforge_full-20110222.tar.bz2' > fontforge_full-20110222.tar.bz2
    bunzip2 fontforge_full-20110222.tar.bz2
    tar xf fontforge_full-20110222.tar.bz2

    patch configure: manually set instances of 'gww_has_gettext' to 'no' in configure
    vulcan build -v -c "export LDFLAGS=-lutil; ./configure --without-x --disable-libff --disable-pyextension --prefix /app/vendor/fontforge-20110222 && make install"
