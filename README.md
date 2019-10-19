base ffmpeg  api  software tools support you analysis video and video info
1:how build ffmpeg for ubuntu linux:
Get the Dependencies
These are packages required for compiling, but you can remove them when you are done if you prefer:

sudo apt-get update -qq && sudo apt-get -y install \
  autoconf \
  automake \
  build-essential \
  cmake \
  git-core \
  libass-dev \
  libfreetype6-dev \
  libsdl2-dev \
  libtool \
  libva-dev \
  libvdpau-dev \
  libvorbis-dev \
  libxcb1-dev \
  libxcb-shm0-dev \
  libxcb-xfixes0-dev \
  pkg-config \
  texinfo \
  wget \
  zlib1g-dev
Note: Server users can omit the ffplay and x11grab dependencies: libsdl2-dev libva-dev libvdpau-dev libxcb1-dev libxcb-shm0-dev libxcb-xfixes0-dev.

In your home directory make a new directory to put all of the source code and binaries into:

mkdir -p ~/ffmpeg_sources ~/bin

Compilation & Installation
This guide assumes that you want to install some of the most common third-party libraries. Each section provides you with the commands needed to install that library.

For each section, copy-paste the entire code-block into your shell.

If you do not require certain features, you may skip the relevant section (if it is not required) and then remove the appropriate ./configure option in FFmpeg. For example, if libvpx is not needed, skip that section and then remove --enable-libvpx from the Install FFmpeg section.

Tip: To significantly speed up the compilation process on systems with multiple cores, you can use the -j option with each make command, such as make -j4.

NASM
An assembler used by some libraries.

If your repository provides nasm version ¡Ý2.13 then you can install that instead of compiling:

sudo apt-get install nasm
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/nasm-2.14.02.tar.bz2 && \
tar xjvf nasm-2.14.02.tar.bz2 && \
cd nasm-2.14.02 && \
./autogen.sh && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" && \
make && \
make install
Yasm
An assembler used by some libraries.

If your repository provides yasm version ¡Ý1.2.0 then you can install that instead of compiling:

sudo apt-get install yasm
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget -O yasm-1.3.0.tar.gz https://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz && \
tar xzvf yasm-1.3.0.tar.gz && \
cd yasm-1.3.0 && \
./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" && \
make && \
make install
libx264
H.264 video encoder. See the H.264 Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-gpl --enable-libx264.

If your repository provides libx264-dev version ¡Ý118 then you can install that instead of compiling:

sudo apt-get install libx264-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C x264 pull 2> /dev/null || git clone --depth 1 https://code.videolan.org/videolan/x264.git && \
cd x264 && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --enable-static --enable-pic && \
PATH="$HOME/bin:$PATH" make && \
make install
libx265
H.265/HEVC video encoder. See the H.265 Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-gpl --enable-libx265.

If your repository provides libx265-dev version ¡Ý68 then you can install that instead of compiling:

sudo apt-get install libx265-dev libnuma-dev
Otherwise you can compile:

sudo apt-get install mercurial libnuma-dev && \
cd ~/ffmpeg_sources && \
if cd x265 2> /dev/null; then hg pull && hg update && cd ..; else hg clone https://bitbucket.org/multicoreware/x265; fi && \
cd x265/build/linux && \
PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DENABLE_SHARED=off ../../source && \
PATH="$HOME/bin:$PATH" make && \
make install
libvpx
VP8/VP9 video encoder/decoder. See the VP9 Video Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-libvpx.

If your repository provides libvpx-dev version ¡Ý1.4.0 then you can install that instead of compiling:

sudo apt-get install libvpx-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C libvpx pull 2> /dev/null || git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git && \
cd libvpx && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --disable-examples --disable-unit-tests --enable-vp9-highbitdepth --as=yasm && \
PATH="$HOME/bin:$PATH" make && \
make install
libfdk-aac
AAC audio encoder. See the AAC Audio Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-libfdk-aac (and --enable-nonfree if you also included --enable-gpl).

If your repository provides libfdk-aac-dev then you can install that instead of compiling:

