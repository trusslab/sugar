# Install Mesa 12.0.6

Copyright (c) 2016-2018 University of California, Irvine. All rights reserved.

Authors: Zhihao Yao, Zongheng Ma, Yingtong Liu, Ardalan Amiri Sani, Aparna Chandramowlishwaran

This document is shared under the GNU Free Documentation License WITHOUT ANY WARRANTY. See https://www.gnu.org/licenses/ for details.

___________________

This guide is based on https://www.mesa3d.org/install.html

**Note: This is NOT Sugar Mesa.**



## Build dependencies

To install Mesa and DRM build dependencies, you need to enable the sources for `apt-get`.

```sh
sudo vim /etc/apt/sources.list
# uncomment this line: 
# deb-src http://*.archive.ubuntu.com/ubuntu/ xenial main restricted
# * is your country code (e.g. us, gb)
sudo apt-get update
sudo apt-get build-dep mesa
```

Note: This is the same as the build dependencies for Sugar Mesa.



## Get the source

```sh
git clone git://anongit.freedesktop.org/git/mesa/mesa
cd mesa
git checkout mesa-12.0.6
export PKG_CONFIG_PATH=/usr/lib/x86_64-linux-gnu/lib/pkgconfig/

#########################################
# configures depend on your GPU model.
#
# Below is an example enabling Radeon GPU
# ./autogen.sh --prefix=/usr/ --enable-gles2 --with-egl-platforms=x11,drm  --enable-shared-glapi --enable-gbm --enable-gallium-llvm --with-dri-drivers=i915,i965,radeon --with-gallium-drivers=radeonsi,r300,r600,swrast
#
# Below is an example for Single GPU machines
./autogen.sh --prefix=/usr/ --enable-gles2 --with-egl-platforms=x11,drm  --enable-shared-glapi --enable-gbm --enable-gallium-llvm --with-dri-drivers=i915,i965
```



## Build

```sh
make -j16
sudo make install
```



## Troubleshooting

#### LLVM not found

If you see this error,

```
checking for RADEON... yes
configure: error: LLVM is required to build Gallium R300 on x86 and x86_64
```

Check if you have installed llvm-3.8. If not, run:

```sh
sudo apt-get install llvm-3.8
```

If you have installed llvm-3.8, and still see this error, modify `configure.ac`:

```sh
vim configure.ac
# find the if statement that causes this problem by searching for "$llvm_prefix"
# replace all 'llvm-config' with 'llvm-config-3.8' in the if statement
```

## Acknowledgments

The work was supported by NSF Award #1617513.
