# Glue code for building [FPGA tools](https://rqou.com/jenkins/job/open-fpga-tools/)


## Dependencies
TODO


## arachne-pnr

# macOS

```sh
export PATH=/usr/local/bin:$PATH

mkdir _icestorm
tar xvJf icestorm-master.tar.xz -C _icestorm
```

```sh
sed -i -e 's#CXXFLAGS = -I$(SRC) -std=c++11 -MD $(OPTDEBUGFLAGS) -Wall -Wshadow -Wsign-compare -Werror#CXXFLAGS = -I$(SRC) -std=c++11 -MD $(OPTDEBUGFLAGS) -Wall -Wshadow -Wsign-compare -Werror -mmacosx-version-min=10.7 -stdlib=libc++#g' Makefile
```

```sh
export PATH=/usr/local/bin:$PATH

make PREFIX=/opt/fpgatools ICEBOX=_icestorm/opt/fpgatools/share/icebox
mkdir _build
make PREFIX=/opt/fpgatools ICEBOX=_icestorm/opt/fpgatools/share/icebox DESTDIR=$(pwd)/_build install
tar cvJf arachne-pnr-master.tar.xz -C _build .
```

# Linux

```sh
mkdir _icestorm
tar xvJf icestorm-master.tar.xz -C _icestorm
```

```sh
make PREFIX=/opt/fpgatools ICEBOX=_icestorm/opt/fpgatools/share/icebox
mkdir _build
make PREFIX=/opt/fpgatools ICEBOX=_icestorm/opt/fpgatools/share/icebox DESTDIR=$(pwd)/_build install
tar cvJf arachne-pnr-master.tar.xz -C _build .
```

# Windows

```sh
mkdir _icestorm
tar xvJf icestorm-master.tar.xz -C _icestorm
```

```sh
sed -i -e 's@all: bin/arachne-pnr share/arachne-pnr/chipdb-1k.bin share/arachne-pnr/chipdb-8k.bin@all: bin/arachne-pnr.exe share/arachne-pnr/chipdb-1k.bin share/arachne-pnr/chipdb-8k.bin@g' Makefile
sed -i -e 's@bin/arachne-pnr: src/arachne-pnr.o src/netlist.o src/blif.o src/pack.o src/place.o src/util.o src/io.o src/route.o src/chipdb.o src/location.o src/configuration.o src/line_parser.o src/pcf.o src/global.o src/constant.o src/designstate.o src/version_$(VER_HASH).o@bin/arachne-pnr.exe: src/arachne-pnr.o src/netlist.o src/blif.o src/pack.o src/place.o src/util.o src/io.o src/route.o src/chipdb.o src/location.o src/configuration.o src/line_parser.o src/pcf.o src/global.o src/constant.o src/designstate.o src/version_$(VER_HASH).o@g' Makefile
sed -i -e 's@share/arachne-pnr/chipdb-1k.bin: bin/arachne-pnr $(ICEBOX)/chipdb-1k.txt@share/arachne-pnr/chipdb-1k.bin: bin/arachne-pnr.exe $(ICEBOX)/chipdb-1k.txt@g' Makefile
sed -i -e 's@bin/arachne-pnr -d 1k -c $(ICEBOX)/chipdb-1k.txt --write-binary-chipdb share/arachne-pnr/chipdb-1k.bin@wine ./bin/arachne-pnr -d 1k -c $(ICEBOX)/chipdb-1k.txt --write-binary-chipdb share/arachne-pnr/chipdb-1k.bin@g' Makefile
sed -i -e 's@share/arachne-pnr/chipdb-8k.bin: bin/arachne-pnr $(ICEBOX)/chipdb-8k.txt@share/arachne-pnr/chipdb-8k.bin: bin/arachne-pnr.exe $(ICEBOX)/chipdb-8k.txt@g' Makefile
sed -i -e 's@bin/arachne-pnr -d 8k -c $(ICEBOX)/chipdb-8k.txt --write-binary-chipdb share/arachne-pnr/chipdb-8k.bin@wine ./bin/arachne-pnr -d 8k -c $(ICEBOX)/chipdb-8k.txt --write-binary-chipdb share/arachne-pnr/chipdb-8k.bin@g' Makefile
sed -i -e 's@cp bin/arachne-pnr $(DESTDIR)$(PREFIX)/bin/arachne-pnr@cp bin/arachne-pnr.exe $(DESTDIR)$(PREFIX)/bin/arachne-pnr.exe@g' Makefile
```