sudo apt-get install libfdk-aac-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C fdk-aac pull 2> /dev/null || git clone --depth 1 https://github.com/mstorsjo/fdk-aac && \
cd fdk-aac && \
autoreconf -fiv && \
./configure --prefix="$HOME/ffmpeg_build" --disable-shared && \
make && \
make install
libmp3lame
MP3 audio encoder.

Requires ffmpeg to be configured with --enable-libmp3lame.

If your repository provides libmp3lame-dev version ¡Ý3.98.3 then you can install that instead of compiling:

sudo apt-get install libmp3lame-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget -O lame-3.100.tar.gz https://downloads.sourceforge.net/project/lame/lame/3.100/lame-3.100.tar.gz && \
tar xzvf lame-3.100.tar.gz && \
cd lame-3.100 && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --disable-shared --enable-nasm && \
PATH="$HOME/bin:$PATH" make && \
make install
libopus
Opus audio decoder and encoder.

Requires ffmpeg to be configured with --enable-libopus.

If your repository provides libopus-dev version ¡Ý1.1 then you can install that instead of compiling:

sudo apt-get install libopus-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C opus pull 2> /dev/null || git clone --depth 1 https://github.com/xiph/opus.git && \
cd opus && \
./autogen.sh && \
./configure --prefix="$HOME/ffmpeg_build" --disable-shared && \
make && \
make install
libaom
AV1 video encoder/decoder:

Warning: libaom does not yet appear to have a stable API, so compilation of libavcodec/libaomenc.c may occasionally fail. Just wait a day or two for us to catch up with these annoying changes, re-download ffmpeg-snapshot.tar.bz2, and try again. Or skip libaom altogether.

cd ~/ffmpeg_sources && \
git -C aom pull 2> /dev/null || git clone --depth 1 https://aomedia.googlesource.com/aom && \
mkdir -p aom_build && \
cd aom_build && \
PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DENABLE_SHARED=off -DENABLE_NASM=on ../aom && \
PATH="$HOME/bin:$PATH" make && \
make install
FFmpeg
cd ~/ffmpeg_sources && \
wget -O ffmpeg-snapshot.tar.bz2 https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2 && \
tar xjvf ffmpeg-snapshot.tar.bz2 && \
cd ffmpeg && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure \
  --prefix="$HOME/ffmpeg_build" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I$HOME/ffmpeg_build/include" \
  --extra-ldflags="-L$HOME/ffmpeg_build/lib" \
  --extra-libs="-lpthread -lm" \
  --bindir="$HOME/bin" \
  --enable-gpl \
  --enable-libfdk-aac \
  --enable-libass \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-neon  \
  --enable-demuxer=dash  \
  --enable-libxml2 \
  --enable-nonfree && \
PATH="$HOME/bin:$PATH" make && \
make install && \
hash -r

build done static library will build output ~/ffmpeg_build/lib

3:set hwo to prgorame ffmpeg library api sotfware code:
3.1:set ffmpeg pkg_config to you unbuntu config path:
/etc/ld.so.conf.d/
vim libffmpeg.conf
input you ffmpeg pk_config path:
/home/gavin/ffmpeg_build/lib


1:how build ffmpeg for ubuntu linux:
Get the Dependencies
These are packages required for compiling, but you can remove them when you are done if you prefer:

sudo apt-get update -qq && sudo apt-get -y install \
  autoconf \
  automake \
  build-essential \
  cmake \
  git-core \
  libass-dev \
  libfreetype6-dev \
  libsdl2-dev \
  libtool \
  libva-dev \
  libvdpau-dev \
  libvorbis-dev \
  libxcb1-dev \
  libxcb-shm0-dev \
  libxcb-xfixes0-dev \
  pkg-config \
  texinfo \
  wget \
  zlib1g-dev
Note: Server users can omit the ffplay and x11grab dependencies: libsdl2-dev libva-dev libvdpau-dev libxcb1-dev libxcb-shm0-dev libxcb-xfixes0-dev.

In your home directory make a new directory to put all of the source code and binaries into:

mkdir -p ~/ffmpeg_sources ~/bin

Compilation & Installation
This guide assumes that you want to install some of the most common third-party libraries. Each section provides you with the commands needed to install that library.

