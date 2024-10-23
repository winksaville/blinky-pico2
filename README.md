# blinky for pico2

Blink the led on a pico2. This is from the examples at repo [rp-rs/rp-hal](https://github.com/rp-rs/rp-hal)
and specifically [blinky.rs](https://github.com/rp-rs/rp-hal/blob/b41c5a7e56c4d31e16dc3ff43a802bb007d1e0bb/rp235x-hal-examples/src/bin/blinky.rs)

## Set up

Install rust via rustup and then install [picotool](https://github.com/raspberrypi/picotool)
to flash the board, also be sure picotool in your PATH. For Arch Linux there are AUR packages
[picotool](https://aur.archlinux.org/packages/picotool)
and [pico-sdk](https://aur.archlinux.org/packages/pico-sdk) which picotool needs:
```
$ picotool version
picotool v2.0.0 (Linux, GNU-14.2.1, Release)
```

Clone [blinky-pico2 repo](https://github.com/winksaville/blinky-pico2).

Clone [rp-rs/rp-hall](https://github.com/rp-rs/rp-hal) so
it's next to `blinky-pico2` or modify the path in `Cargo.toml`. Bottom
line rp235x-hal must be locatable by the dependency line in `Cargo.toml`:
```
rp235x-hal = { path = "../rp-hal/rp235x-hal", version = "0.2.0", features = ["binary-info", "critical-section-impl", "rt", "defmt"] }
```

Next switch to this known working commit; `git switch -C ok-blinky-pico2 247dce8c68d1be46c22ccd01f812252c908cc13a`

## Build and run

Build blinky:
```
wink@fwlaptop 24-10-23T23:02:00.264Z:~/prgs/rust/myrepos/blinky-pico2 (main)
$ cargo build
    Updating crates.io index
  Downloaded darling_macro v0.20.10
  Downloaded autocfg v1.4.0
..
  Downloaded sha2-const-stable v0.1.0
  Downloaded 78 crates (12.7 MB) in 2.26s (largest was `sha2-const-stable` at 4.4 MB)
   Compiling proc-macro2 v1.0.89
   Compiling unicode-ident v1.0.13
..
   Compiling rp235x-hal v0.2.0 (/home/wink/prgs/rust/myrepos/rp-hal/rp235x-hal)
   Compiling pio-proc v0.2.2
   Compiling blinky-pico2 v0.1.0 (/home/wink/prgs/rust/myrepos/blinky-pico2)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 24.02s
wink@fwlaptop 24-10-23T23:02:47.808Z:~/prgs/rust/myrepos/blinky-pico2 (main)
$
```

Put the pico2 in bootmode (hold down BOOTSEL while plugging USB cable
attached to the pico2 in to your computers USB port).

Use `run` to `flash` blinky and the green LED should be blinking:
```
wink@fwlaptop 24-10-23T23:02:47.808Z:~/prgs/rust/myrepos/blinky-pico2 (main)
$ cargo run
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.04s
     Running `picotool load -u -v -x -t elf target/thumbv8m.main-none-eabihf/debug/blinky-pico2`
Family id 'rp2350-arm-s' can be downloaded in absolute space:
  00000000->02000000
Loading into Flash: [==============================]  100%
Verifying Flash:    [==============================]  100%
  OK

The device was rebooted to start the application.
wink@fwlaptop 24-10-23T23:03:19.494Z:~/prgs/rust/myrepos/blinky-pico2 (main)
```

## Fails with current released of rp235x-hal

Using `cargo add rp235x-hal --features "binary-info critical-section-impl rt defmt"` which
creates dependency:
```
rp235x-hal = { version = "0.2.0", features = ["binary-info", "critical-section-impl", "rt", "defmt"] }
```

Fails with:
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