```sh
make PREFIX=/opt/fpgatools ICEBOX=_icestorm/opt/fpgatools/share/icebox CC=x86_64-w64-mingw32-gcc-win32 CXX=x86_64-w64-mingw32-g++-win32 LDFLAGS="-static"
mkdir _build
make PREFIX=/opt/fpgatools ICEBOX=_icestorm/opt/fpgatools/share/icebox DESTDIR=$(pwd)/_build install
tar cvJf arachne-pnr-master.tar.xz -C _build .
```


## icestorm

# macOS

```sh
sed -i -e 's#LDLIBS += $(shell for pkg in libftdi1 libftdi; do $(PKG_CONFIG) --silence-errors --libs $$pkg && exit; done; echo -lftdi; )##g' iceprog/Makefile
sed -i -e 's#CFLAGS += $(shell for pkg in libftdi1 libftdi; do $(PKG_CONFIG) --silence-errors --cflags $$pkg && exit; done; )##g' iceprog/Makefile
```

```sh
export PATH=/usr/local/bin:$PATH

make PREFIX=/opt/fpgatools CFLAGS="-mmacosx-version-min=10.7 -stdlib=libc++ -I/Users/rqou/not-quite-brew/usr/local/include/libftdi1" CXXFLAGS="-mmacosx-version-min=10.7 -stdlib=libc++ -std=c++11 -I/Users/rqou/not-quite-brew/usr/local/include/libftdi1" LDFLAGS="-mmacosx-version-min=10.7 -stdlib=libc++ -L/Users/rqou/not-quite-brew/usr/local/lib -lusb-1.0 -framework CoreFoundation -framework IOKit" LIBFTDI_NAME="ftdi1"
mkdir _build
make PREFIX=/opt/fpgatools DESTDIR=$(pwd)/_build install
tar cvJf icestorm-master.tar.xz -C _build .
```

# Linux

```sh
make PREFIX=/opt/fpgatools
mkdir _build
make PREFIX=/opt/fpgatools DESTDIR=$(pwd)/_build install
tar cvJf icestorm-master.tar.xz -C _build .
```

# Windows