For each section, copy-paste the entire code-block into your shell.

If you do not require certain features, you may skip the relevant section (if it is not required) and then remove the appropriate ./configure option in FFmpeg. For example, if libvpx is not needed, skip that section and then remove --enable-libvpx from the Install FFmpeg section.

Tip: To significantly speed up the compilation process on systems with multiple cores, you can use the -j option with each make command, such as make -j4.

NASM
An assembler used by some libraries.

If your repository provides nasm version ¡Ý2.13 then you can install that instead of compiling:

sudo apt-get install nasm
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/nasm-2.14.02.tar.bz2 && \
tar xjvf nasm-2.14.02.tar.bz2 && \
cd nasm-2.14.02 && \
./autogen.sh && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" && \
make && \
make install
Yasm
An assembler used by some libraries.

If your repository provides yasm version ¡Ý1.2.0 then you can install that instead of compiling:

sudo apt-get install yasm
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget -O yasm-1.3.0.tar.gz https://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz && \
tar xzvf yasm-1.3.0.tar.gz && \
cd yasm-1.3.0 && \
./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" && \
make && \
make install
libx264
H.264 video encoder. See the H.264 Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-gpl --enable-libx264.

If your repository provides libx264-dev version ¡Ý118 then you can install that instead of compiling:

sudo apt-get install libx264-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C x264 pull 2> /dev/null || git clone --depth 1 https://code.videolan.org/videolan/x264.git && \
cd x264 && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --enable-static --enable-pic && \
PATH="$HOME/bin:$PATH" make && \
make install
libx265
H.265/HEVC video encoder. See the H.265 Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-gpl --enable-libx265.

If your repository provides libx265-dev version ¡Ý68 then you can install that instead of compiling:

sudo apt-get install libx265-dev libnuma-dev
Otherwise you can compile:

sudo apt-get install mercurial libnuma-dev && \
cd ~/ffmpeg_sources && \
if cd x265 2> /dev/null; then hg pull && hg update && cd ..; else hg clone https://bitbucket.org/multicoreware/x265; fi && \
cd x265/build/linux && \
PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DENABLE_SHARED=off ../../source && \
PATH="$HOME/bin:$PATH" make && \
make install
libvpx
VP8/VP9 video encoder/decoder. See the VP9 Video Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-libvpx.

If your repository provides libvpx-dev version ¡Ý1.4.0 then you can install that instead of compiling:

sudo apt-get install libvpx-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C libvpx pull 2> /dev/null || git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git && \
cd libvpx && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --disable-examples --disable-unit-tests --enable-vp9-highbitdepth --as=yasm && \
PATH="$HOME/bin:$PATH" make && \
make install
libfdk-aac
AAC audio encoder. See the AAC Audio Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-libfdk-aac (and --enable-nonfree if you also included --enable-gpl).

If your repository provides libfdk-aac-dev then you can install that instead of compiling:

sudo apt-get install libfdk-aac-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C fdk-aac pull 2> /dev/null || git clone --depth 1 https://github.com/mstorsjo/fdk-aac && \
cd fdk-aac && \
autoreconf -fiv && \
./configure --prefix="$HOME/ffmpeg_build" --disable-shared && \
make && \
make install
libmp3lame
MP3 audio encoder.

Requires ffmpeg to be configured with --enable-libmp3lame.

If your repository provides libmp3lame-dev version ¡Ý3.98.3 then you can install that instead of compiling:

sudo apt-get install libmp3lame-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget -O lame-3.100.tar.gz https://downloads.sourceforge.net/project/lame/lame/3.100/lame-3.100.tar.gz && \
tar xzvf lame-3.100.tar.gz && \
cd lame-3.100 && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --disable-shared --enable-nasm && \
PATH="$HOME/bin:$PATH" make && \
make install
libopus
Opus audio decoder and encoder.

Requires ffmpeg to be configured with --enable-libopus.

If your repository provides libopus-dev version ¡Ý1.1 then you can install that instead of compiling:

sudo apt-get install libopus-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C opus pull 2> /dev/null || git clone --depth 1 https://github.com/xiph/opus.git && \
cd opus && \
./autogen.sh && \
./configure --prefix="$HOME/ffmpeg_build" --disable-shared && \
make && \
make install
libaom
AV1 video encoder/decoder:

Warning: libaom does not yet appear to have a stable API, so compilation of libavcodec/libaomenc.c may occasionally fail. Just wait a day or two for us to catch up with these annoying changes, re-download ffmpeg-snapshot.tar.bz2, and try again. Or skip libaom altogether.

cd ~/ffmpeg_sources && \
git -C aom pull 2> /dev/null || git clone --depth 1 https://aomedia.googlesource.com/aom && \
mkdir -p aom_build && \
cd aom_build && \
PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DENABLE_SHARED=off -DENABLE_NASM=on ../aom && \
PATH="$HOME/bin:$PATH" make && \
make install
FFmpeg
cd ~/ffmpeg_sources && \
wget -O ffmpeg-snapshot.tar.bz2 https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2 && \
tar xjvf ffmpeg-snapshot.tar.bz2 && \
cd ffmpeg && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure \
  --prefix="$HOME/ffmpeg_build" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I$HOME/ffmpeg_build/include" \
  --extra-ldflags="-L$HOME/ffmpeg_build/lib" \
  --extra-libs="-lpthread -lm" \
  --bindir="$HOME/bin" \
  --enable-gpl \
  --enable-libfdk-aac \
  --enable-libass \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-neon  \
  --enable-demuxer=dash  \
  --enable-libxml2 \
  --enable-nonfree && \
PATH="$HOME/bin:$PATH" make && \
make install && \
hash -r

build done static library will build output ~/ffmpeg_build/lib

3:set hwo to prgorame ffmpeg library api sotfware code:
3.1:set ffmpeg pkg_config to you unbuntu config path:
/etc/ld.so.conf.d/
vim libffmpeg.conf
input you ffmpeg pk_config path:
/home/gavin/ffmpeg_build/lib

1:how build ffmpeg for ubuntu linux:
Get the Dependencies
These are packages required for compiling, but you can remove them when you are done if you prefer:

sudo apt-get update -qq && sudo apt-get -y install \
  autoconf \
  automake \
  build-essential \
  cmake \
  git-core \
  libass-dev \
  libfreetype6-dev \
  libsdl2-dev \
  libtool \
  libva-dev \
  libvdpau-dev \
  libvorbis-dev \
  libxcb1-dev \
  libxcb-shm0-dev \
  libxcb-xfixes0-dev \
  pkg-config \
  texinfo \
  wget \
  zlib1g-dev
Note: Server users can omit the ffplay and x11grab dependencies: libsdl2-dev libva-dev libvdpau-dev libxcb1-dev libxcb-shm0-dev libxcb-xfixes0-dev.

In your home directory make a new directory to put all of the source code and binaries into:

mkdir -p ~/ffmpeg_sources ~/bin

Compilation & Installation
This guide assumes that you want to install some of the most common third-party libraries. Each section provides you with the commands needed to install that library.

For each section, copy-paste the entire code-block into your shell.

If you do not require certain features, you may skip the relevant section (if it is not required) and then remove the appropriate ./configure option in FFmpeg. For example, if libvpx is not needed, skip that section and then remove --enable-libvpx from the Install FFmpeg section.

Tip: To significantly speed up the compilation process on systems with multiple cores, you can use the -j option with each make command, such as make -j4.

NASM
An assembler used by some libraries.

If your repository provides nasm version ¡Ý2.13 then you can install that instead of compiling:

sudo apt-get install nasm
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/nasm-2.14.02.tar.bz2 && \
tar xjvf nasm-2.14.02.tar.bz2 && \
cd nasm-2.14.02 && \
./autogen.sh && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" && \
make && \
make install
Yasm
An assembler used by some libraries.

If your repository provides yasm version ¡Ý1.2.0 then you can install that instead of compiling:

sudo apt-get install yasm
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget -O yasm-1.3.0.tar.gz https://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz && \
tar xzvf yasm-1.3.0.tar.gz && \
cd yasm-1.3.0 && \
./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" && \
make && \
make install
libx264
H.264 video encoder. See the H.264 Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-gpl --enable-libx264.

