# My personal build of st - the simple (suckless) terminal

The suckless terminal (st) with some additional features that make it literally the best terminal emulator ever!

## Patches

- alpha
- font2
- ligatures

## Extra Features

- **copy text** with `alt-c`, **paste text** with `alt-v` or `shift-insert`

## Installation

The following dependencies should already be installed before continuing with any of the following steps

- xlib header files
- libharfbuzz build files `sudo apt install libharfbuzz-dev`
- make
- fontconfig
- libX11
- libXft

**Fonts:**

- JetbrainsMono Mono Nerd Font or any nerd font from [here](https://www.nerdfonts.com/font-downloads).
- fonts-noto-color-emoji `sudo apt install fonts-noto-color-emoji`
- fonts-powerline `sudo apt install fonts-powerline`

Most likely you have all of these installed already.

```sh
git clone git@github.com:vandycknick/st.git
make
sudo make install
```

Be sure to have a composite manager (`xcompmgr`, `picom`, etc.) running if you want transparency.

## Emojis and Special Characters

It could be that st crashes while trying to render an emoji or special character. This is due to a shortcoming/bug in libXft, this is fixed as of version 2.3.5. Modern or rolling linux distros like Arch should already shipped with a patched libXft library. On older distros you'll have to build libXft from source and apply the pacth manually:

Clone libXft:

```sh
git https://gitlab.freedesktop.org/xorg/lib/libxft
cd libxft
git checkout libXft-2.3.6
sh autogen.sh --sysconfdir=/etc --prefix=/usr --mandir=/usr/share/man
sudo make install
```

Backup current libXFT binary and replace old lib with patched version:

```sh
sudo ln -sf /usr/lib/libXft.so.2.3.6 /usr/lib/x86_64-linux-gnu/libXft.so.2.3.6
sudo ln -sf /usr/lib/x86_64-linux-gnu/libXft.so.2.3.6 /usr/lib/x86_64-linux-gnu/libXft.so
sudo ln -sf /usr/lib/x86_64-linux-gnu/libXft.so.2.3.6 /usr/lib/x86_64-linux-gnu/libXft.so.2
```

## Configure ST as your default terminal emulator

With `update-alternatives` you can configure st as your default terminal emulator with just a few commands:

```sh
sudo update-alternatives --install /usr/bin/x-terminal-emulator x-terminal-emulator /usr/local/bin/st 0

sudo update-alternatives --set x-terminal-emulator /usr/local/bin/st
```

## Upgrade ST

Clone the upstream ST repository locally:

```sh
git clone git://git.suckless.org/st upstream
```

Go into the upstream directory and create a patch between the current version and the latest version.

> The current version can be found at the top of the config.mk file

```sh
cd upstream
mkdir -p ../patches
git diff 0.8.4 0.8.5 > ../patches/st-0.8.5.diff
```

This creates a new patch, which can be applied with `git apply`

```sh
cd ..
git apply --reject --whitespace=fix ./patches/st-0.8.5
```

Fixup any rejected patches and run `make` to create a new build.
