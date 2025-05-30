

liblo
=====

liblo is a lightweight library that provides an easy to use implementation of
the OSC protocol. For more information about the OSC protocol, please see:

  - [OSC at CNMAT](http://www.cnmat.berkeley.edu/OpenSoundControl/)
  - [https://opensoundcontrol.stanford.edu/](https://opensoundcontrol.stanford.edu/)
  - [https://en.wikipedia.org/wiki/Open_Sound_Control](https://en.wikipedia.org/wiki/Open_Sound_Control)

The official liblo homepage is here:

  - [liblo homepage](http://liblo.sourceforge.net/)

liblo is portable to most UNIX systems (including OS X) and
Windows. It is released under the GNU Lesser General Public Licence
(LGPL) v2.1 or later.  See COPYING for details.

Building
--------

To build and install liblo, read INSTALL in the main liblo directory.
liblo is configured as a dynamically-linked library. To use liblo in a
new application, after

    configure

you should install liblo with

    make install

so that the liblo library can be located by your application.

To build with MS Visual Studio on Windows, please use CMake as
described next.  See `build/README.md` for more details.

Building with CMake
-------------------

If you prefer the CMake build system, support has been added.  Instead
of the `configure` step listed in the previous section, create a build
directory and initialize CMake:

    mkdir ~/build/liblo
    cmake ~/source/liblo/cmake <more options..>
    make

Examples
--------

See `examples` for example source code for a simple client and two
servers:

  - `example_server.c` uses `lo_server_thread_start()` to create a liblo server in an separate thread.

  - `nonblocking_server_example.c` uses `select()` to wait for either console input or OSC messages, all in a single thread.

  - `example_client.c` uses liblo to send OSC messages to a server.

These examples will work without installing liblo. This is
accomplished by a shell script. For example, `examples/client_example`
is a shell script that runs the "real" program
`examples/.libs/example_client`.  Because of this indirection, you
cannot run `example_client` with a debugger.

Debugging
---------

To debug applications using liblo, one option is to include all the
liblo source code in the application rather than linking with the
liblo library. For more information about this, please see the
(libtool manual)[1]

[1]: http://www.gnu.org/software/libtool/manual/libtool.html#Debugging-executables

To compile liblo with debugging flags, use,

    ./configure --enable-debug

## IPv6 NOTICE

liblo was written to support both IPv4 and IPv6, however it has caused
various problems along the way because most of the currently available
OSC applications like Pd and SuperCollider don't listen on IPv6
sockets. IPv6 is currently disabled by default, but you can enable it
using

    ./configure --enable-ipv6

## Poll() vs Select()

Some platforms may have both `poll()` and `select()` available for listening efficiently on multiple servers/sockets. In this case, liblo will default to using `poll` since it is considered to be more scalable. However, on some platforms (e.g. MacOS) the liblo code path using `select()` is considerably faster. You may wish to explictly disable support for `poll` if your applications do not require extreme scalability and are sensitive to small differences in efficiency. This can be done when compiling the library from source, either using configure:

    ./configure --disable-poll

or if using cmake:

    cmake -DWITH_POLL=OFF <path>