```sh
sed -i -e 's@CFLAGS = -MD -O0 -ggdb -Wall -std=c99 -I/usr/local/include@CFLAGS = -MD -O0 -ggdb -Wall -std=c99 -I/usr/local/include -I/home/rqou/windoze/usr/local/include/libftdi1@g' config.mk
sed -i -e 's@CXXFLAGS = -MD -O0 -ggdb -Wall -std=c++11 -I/usr/local/include@CXXFLAGS = -MD -O0 -ggdb -Wall -std=c++11 -I/usr/local/include -I/home/rqou/windoze/usr/local/include/libftdi1@g' config.mk

sed -i -e 's@cp icebox_chipdb.py  $(DESTDIR)$(PREFIX)/bin/icebox_chipdb@cp icebox_chipdb.py  $(DESTDIR)$(PREFIX)/bin/icebox_chipdb.py@g' icebox/Makefile
sed -i -e 's@cp icebox_diff.py  $(DESTDIR)$(PREFIX)/bin/icebox_diff@cp icebox_diff.py  $(DESTDIR)$(PREFIX)/bin/icebox_diff.py@g' icebox/Makefile
sed -i -e 's@cp icebox_explain.py  $(DESTDIR)$(PREFIX)/bin/icebox_explain@cp icebox_explain.py  $(DESTDIR)$(PREFIX)/bin/icebox_explain.py@g' icebox/Makefile
sed -i -e 's@cp icebox_colbuf.py  $(DESTDIR)$(PREFIX)/bin/icebox_colbuf@cp icebox_colbuf.py  $(DESTDIR)$(PREFIX)/bin/icebox_colbuf.py@g' icebox/Makefile
sed -i -e 's@cp icebox_html.py  $(DESTDIR)$(PREFIX)/bin/icebox_html@cp icebox_html.py  $(DESTDIR)$(PREFIX)/bin/icebox_html.py@g' icebox/Makefile
sed -i -e 's@cp icebox_maps.py  $(DESTDIR)$(PREFIX)/bin/icebox_maps@cp icebox_maps.py  $(DESTDIR)$(PREFIX)/bin/icebox_maps.py@g' icebox/Makefile
sed -i -e 's@cp icebox_vlog.py  $(DESTDIR)$(PREFIX)/bin/icebox_vlog@cp icebox_vlog.py  $(DESTDIR)$(PREFIX)/bin/icebox_vlog.py@g' icebox/Makefile
sed -i -e 's@cp icebox_stat.py  $(DESTDIR)$(PREFIX)/bin/icebox_stat@cp icebox_stat.py  $(DESTDIR)$(PREFIX)/bin/icebox_stat.py@g' icebox/Makefile

sed -i -e 's@cp icebram $(DESTDIR)$(PREFIX)/bin/icebram@cp icebram$(EXE) $(DESTDIR)$(PREFIX)/bin/@g' icebram/Makefile
sed -i -e 's@cp icemulti $(DESTDIR)$(PREFIX)/bin/icemulti@cp icemulti$(EXE) $(DESTDIR)$(PREFIX)/bin/@g' icemulti/Makefile
sed -i -e 's@cp icepack $(DESTDIR)$(PREFIX)/bin/icepack@cp icepack$(EXE) $(DESTDIR)$(PREFIX)/bin/@g' icepack/Makefile
sed -i -e 's@ln -sf icepack $(DESTDIR)$(PREFIX)/bin/iceunpack@ln -sf icepack$(EXE) $(DESTDIR)$(PREFIX)/bin/iceunpack$(EXE)@g' icepack/Makefile
sed -i -e 's@cp icepll $(DESTDIR)$(PREFIX)/bin/icepll@cp icepll$(EXE) $(DESTDIR)$(PREFIX)/bin/@g' icepll/Makefile
sed -i -e 's@LDLIBS += $(shell for pkg in libftdi1 libftdi; do $(PKG_CONFIG) --silence-errors --libs $$pkg && exit; done; echo -lftdi; )@LDLIBS += $(shell for pkg in libftdi1 libftdi; do $(PKG_CONFIG) --silence-errors --libs $$pkg && exit; done; echo -lftdi1 -lusb-1.0; )@g' iceprog/Makefile
sed -i -e 's@cp iceprog $(DESTDIR)$(PREFIX)/bin/iceprog@cp iceprog$(EXE) $(DESTDIR)$(PREFIX)/bin/@g' iceprog/Makefile
sed -i -e 's@cp icetime $(DESTDIR)$(PREFIX)/bin/icetime@cp icetime$(EXE) $(DESTDIR)$(PREFIX)/bin/@g' icetime/Makefile
```

```sh
LDFLAGS="-L/home/rqou/windoze/usr/local/lib -L/home/rqou/windoze/libusb/MinGW64/static -static -static-libstdc++" make MXE=1 CXX=x86_64-w64-mingw32-gcc-win32 PREFIX=/opt/fpgatools
mkdir _build
make PREFIX=/opt/fpgatools DESTDIR=$(pwd)/_build MXE=1 install
tar cvJf icestorm-master.tar.xz -C _build .
```


## openfpga

# macOS

```sh
sed -i -e 's#add_subdirectory(tests)##g' CMakeLists.txt
sed -i -e 's#add_subdirectory(gpcosim)##g' src/CMakeLists.txt
```

```sh
export PATH=/usr/local/bin:$PATH

cmake . -DBUILD_DOC=off -DCMAKE_INSTALL_PREFIX=/opt/fpgatools -DCMAKE_CXX_FLAGS="-mmacosx-version-min=10.7 -stdlib=libc++ -I/Users/rqou/not-quite-brew/usr/local/include" -DCMAKE_EXE_LINKER_FLAGS="-mmacosx-version-min=10.7 -stdlib=libc++ -L/Users/rqou/not-quite-brew/usr/local/lib -framework CoreFoundation -framework IOKit"
mkdir _build
make DESTDIR=$(pwd)/_build install
tar cvJf openfpga-master.tar.xz -C _build .
```

# Linux

```sh
sed -i -e 's#add_subdirectory(tests)##g' CMakeLists.txt
sed -i -e 's#add_subdirectory(gpcosim)##g' src/CMakeLists.txt
```

```sh
cmake . -DBUILD_DOC=off -DCMAKE_INSTALL_PREFIX=/opt/fpgatools
mkdir _build
make DESTDIR=$(pwd)/_build install
tar cvJf openfpga-master.tar.xz -C _build .
```

# Windows

```sh
sed -i -e 's#add_subdirectory(tests)##g' CMakeLists.txt
sed -i -e 's#add_subdirectory(gpcosim)##g' src/CMakeLists.txt
```

```sh
cmake . -DBUILD_DOC=off -DCMAKE_INSTALL_PREFIX=/opt/fpgatools -DCMAKE_CXX_COMPILER=x86_64-w64-mingw32-g++-win32 -DCMAKE_SYSTEM_NAME=Windows -DCMAKE_CXX_FLAGS="-I/home/rqou/windoze/mingw-std-threads -I/home/rqou/windoze/libusb/include -I/home/rqou/windoze/usr/local/include" -DCMAKE_EXE_LINKER_FLAGS="-L/home/rqou/windoze/libusb/MinGW64/static -L/home/rqou/windoze/usr/local/lib -static -static-libstdc++"
mkdir _build
make DESTDIR=$(pwd)/_build install
tar cvJf openfpga-master.tar.xz -C _build .
```


## Yosys

# macOS

```sh
sed -i -e 's#LINK_CURSES := 0#LINK_CURSES := 1#g' Makefile
sed -i -e 's#CXXFLAGS := $(CXXFLAGS) -Wall -Wextra -ggdb -I. -I"$(YOSYS_SRC)" -MD -D_YOSYS_ -fPIC -I$(PREFIX)/include#CXXFLAGS := $(CXXFLAGS) -Wall -Wextra -ggdb -I. -I"$(YOSYS_SRC)" -MD -D_YOSYS_ -fPIC -I$(PREFIX)/include -mmacosx-version-min=10.7 -stdlib=libc++#g' Makefile
sed -i -e 's#LDFLAGS := $(LDFLAGS) -L$(LIBDIR)#LDFLAGS := $(LDFLAGS) -L$(LIBDIR) -mmacosx-version-min=10.7 -stdlib=libc++#g' Makefile
sed -i -e 's#CXXFLAGS += -I$(BREW_PREFIX)/readline/include#CXXFLAGS += -I/Users/rqou/not-quite-brew/usr/local/include -I/Users/rqou/not-quite-brew/usr/local/lib/libffi-3.0.13/include#g' Makefile
sed -i -e 's#LDFLAGS += -L$(BREW_PREFIX)/readline/lib#LDFLAGS += -L/Users/rqou/not-quite-brew/usr/local/lib#g' Makefile
sed -i -e 's#PKG_CONFIG_PATH := $(BREW_PREFIX)/libffi/lib/pkgconfig:$(PKG_CONFIG_PATH)##g' Makefile
sed -i -e 's#PKG_CONFIG_PATH := $(BREW_PREFIX)/tcl-tk/lib/pkgconfig:$(PKG_CONFIG_PATH)##g' Makefile
sed -i -e 's#ABCMKARGS = CC="$(CXX)" CXX="$(CXX)"#ABCMKARGS = CC="$(CXX) -mmacosx-version-min=10.7 -stdlib=libc++" CXX="$(CXX) -mmacosx-version-min=10.7 -stdlib=libc++" LD="$(CXX) -mmacosx-version-min=10.7 -stdlib=libc++"#g' Makefile
```

```sh
export PATH=/usr/local/bin:$PATH

make -j4 PREFIX=/opt/fpgatools
mkdir _build
make PREFIX=/opt/fpgatools DESTDIR=$(pwd)/_build install
tar cvJf yosys-master.tar.xz -C _build .
```

# Linux

```sh
make -j4 PREFIX=/opt/fpgatools
mkdir _build
make PREFIX=/opt/fpgatools DESTDIR=$(pwd)/_build install
tar cvJf yosys-master.tar.xz -C _build .
```

# Windows

```sh
sed -i -e 's@CONFIG := clang@CONFIG := mxe@g' Makefile
sed -i -e 's@ENABLE_PLUGINS := 1@ENABLE_PLUGINS := 0@g' Makefile
sed -i -e 's@CXXFLAGS := $(CXXFLAGS) -Wall -Wextra -ggdb -I. -I"$(YOSYS_SRC)" -MD -D_YOSYS_ -fPIC -I$(PREFIX)/include@CXXFLAGS := $(CXXFLAGS) -Wall -Wextra -ggdb -I. -I"$(YOSYS_SRC)" -MD -D_YOSYS_ -fPIC -I$(PREFIX)/include -fpermissive@g' Makefile
sed -i -e 's@LDFLAGS := $(LDFLAGS) -L$(LIBDIR)@LDFLAGS := $(LDFLAGS) -L$(LIBDIR) -static -static-libstdc++@g' Makefile
sed -i -e 's@ABCMKARGS += ARCHFLAGS="-DSIZEOF_VOID_P=4 -DSIZEOF_LONG=4 -DSIZEOF_INT=4 -DWIN32_NO_DLL -DHAVE_STRUCT_TIMESPEC -fpermissive -w"@ABCMKARGS += ARCHFLAGS="-DSIZEOF_VOID_P=8 -DSIZEOF_LONG=4 -DSIZEOF_INT=4 -DWIN32_NO_DLL -DHAVE_STRUCT_TIMESPEC -fpermissive -w -I/home/rqou/windoze/usr/local/include"@g' Makefile
sed -i -e 's@ABCMKARGS += LIBS="lib/x86/pthreadVC2.lib -s" ABC_USE_NO_READLINE=1 CC="$(CXX)" CXX="$(CXX)"@ABCMKARGS += LIBS="lib/x64/pthreadVC2.lib -s -L/home/rqou/windoze/usr/local/lib -lreadline -ltermcap" CC="$(CXX)" CXX="$(CXX)"@g' Makefile
sed -i -e 's@CXXFLAGS += -DYOSYS_ENABLE_READLINE@CXXFLAGS += -DYOSYS_ENABLE_READLINE -I/home/rqou/windoze/usr/local/include@g' Makefile
sed -i -e 's@LDLIBS += -lreadline@LDLIBS += -L/home/rqou/windoze/usr/local/lib -lreadline@g' Makefile
sed -i -e 's@LDLIBS += -lpdcurses@LDLIBS += -ltermcap@g' Makefile
sed -i -e 's@TCL_VERSION ?= tcl$(shell bash -c "tclsh <(echo '"'"'puts [info tclversion]'"'"')")@TCL_VERSION ?= 8.6@g' Makefile
sed -i -e 's@CXXFLAGS += $(shell PKG_CONFIG_PATH=$(PKG_CONFIG_PATH) $(PKG_CONFIG) --silence-errors --cflags tcl || echo -I$(TCL_INCLUDE)) -DYOSYS_ENABLE_TCL@CXXFLAGS += -DYOSYS_ENABLE_TCL -DSTATIC_BUILD@g' Makefile
sed -i -e 's@LDLIBS += $(shell PKG_CONFIG_PATH=$(PKG_CONFIG_PATH) $(PKG_CONFIG) --silence-errors --libs tcl || echo -l$(TCL_VERSION))@LDLIBS += -ltcl86 -lws2_32 -lnetapi32 -luserenv@g' Makefile
sed -i -e 's@echo '"'"'REEBE: NOP pbagnvaf ybpny zbqvsvpngvbaf! Frg NOPERI=qrsnhyg va Lbflf Znxrsvyr!'"'"' | tr '"'"'A-Za-z'"'"' '"'"'N-ZA-Mn-za-m'"'"'; false; \\@true; \\@g' Makefile

sed -i -e 's~cd abc \&\& $(MAKE) DEP= clean \&\& hg pull \&\& hg update -r $(ABCREV);~cd abc \&\& $(MAKE) DEP= clean \&\& hg pull \&\& hg update -r $(ABCREV) \&\& sed -i -e '"'"'s@#define SCL_DIRECTIVE(ITEM)     "."ITEM@#define SCL_DIRECTIVE(ITEM)     "." ITEM@g'"'"' -e '"'"'s@#define SCL_DEF_DIRECTIVE(ITEM) ".default_"ITEM@#define SCL_DEF_DIRECTIVE(ITEM) ".default_" ITEM@g'"'"' src/map/scl/sclCon.h;~g' Makefile
```

```sh
make CXX=x86_64-w64-mingw32-g++-win32 LD=x86_64-w64-mingw32-g++-win32 -j4 PREFIX=/opt/fpgatools
mkdir _build
make CXX=x86_64-w64-mingw32-g++-win32 LD=x86_64-w64-mingw32-g++-win32 PREFIX=/opt/fpgatools DESTDIR=$(pwd)/_build install

cp abc/lib/x64/pthreadVC2.dll _build/opt/fpgatools/bin

tar cvJf yosys-master.tar.xz -C _build .
```
