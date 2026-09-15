# DS3231 driver for Zephyr

> [!IMPORTANT]
> **This driver is deprecated and no longer recommended for new projects.**
>
> Zephyr now ships an in-tree DS3231 RTC driver
> ([`drivers/rtc/rtc_ds3231.c`](https://github.com/zephyrproject-rtos/zephyr/blob/main/drivers/rtc/rtc_ds3231.c),
> compatible `maxim,ds3231`). It implements the full RTC API — time get/set,
> both alarms, alarm and update callbacks — is maintained upstream, and needs
> no external module.
>
> This repository is kept for reference and for existing projects that still
> depend on it. It receives bug fixes only; no new features are planned.

- [Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/ds3231.pdf)

## Wiring

1. Connect your picoprobe to your pico
2. For the Adafruit DS3231 sensor breakout,

| Pico | Adafruit DS321 breakout |
|------|:-----------------------:|
| GP4  | SDA                     |
| GP5  | SCL                     |
| 3V3  | Vin                     |
| GND  | GND                     |

## Setup

1. Setup a zephyr 3.5 environment on your development machine.
2. Run `west init -m https://github.com/arribada/ds3231-driver-zephyr.git ds3231-env`
3. Run `west update`

The driver and the example applications now live on `main`. Earlier
instructions pointed at the `add-time-apis` branch, which has been merged.

## Flashing

1. Go the app in `examples/simple`
2. To build the example, run `west build -b rpi_pico . -- -DOPENOCD=/usr/bin/openocd -DOPENOCD_DEFAULT_PATH=/usr/share/openocd/scripts -DRPI_PICO_DEBUG_ADAPTER=cmsis-dap`
3. To flash use `west flash`
4. Using a serial utility like `minicom` you can now check the logs. In case of the `examples/shell` app, you can use the same serial utility to interact with the application. Type `help` to get started.

## Status

Known limitations, unlikely to be addressed here — use the upstream driver
instead:

- No input validation in `rtc_set_time()`; the RTC API expects `-EINVAL` on an
  invalid date.
- The year register wraps at 2100 (`tm_year - 100` is not taken modulo 100).
- 12-hour mode is not supported; the driver assumes 24-hour mode.
- `alarm_is_pending()` is a stub that always returns 0.

## License

[MIT](./LICENSE)