If your repository provides libx264-dev version ¡Ý118 then you can install that instead of compiling:

sudo apt-get install libx264-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C x264 pull 2> /dev/null || git clone --depth 1 https://code.videolan.org/videolan/x264.git && \
cd x264 && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --enable-static --enable-pic && \
PATH="$HOME/bin:$PATH" make && \
make install
libx265
H.265/HEVC video encoder. See the H.265 Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-gpl --enable-libx265.

If your repository provides libx265-dev version ¡Ý68 then you can install that instead of compiling:

sudo apt-get install libx265-dev libnuma-dev
Otherwise you can compile:

sudo apt-get install mercurial libnuma-dev && \
cd ~/ffmpeg_sources && \
if cd x265 2> /dev/null; then hg pull && hg update && cd ..; else hg clone https://bitbucket.org/multicoreware/x265; fi && \
cd x265/build/linux && \
PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DENABLE_SHARED=off ../../source && \
PATH="$HOME/bin:$PATH" make && \
make install
libvpx
VP8/VP9 video encoder/decoder. See the VP9 Video Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-libvpx.

If your repository provides libvpx-dev version ¡Ý1.4.0 then you can install that instead of compiling:

sudo apt-get install libvpx-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C libvpx pull 2> /dev/null || git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git && \
cd libvpx && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --disable-examples --disable-unit-tests --enable-vp9-highbitdepth --as=yasm && \
PATH="$HOME/bin:$PATH" make && \
make install
libfdk-aac
AAC audio encoder. See the AAC Audio Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-libfdk-aac (and --enable-nonfree if you also included --enable-gpl).

If your repository provides libfdk-aac-dev then you can install that instead of compiling:

sudo apt-get install libfdk-aac-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C fdk-aac pull 2> /dev/null || git clone --depth 1 https://github.com/mstorsjo/fdk-aac && \
cd fdk-aac && \
autoreconf -fiv && \
./configure --prefix="$HOME/ffmpeg_build" --disable-shared && \
make && \
make install
libmp3lame
MP3 audio encoder.

Requires ffmpeg to be configured with --enable-libmp3lame.

If your repository provides libmp3lame-dev version ¡Ý3.98.3 then you can install that instead of compiling:

sudo apt-get install libmp3lame-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget -O lame-3.100.tar.gz https://downloads.sourceforge.net/project/lame/lame/3.100/lame-3.100.tar.gz && \
tar xzvf lame-3.100.tar.gz && \
cd lame-3.100 && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --disable-shared --enable-nasm && \
PATH="$HOME/bin:$PATH" make && \
make install
libopus
Opus audio decoder and encoder.

Requires ffmpeg to be configured with --enable-libopus.

If your repository provides libopus-dev version ¡Ý1.1 then you can install that instead of compiling:

sudo apt-get install libopus-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C opus pull 2> /dev/null || git clone --depth 1 https://github.com/xiph/opus.git && \
cd opus && \
./autogen.sh && \
./configure --prefix="$HOME/ffmpeg_build" --disable-shared && \
make && \
make install
libaom
AV1 video encoder/decoder:

Warning: libaom does not yet appear to have a stable API, so compilation of libavcodec/libaomenc.c may occasionally fail. Just wait a day or two for us to catch up with these annoying changes, re-download ffmpeg-snapshot.tar.bz2, and try again. Or skip libaom altogether.

cd ~/ffmpeg_sources && \
git -C aom pull 2> /dev/null || git clone --depth 1 https://aomedia.googlesource.com/aom && \
mkdir -p aom_build && \
cd aom_build && \
PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DENABLE_SHARED=off -DENABLE_NASM=on ../aom && \
PATH="$HOME/bin:$PATH" make && \
make install
FFmpeg
cd ~/ffmpeg_sources && \
wget -O ffmpeg-snapshot.tar.bz2 https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2 && \
tar xjvf ffmpeg-snapshot.tar.bz2 && \
cd ffmpeg && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure \
  --prefix="$HOME/ffmpeg_build" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I$HOME/ffmpeg_build/include" \
  --extra-ldflags="-L$HOME/ffmpeg_build/lib" \
  --extra-libs="-lpthread -lm" \
  --bindir="$HOME/bin" \
  --enable-gpl \
  --enable-libfdk-aac \
  --enable-libass \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-neon  \
  --enable-demuxer=dash  \
  --enable-libxml2 \
  --enable-nonfree && \
PATH="$HOME/bin:$PATH" make && \
make install && \
hash -r

