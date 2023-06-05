# dummy-pm

A dead-simple power manager that does several things.

This is essentially a script that wraps swayidle and checks for battery status.
You can set timers to dim / turn off your screen and suspend the computer.

## Dependencies

The script requires `swayidle` to be installed.
Other dependencies are optional; but if you want the script to work out of the box,
you should install `brightnessctl`, `wlopm` and `swaylock`.

## Usage

1. Download `dummy-pm` and put it somewhere.
2. Create a configuration file, `dummy-pm-config` in `$XDG_CONFIG_DIR` or `~/.config`.
3. Start the daemon by running `dummy-pm start` (use `--foreground` to run it in foreground).

## Configuration

There's not much to configure.
The configuration is done in `dummy-pm-config`, which is a bash file sourced by the script
on runtime.

```sh
# Configuration for dummy-pm
# The duration is in the format (amount)[smh],
# Where s represents seconds, m represents minutes and h represents hours
# For instance, 15s is 15 seconds.

# dim the screen after a certain amount of time
dpm_ac_dim_after=never
dpm_bat_dim_after=5m

# turn off the screen after a certain amount of time
dpm_ac_dpms_after=never
dpm_bat_dpms_after=10m

# suspend the computer after a certain amount of time
dpm_ac_suspend_after=never
dpm_bat_suspend_after=15m

# These commands are called for dimming, undimming, DPMS and other things
# They're run in a shell
dpm_dim_command="brightnessctl --save s 40%"
dpm_undim_command="brightnessctl --restore"
dpm_dpms_on_command="wlopm --off eDP-1"
dpm_dpms_off_command="wlopm --on eDP-1"
dpm_suspend_command="systemctl suspend"
dpm_lock_command="swaylock"
```

Optionally, if you have other ways to check if the computer is plugged in,
you can override the `is_plugged_in` function.

The function should print out 1 or 0,
indicating that the PC is on AC or battery respectively.

For example:

```sh
is_plugged_in() {
	echo 1 # wow, always plugged in
}
```
