# blinky for pico2

Blink the LED on a pico2. This is from the examples at repo [rp-rs/rp-hal](https://github.com/rp-rs/rp-hal)
and specifically [blinky.rs](https://github.com/rp-rs/rp-hal/blob/b41c5a7e56c4d31e16dc3ff43a802bb007d1e0bb/rp235x-hal-examples/src/bin/blinky.rs)

## Set up

Install rust via rustup and then install [picotool](https://github.com/raspberrypi/picotool)
to flash the board, also be sure picotool is in your PATH. On Arch Linux you can install
the are AUR packages [pico-sdk](https://aur.archlinux.org/packages/pico-sdk) and then
[picotool](https://aur.archlinux.org/packages/picotool):

### First install pico-sdk

Install pico-sdk via aur, required by picotool

```
wink@3900x 26-06-02T01:57:05.728Z:~/aur
$ git clone https://aur.archlinux.org/pico-sdk.git
Cloning into 'pico-sdk'...
remote: Enumerating objects: 51, done.
remote: Counting objects: 100% (51/51), done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 51 (delta 21), reused 44 (delta 15), pack-reused 0 (from 0)
Receiving objects: 100% (51/51), 18.03 KiB | 18.03 MiB/s, done.
Resolving deltas: 100% (21/21), done.
wink@3900x 26-06-02T01:57:22.999Z:~/aur
$ cd pico-sdk/
wink@3900x 26-06-02T01:57:28.906Z:~/aur/pico-sdk (master)
$ makepkg -sir
==> Making package: pico-sdk 2.2.0-3 (Mon 01 Jun 2026 06:57:35 PM PDT)
==> Checking runtime dependencies...
==> Checking buildtime dependencies...
==> Retrieving sources...
  -> Cloning pico-sdk git repo...
Cloning into bare repository '/home/wink/aur/pico-sdk/pico-sdk'...
remote: Enumerating objects: 37807, done.
remote: Counting objects: 100% (803/803), done.

...

  -> Purging unwanted files...
  -> Compressing man and info pages...
==> Checking for packaging issues...
libfakeroot internal error: payload not recognized!
==> Creating package "pico-sdk"...
  -> Generating .PKGINFO file...
  -> Generating .BUILDINFO file...
  -> Adding install file...
  -> Generating .MTREE file...
  -> Compressing package...
==> Leaving fakeroot environment.
==> Finished making: pico-sdk 2.2.0-3 (Mon 01 Jun 2026 06:58:54 PM PDT)
==> Installing package pico-sdk with pacman -U...
[sudo] password for wink: 
loading packages...
resolving dependencies...
looking for conflicting packages...

Packages (1) pico-sdk-2.2.0-3

Total Installed Size:  279.91 MiB
Net Upgrade Size:        0.00 MiB

:: Proceed with installation? [Y/n] 
(1/1) checking keys in keyring                                                                     [##########################################################] 100%
(1/1) checking package integrity                                                                   [##########################################################] 100%
(1/1) loading package files                                                                        [##########################################################] 100%
(1/1) checking for file conflicts                                                                  [##########################################################] 100%
(1/1) checking available disk space                                                                [##########################################################] 100%
:: Processing package changes...
(1/1) reinstalling pico-sdk                                                                        [##########################################################] 100%
:: Running post-transaction hooks...
(1/1) Arming ConditionNeedsUpdate...
wink@3900x 26-06-02T01:59:31.701Z:~/aur/pico-sdk (master)
```

Next install picotool:

```
wink@3900x 26-06-02T02:01:12.189Z:~/aur
$ git clone https://aur.archlinux.org/picotool.git
Cloning into 'picotool'...
remote: Enumerating objects: 65, done.
remote: Counting objects: 100% (65/65), done.
remote: Compressing objects: 100% (54/54), done.
remote: Total 65 (delta 16), reused 59 (delta 11), pack-reused 0 (from 0)
Receiving objects: 100% (65/65), 29.54 KiB | 29.54 MiB/s, done.
Resolving deltas: 100% (16/16), done.
wink@3900x 26-06-02T02:01:18.116Z:~/aur
$ cd picotool/
wink@3900x 26-06-02T02:01:24.106Z:~/aur/picotool (master)
$ makepkg -sir
==> Making package: picotool 2.2.0-1 (Mon 01 Jun 2026 07:01:30 PM PDT)
==> Checking runtime dependencies...
==> Checking buildtime dependencies...
==> Retrieving sources...
  -> Downloading picotool-2.2.0.tar.gz...
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
  0      0   0      0   0      0      0      0                              0
100 591.1k   0 591.1k   0      0 703.9k      0                              0
  -> Found 70-picotool.rules
==> Validating source files with sha256sums...
    picotool-2.2.0.tar.gz ... Passed
    70-picotool.rules ... Passed
==> Extracting sources...
  -> Extracting picotool-2.2.0.tar.gz with bsdtar
==> Starting build()...
==> WARNING: PICO_SDK_PATH is not set! Using default path: /usr/share/pico-sdk
-- The C compiler identification is GNU 16.1.1
-- The CXX compiler identification is GNU 16.1.1
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done

...

  -> Generating .BUILDINFO file...
  -> Generating .MTREE file...
  -> Compressing package...
==> Leaving fakeroot environment.
==> Finished making: picotool 2.2.0-1 (Mon 01 Jun 2026 07:02:01 PM PDT)
==> Installing package picotool with pacman -U...
[sudo] password for wink: 
loading packages...
warning: picotool-2.2.0-1 is up to date -- reinstalling
resolving dependencies...
looking for conflicting packages...

Packages (1) picotool-2.2.0-1

Total Installed Size:  2.39 MiB
Net Upgrade Size:      0.00 MiB

:: Proceed with installation? [Y/n]  
(1/1) checking keys in keyring                                                                     [##########################################################] 100%
(1/1) checking package integrity                                                                   [##########################################################] 100%
(1/1) loading package files                                                                        [##########################################################] 100%
(1/1) checking for file conflicts                                                                  [##########################################################] 100%
(1/1) checking available disk space                                                                [##########################################################] 100%
:: Processing package changes...
(1/1) reinstalling picotool                                                                        [##########################################################] 100%
:: Running post-transaction hooks...
(1/2) Reloading device manager configuration...
(2/2) Arming ConditionNeedsUpdate...
wink@3900x 26-06-02T02:02:12.936Z:~/aur/picotool (master)
```

Now verify picotool is installed and is the expected version
v2.2.0 in this case:

```
wink@3900x 26-06-02T02:02:12.936Z:~/aur/picotool (master)
$ picotool version
picotool v2.2.0 (Linux, GNU-16.1.1, Release)
wink@3900x 26-06-02T02:02:21.847Z:~/aur/picotool (master)
```

If it's not maybe you have a previous version installed locally
**which happen to me**. Check using `which picotool` and verify which
is being used. It should be `/usr/bin/picotool`, if not remove
whatever you have and try `which picotool` you have, hopefully:

```
wink@3900x 26-06-02T02:02:21.847Z:~/aur/picotool (master)
$ which picotool
/usr/bin/picotool
wink@3900x 26-06-02T02:04:36.107Z:~/aur/picotool (master)
```

### clone blinky-pic2

Clone this repo, [blinky-pico2](https://github.com/winksaville/blinky-pico2).

## Build and run for ARM

As currently configured the default target is `thumbv8m.main-none-eabihf` so
it's not necessary to specify the target when building or running. But
I'd suggest suing the alias `cargo build-arm` and `cargo run-arm` to be
more explicit.

Build blinky-pico2:
```console
wink@3900x 26-06-02T01:45:18.259Z:~/data/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$ cargo clean
     Removed 1714 files, 1.3GiB total
wink@3900x 26-06-02T01:46:16.761Z:~/data/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$ cargo build-arm
   Compiling proc-macro2 v1.0.89
   Compiling unicode-ident v1.0.13
   Compiling version_check v0.9.5
   Compiling autocfg v1.4.0
   Compiling libc v0.2.161
   
..

   Compiling pio-proc v0.2.2
   Compiling pio-proc v0.3.0
   Compiling pio v0.3.0
   Compiling rp235x-hal v0.4.0
   Compiling blinky-pico2 v0.2.1 (/home/wink/data/prgs/rpi-pico/myrepos/blinky-pico2)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 11.08s
wink@3900x 26-06-02T01:46:32.574Z:~/data/prgs/rpi-pico/myrepos/blinky-pico2 (main)
```

Put the pico2 in bootmode (hold down BOOTSEL while plugging USB cable
attached to the pico2 in to your computers USB port).

Use `run-arm` to `flash` blinky and the green LED should be blinking:
```console
wink@3900x 26-06-02T01:46:32.574Z:~/data/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$ cargo run-arm
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `picotool load -u -v -x -t elf target/thumbv8m.main-none-eabihf/debug/blinky-pico2`
Family ID 'rp2350-arm-s' can be downloaded in absolute space:
  00000000->02000000
Loading into Flash:   [==============================]  100%
Verifying Flash:      [==============================]  100%
  OK

The device was rebooted to start the application.
wink@3900x 26-06-02T01:47:19.457Z:~/data/prgs/rpi-pico/myrepos/blinky-pico2 (main)
```

## Build and run for RISC-V

To build for RISC-V you must specify the target `riscv32imac-unknown-none-elf` and
run or as I'm doing below, use the alias `cargo build-riscv` and `cargo run-riscv`.

Build blinky-pico2:
```console
wink@3900x 24-10-31T17:54:25.040Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$ cargo build-riscv
   Compiling nb v1.1.0
   Compiling byteorder v1.5.0
..
   Compiling frunk v0.4.3
   Compiling rp235x-hal v0.2.0 (/home/wink/prgs/rpi-pico/myrepos/rp-hal/rp235x-hal)
   Compiling blinky-pico2 v0.2.0 (/home/wink/prgs/rpi-pico/myrepos/blinky-pico2)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 6.53s
wink@3900x 24-10-31T17:56:20.061Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
```

Put the pico2 in bootmode (hold down BOOTSEL while plugging USB cable
attached to the pico2 in to your computers USB port).

Use `run` to `flash` blinky and the green LED should be blinking:
```console
wink@3900x 24-10-31T17:56:20.061Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$ cargo run-riscv
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.07s
     Running `picotool load -u -v -x -t elf target/riscv32imac-unknown-none-elf/debug/blinky-pico2`
Family id 'rp2350-riscv' can be downloaded in absolute space:
  00000000->02000000
Loading into Flash: [==============================]  100%
Verifying Flash:    [==============================]  100%
  OK

The device was rebooted to start the application.
wink@3900x 24-10-31T17:57:43.143Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
```

I've also created two custom aliases which build and run
using the release profile, `rra` for ARM (**Note:** be in BOOTSEL mode):
```console
wink@3900x 24-10-31T17:57:43.143Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$ cargo rra
   Compiling proc-macro2 v1.0.89
   Compiling unicode-ident v1.0.13
..
   Compiling pio-proc v0.2.2
   Compiling blinky-pico2 v0.2.0 (/home/wink/prgs/rpi-pico/myrepos/blinky-pico2)
    Finished `release` profile [optimized] target(s) in 11.97s
     Running `picotool load -u -v -x -t elf target/thumbv8m.main-none-eabihf/release/blinky-pico2`
Family id 'rp2350-arm-s' can be downloaded in absolute space:
  00000000->02000000
Loading into Flash: [==============================]  100%
Verifying Flash:    [==============================]  100%
  OK

The device was rebooted to start the application.
wink@3900x 24-10-31T18:02:35.448Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
```

and `rrr` for RISC-V (**Note:** be in BOOTSEL mode):
```console
wink@3900x 24-10-31T18:05:14.004Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$ cargo rrr
   Compiling proc-macro2 v1.0.89
   Compiling unicode-ident v1.0.13
..
   Compiling pio-proc v0.2.2
   Compiling blinky-pico2 v0.2.0 (/home/wink/prgs/rpi-pico/myrepos/blinky-pico2)
    Finished `release` profile [optimized] target(s) in 12.10s
     Running `picotool load -u -v -x -t elf target/riscv32imac-unknown-none-elf/release/blinky-pico2`
Family id 'rp2350-riscv' can be downloaded in absolute space:
  00000000->02000000
Loading into Flash: [==============================]  100%
Verifying Flash:    [==============================]  100%
  OK

The device was rebooted to start the application.
wink@3900x 24-10-31T18:05:30.286Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
```

## Fails with current released of rp235x-hal

As mentioned above the current released of rp235x-hal does not work with this repo,
hopeufull this will be fixed soon. This is what happened when I added rp235x-hal
using `cargo add rp235x-hal --features "binary-info critical-section-impl rt defmt"`
which creates dependency:
```
rp235x-hal = { version = "0.2.0", features = ["binary-info", "critical-section-impl", "rt", "defmt"] }
```

It fails with:
```
 error[E0433]: failed to resolve: could not find `rp_cargo_bin_name` in `binary_info`
   --> src/main.rs:86:23
    |
 86 |     hal::binary_info::rp_cargo_bin_name!(),
    |                       ^^^^^^^^^^^^^^^^^ could not find `rp_cargo_bin_name` in `binary_info`

 error[E0433]: failed to resolve: could not find `rp_cargo_homepage_url` in `binary_info`
   --> src/main.rs:89:23
    |
 89 |     hal::binary_info::rp_cargo_homepage_url!(),
    |                       ^^^^^^^^^^^^^^^^^^^^^ could not find `rp_cargo_homepage_url` in `binary_info`
```

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall
be dual licensed as above, without any additional terms or conditions.