build done static library will build output ~/ffmpeg_build/lib

3:set hwo to prgorame ffmpeg library api sotfware code:
3.1:set ffmpeg pkg_config to you unbuntu config path:
/etc/ld.so.conf.d/
vim libffmpeg.conf
input you ffmpeg pk_config path:
/home/gavin/ffmpeg_build/lib

1:how build ffmpeg for ubuntu linux:
Get the Dependencies
These are packages required for compiling, but you can remove them when you are done if you prefer:

sudo apt-get update -qq && sudo apt-get -y install \
  autoconf \
  automake \
  build-essential \
  cmake \
  git-core \
  libass-dev \
  libfreetype6-dev \
  libsdl2-dev \
  libtool \
  libva-dev \
  libvdpau-dev \
  libvorbis-dev \
  libxcb1-dev \
  libxcb-shm0-dev \
  libxcb-xfixes0-dev \
  pkg-config \
  texinfo \
  wget \
  zlib1g-dev
Note: Server users can omit the ffplay and x11grab dependencies: libsdl2-dev libva-dev libvdpau-dev libxcb1-dev libxcb-shm0-dev libxcb-xfixes0-dev.

In your home directory make a new directory to put all of the source code and binaries into:

mkdir -p ~/ffmpeg_sources ~/bin

Compilation & Installation
This guide assumes that you want to install some of the most common third-party libraries. Each section provides you with the commands needed to install that library.

For each section, copy-paste the entire code-block into your shell.

If you do not require certain features, you may skip the relevant section (if it is not required) and then remove the appropriate ./configure option in FFmpeg. For example, if libvpx is not needed, skip that section and then remove --enable-libvpx from the Install FFmpeg section.

Tip: To significantly speed up the compilation process on systems with multiple cores, you can use the -j option with each make command, such as make -j4.

NASM
An assembler used by some libraries.

If your repository provides nasm version ¡Ý2.13 then you can install that instead of compiling:

sudo apt-get install nasm
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/nasm-2.14.02.tar.bz2 && \
tar xjvf nasm-2.14.02.tar.bz2 && \
cd nasm-2.14.02 && \
./autogen.sh && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" && \
make && \
make install
Yasm
An assembler used by some libraries.

If your repository provides yasm version ¡Ý1.2.0 then you can install that instead of compiling:

sudo apt-get install yasm
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget -O yasm-1.3.0.tar.gz https://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz && \
tar xzvf yasm-1.3.0.tar.gz && \
cd yasm-1.3.0 && \
./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" && \
make && \
make install
libx264
H.264 video encoder. See the H.264 Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-gpl --enable-libx264.

If your repository provides libx264-dev version ¡Ý118 then you can install that instead of compiling:

sudo apt-get install libx264-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C x264 pull 2> /dev/null || git clone --depth 1 https://code.videolan.org/videolan/x264.git && \
cd x264 && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --enable-static --enable-pic && \
PATH="$HOME/bin:$PATH" make && \
make install
libx265
H.265/HEVC video encoder. See the H.265 Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-gpl --enable-libx265.

If your repository provides libx265-dev version ¡Ý68 then you can install that instead of compiling:

sudo apt-get install libx265-dev libnuma-dev
Otherwise you can compile:

sudo apt-get install mercurial libnuma-dev && \
cd ~/ffmpeg_sources && \
if cd x265 2> /dev/null; then hg pull && hg update && cd ..; else hg clone https://bitbucket.org/multicoreware/x265; fi && \
cd x265/build/linux && \
PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DENABLE_SHARED=off ../../source && \
PATH="$HOME/bin:$PATH" make && \
make install
libvpx
VP8/VP9 video encoder/decoder. See the VP9 Video Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-libvpx.

