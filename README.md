# Heroku Buildpack: Fontforge

This is a Heroku buildpack for fontforge.

## Usage

    heroku create --stack cedar --buildpack https://github.com/bernerdschaefer/heroku-buildpack-fontforge.git
    git push heroku master

## Notes about the Fontforge Build

The following features are disabled:

* X11 - no X11 user interface
* libff - no fontforge shared library
* pyextension - no python extension

Note that you can still use the python scripting interface for fontforge, but
cannot write normal python scripts which import fontforge.

### Notes on building fontforge for Heroku

Here's how fontforge was built for this buildpack.

    export FF_URL='http://downloads.sourceforge.net/project/fontforge/fontforge-source/fontforge_full-20110222.tar.bz2'
    curl -L $FF_URL > fontforge_full-20110222.tar.bz2
    bunzip2 fontforge_full-20110222.tar.bz2
    tar xf fontforge_full-20110222.tar.bz2

Next, `configure` was manually patched to set `gww_has_gettext` to 'no'. Heroku
includes the necessary headers for gettext, but not the binaries. The only thing this
changes is that all messages will be in english, rather than translatable.

Finally, build it!

    vulcan build -v -c "\
      export LDFLAGS=-lutil; \
      ./configure \
        --without-x \
        --disable-libff \
        --disable-pyextension \
        --prefix /app/vendor/fontforge-20110222 && \
      make install"
