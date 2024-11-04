# blinky for pico2

Blink the LED on a pico2. This is from the examples at repo [rp-rs/rp-hal](https://github.com/rp-rs/rp-hal)
and specifically [blinky.rs](https://github.com/rp-rs/rp-hal/blob/b41c5a7e56c4d31e16dc3ff43a802bb007d1e0bb/rp235x-hal-examples/src/bin/blinky.rs)

## Set up

Install rust via rustup and then install [picotool](https://github.com/raspberrypi/picotool)
to flash the board, also be sure picotool is in your PATH. On Arch Linux you can install
the are AUR packages [pico-sdk](https://aur.archlinux.org/packages/pico-sdk) and then
[picotool](https://aur.archlinux.org/packages/picotool):
```
$ picotool version
picotool v2.0.0 (Linux, GNU-14.2.1, Release)
```

Clone this repo, [blinky-pico2](https://github.com/winksaville/blinky-pico2).

ATM blinky-pico2 needs rp235x-hal v0.3.0 but it is not yet published
so you cannot just `cargo install rp235x-hal` you must
you must clone [rp-rs/rp-hall](https://github.com/rp-rs/rp-hal) so
it's next to `blinky-pico2` or modify the path in `Cargo.toml` to point at
where you have rp-hall/rp235x-hal. Bottom line, rp235x-hal must be locatable
by the dependency line in `Cargo.toml`:
```
rp235x-hal = { path = "../rp-hal/rp235x-hal", version = "0.3.0", features = ["binary-info", "critical-section-impl", "rt", "defmt"] }
```

Next switch to this known working commit;
`git switch -C ok-blinky-pico2 4a71adf031` which is v0.3.0 although
the latest main may work.

## Build and run for ARM

As currently configured the default target is `thumbv8m.main-none-eabihf` so
it's not necessary to specify the target when building or running. But
I'd suggest suing the alias `cargo build-arm` and `cargo run-arm` to be
more explicit.

Build blinky-pico2:
```console
wink@3900x 24-10-31T17:51:10.307Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$ cargo build-arm
   Compiling proc-macro2 v1.0.89
   Compiling unicode-ident v1.0.13
..
   Compiling pio-proc v0.2.2
   Compiling blinky-pico2 v0.2.0 (/home/wink/prgs/rpi-pico/myrepos/blinky-pico2)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 15.30s
wink@fwlaptop 24-10-23T23:02:47.808Z:~/prgs/rust/myrepos/blinky-pico2 (main)
```

Put the pico2 in bootmode (hold down BOOTSEL while plugging USB cable
attached to the pico2 in to your computers USB port).

Use `run-arm` to `flash` blinky and the green LED should be blinking:
```console
wink@3900x 24-10-31T17:54:12.195Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$ cargo run-arm
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.05s
     Running `picotool load -u -v -x -t elf target/thumbv8m.main-none-eabihf/debug/blinky-pico2`
Family id 'rp2350-arm-s' can be downloaded in absolute space:
  00000000->02000000
Loading into Flash: [==============================]  100%
Verifying Flash:    [==============================]  100%
  OK

The device was rebooted to start the application.
wink@3900x 24-10-31T17:54:25.040Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
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

