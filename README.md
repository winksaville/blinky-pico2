# blinky for pico2

Blink the led on a pico2. This is from the examples at repo [rp-rs/rp-hal](https://github.com/rp-rs/rp-hal)
and specifically [blinky.rs](https://github.com/rp-rs/rp-hal/blob/b41c5a7e56c4d31e16dc3ff43a802bb007d1e0bb/rp235x-hal-examples/src/bin/blinky.rs)

## Setting up

Install rust via rustup and then install [picotool](https://github.com/raspberrypi/picotool)
so it's on your path. This allows the binary to be flashed onto the pico2.


Clone [rp-rs/rp-hall/rp235x-hal](https://github.com/rp-rs/rp-hal) so
it's next to `blinky-pico2` and the dependency path for rp235x-hal
in `Cargo.toml` is correct:
```
rp235x-hal = { path = "../rp-hal/rp235x-hal", version = "0.2.0", features = ["binary-info", "critical-section-impl", "rt", "defmt"] }
```

Next checkout this known working commit; `git switch -C ok-blinky-pico2 247dce8c68d1be46c22ccd01f812252c908cc13a`

Build the code:
```
wink@3900x 24-10-23T20:59:18.211Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$ cargo build --target=thumbv8m.main-none-eabihf
   Compiling proc-macro2 v1.0.89
   Compiling unicode-ident v1.0.13
..
   Compiling pio-parser v0.2.2
   Compiling rp235x-hal v0.2.0 (/home/wink/prgs/rpi-pico/myrepos/rp-hal/rp235x-hal)
   Compiling pio-proc v0.2.2
   Compiling blinky-pico2 v0.1.0 (/home/wink/prgs/rpi-pico/myrepos/blinky-pico2)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 15.20s
wink@3900x 24-10-23T20:59:36.804Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$
```

Put the pico2 in bootmode by holding now BOOTSEL while plugging in to your
dev computers USB port.

Now do `run` which will `flash` blinky and the green LED should be blinking:
```
$ cargo run --target=thumbv8m.main-none-eabihf
wink@3900x 24-10-23T20:59:36.804Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$ cargo run --target=thumbv8m.main-none-eabihf
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.05s
     Running `picotool load -u -v -x -t elf target/thumbv8m.main-none-eabihf/debug/blinky-pico2`
Family id 'rp2350-arm-s' can be downloaded in absolute space:
  00000000->02000000
Loading into Flash: [==============================]  100%
Verifying Flash:    [==============================]  100%
  OK

The device was rebooted to start the application.
wink@3900x 24-10-23T21:02:36.642Z:~/prgs/rpi-pico/myrepos/blinky-pico2 (main)
$
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