If your repository provides libvpx-dev version ¡Ý1.4.0 then you can install that instead of compiling:

sudo apt-get install libvpx-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C libvpx pull 2> /dev/null || git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git && \
cd libvpx && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --disable-examples --disable-unit-tests --enable-vp9-highbitdepth --as=yasm && \
PATH="$HOME/bin:$PATH" make && \
make install
libfdk-aac
AAC audio encoder. See the AAC Audio Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-libfdk-aac (and --enable-nonfree if you also included --enable-gpl).

If your repository provides libfdk-aac-dev then you can install that instead of compiling:

sudo apt-get install libfdk-aac-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C fdk-aac pull 2> /dev/null || git clone --depth 1 https://github.com/mstorsjo/fdk-aac && \
cd fdk-aac && \
autoreconf -fiv && \
./configure --prefix="$HOME/ffmpeg_build" --disable-shared && \
make && \
make install
libmp3lame
MP3 audio encoder.

Requires ffmpeg to be configured with --enable-libmp3lame.

If your repository provides libmp3lame-dev version ¡Ý3.98.3 then you can install that instead of compiling:

sudo apt-get install libmp3lame-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
wget -O lame-3.100.tar.gz https://downloads.sourceforge.net/project/lame/lame/3.100/lame-3.100.tar.gz && \
tar xzvf lame-3.100.tar.gz && \
cd lame-3.100 && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --disable-shared --enable-nasm && \
PATH="$HOME/bin:$PATH" make && \
make install
libopus
Opus audio decoder and encoder.

Requires ffmpeg to be configured with --enable-libopus.

If your repository provides libopus-dev version ¡Ý1.1 then you can install that instead of compiling:

sudo apt-get install libopus-dev
Otherwise you can compile:

cd ~/ffmpeg_sources && \
git -C opus pull 2> /dev/null || git clone --depth 1 https://github.com/xiph/opus.git && \
cd opus && \
./autogen.sh && \
./configure --prefix="$HOME/ffmpeg_build" --disable-shared && \
make && \
make install
libaom
AV1 video encoder/decoder:

Warning: libaom does not yet appear to have a stable API, so compilation of libavcodec/libaomenc.c may occasionally fail. Just wait a day or two for us to catch up with these annoying changes, re-download ffmpeg-snapshot.tar.bz2, and try again. Or skip libaom altogether.

cd ~/ffmpeg_sources && \
git -C aom pull 2> /dev/null || git clone --depth 1 https://aomedia.googlesource.com/aom && \
mkdir -p aom_build && \
cd aom_build && \
PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DENABLE_SHARED=off -DENABLE_NASM=on ../aom && \
PATH="$HOME/bin:$PATH" make && \
make install
FFmpeg
cd ~/ffmpeg_sources && \
wget -O ffmpeg-snapshot.tar.bz2 https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2 && \
tar xjvf ffmpeg-snapshot.tar.bz2 && \
cd ffmpeg && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure \
  --prefix="$HOME/ffmpeg_build" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I$HOME/ffmpeg_build/include" \
  --extra-ldflags="-L$HOME/ffmpeg_build/lib" \
  --extra-libs="-lpthread -lm" \
  --bindir="$HOME/bin" \
  --enable-gpl \
  --enable-libfdk-aac \
  --enable-libass \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-neon  \
  --enable-demuxer=dash  \
  --enable-libxml2 \
  --enable-nonfree && \
PATH="$HOME/bin:$PATH" make && \
make install && \
hash -r

build done static library will build output ~/ffmpeg_build/lib

3:set hwo to prgorame ffmpeg library api sotfware code:
3.1:set ffmpeg pkg_config to you unbuntu config path:
/etc/ld.so.conf.d/
vim libffmpeg.conf
input you ffmpeg pk_config path:
/home/gavin/ffmpeg_build/lib

3.2 set ffmpeg pkg_config path to
cd ~
vim .bashrc
append text:
export PKG_CONFIG_PATH=/home/gavin/ffmpeg_build/lib/pkgconfig:$PKG_CONFIG_PATH
source .bashrc

4:write ffmpeg api programe source code:

