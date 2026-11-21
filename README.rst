.. image:: https://img.shields.io/github/actions/workflow/status/quodlibet/quodlibet/main.yml?branch=main
    :target: https://github.com/quodlibet/quodlibet/actions

.. image:: https://readthedocs.org/projects/quodlibet/badge/?version=latest
    :target: https://quodlibet.readthedocs.io

.. image:: https://img.shields.io/codecov/c/github/quodlibet/quodlibet
    :target: https://app.codecov.io/gh/quodlibet/quodlibet

.. image:: https://img.shields.io/github/v/release/quodlibet/quodlibet
    :target: https://github.com/quodlibet/quodlibet/releases

.. image:: https://img.shields.io/github/contributors/quodlibet/quodlibet
    :target: https://github.com/quodlibet/quodlibet/graphs/contributors

.. image:: https://hosted.weblate.org/widgets/quodlibet/-/quodlibet/svg-badge.svg
    :alt: Translation status
    :target: https://hosted.weblate.org/engage/quodlibet/



Quod Libet: an audio library, manager & player
==============================================

.. image:: ./quodlibet/images/hicolor/scalable/apps/io.github.quodlibet.QuodLibet.svg
   :width: 128px
   :alt: Quod Libet logo
   :align: left

`Quod Libet <https://quodlibet.readthedocs.io>`_
is a cross-platform audio / music management program.
It provides many ways to view your local libraries,
and supports streaming audio and podcast feeds.
It has extremely flexible metadata editing and searching capabilities.

With over 90 plugins included, you can extend and integrate with almost anything,
or `write your own <https://quodlibet.readthedocs.io/en/latest/development/plugins.html>`_ in Python!

Quod Libet in action
--------------------

.. figure:: https://github.com/quodlibet/quodlibet/raw/main/docs/images/album-list-2017-08.png
    :align: center

See more `screenshots in the user guide <https://quodlibet.readthedocs.io/en/latest/screenshots.html>`_.

Ex Falso: just the tag editor
-----------------------------

Ex Falso is a bare-bones tag editor with the same editing interface as Quod Libet.


Getting Started
---------------

For information on how to build, install or test Quod Libet (and Ex Falso),
check out the
`development documentation <https://quodlibet.readthedocs.org/en/latest/development/overview.html>`_.

For how to use the apps, consult the `user guide <https://quodlibet.readthedocs.io/en/latest/guide/>`_.


Building and Testing Extra Information
--------------------

Requires Python 3.10+ and `Poetry <https://python-poetry.org/>`_ 2.

For Linux and Windows, follow the
`development documentation <https://quodlibet.readthedocs.org/en/latest/development/devenv.html>`_.

On macOS (Homebrew)::

    git clone https://github.com/lennartschaeffer/quodlibet.git
    cd quodlibet
    brew install cairo dbus gst-libav gst-plugins-bad gst-plugins-good gst-plugins-ugly \
        gstreamer gtk-mac-integration gtk+3 libsoup pkg-config pygobject3 poetry
    poetry install

    # Let GTK find Homebrew's icon theme (needed by tests/test_icons.py)
    export XDG_DATA_DIRS=/opt/homebrew/share:${XDG_DATA_DIRS:-/usr/local/share:/usr/share}

Homebrew's GStreamer has no WavPack or SPC/VGM decoders, which
``tests/test_player_gst.py::TGStreamerCodecs::test_decode_all`` needs.
Build them from the sources of your installed GStreamer version::

    brew install meson ninja wavpack game-music-emu
    V=$(pkg-config --modversion gstreamer-1.0)
    mkdir -p ~/gst-extra && cd ~/gst-extra
    for p in good bad; do
        curl -LO https://gstreamer.freedesktop.org/src/gst-plugins-$p/gst-plugins-$p-$V.tar.xz
        tar xf gst-plugins-$p-$V.tar.xz
    done
    (cd gst-plugins-good-$V && meson setup build --prefix=$HOME/gst-extra/install \
        -Dauto_features=disabled -Dwavpack=enabled && ninja -C build install)
    (cd gst-plugins-bad-$V && meson setup build --prefix=$HOME/gst-extra/install \
        -Dauto_features=disabled -Dgme=enabled \
        -Dc_args=-I/opt/homebrew/include -Dc_link_args=-L/opt/homebrew/lib \
        && ninja -C build install)

    export GST_PLUGIN_PATH=$HOME/gst-extra/install/lib/gstreamer-1.0
    rm -f ~/.cache/quodlibet/gst-registry-*.bin   # make Quod Libet rescan plugins

Put the two ``export`` lines in ``~/.zshrc``

Run the tests (all should pass, some are skipped on purpose)::

    poetry run pytest tests

Run the app on macOS with ``./dev-utils/macos/run.sh``, or on Linux with
``poetry run ./quodlibet.py``.


Contact
-------

.. image:: https://img.shields.io/mastodon/follow/109620111805015448?domain=https%3A%2F%2Ffosstodon.org&style=social
    :target: https://fosstodon.org/@quodlibet

.. image:: https://img.shields.io/badge/discord-chat-blue?style=social&logo=discord
    :target: https://discord.gg/9A4RKFEm3c
