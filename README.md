# polybar-wireguard

![demo](./demo.gif)

A simple Polybar script to display and control WireGuard VPNs.

* Displays the currently connected configuration,
* Defines `--connect`, `--disconnect`, and `--toggle` switches that can be mapped to different mouse clicks.

## Dependencies

* `dmenu` so that you can select a WireGuard config from a menu when connecting. This can be replaced by something like [rofi](https://github.com/davatorium/rofi) or removed altogether by hard-coding a config in the script. You can find my version of `dmenu` [here](https://github.com/mil-ad/dmenu).
* `networkctl` to query currently connected interface without needing sudo. I [used to](https://github.com/mil-ad/polybar-wireguard/blob/838ec1c59f9b9686bece53f9d6e470ff51e35d06/polybar-wireguard#L10) use `wg` for this but `wg` needs sudo access and calling it in a loop generated a lot of warnings and logs.
* [Material design icons](https://github.com/google/material-design-icons/releases) for the ![image](./vpn_lock_icon.png) icon.

## Installation

1. `wg-quick` binary needs sudo access to run which isn't great news for scripting. I have therefore added a specific rule to my `/etc/sudoers` (via `visudo`) to make them password-less:

```
INSERT_YOUR_USERNAME ALL = (root) NOPASSWD: /usr/bin/wg-quick
```

⚠️ You obviously don't want to do this for a user who doesn't already have sudo access! WireGuard let's you to [add hooks](https://github.com/pirate/wireguard-docs#postup) for various events which could let a non-sudo user run arbitrary code.

2. Edit the `configs_path` variable in the script to point it to where your WireGuard configs live. If they're in `/etc/wireguard` you might have to make sure permissions of that directory allows listing the files inside it. I believe by default that directory is only accessible by root.

3. If you're keeping the default icon from Material font, make sure the font is configured correctly in Polybar and the script. Note that the Polybar font indexing is [off by 1](https://github.com/polybar/polybar/wiki/Fonts#fonts). The unmodified script expects the following in the Polybar:

```
font-3 = Material Icons:style=Regular:size=15
```

4. Add a module to Polybar. For example:


```ini
[module/wireguard]
type = custom/script
exec = /PATH/TO/wireguard-polybar
tail = false
interval = 1
click-left = /PATH/TO/wireguard-polybar --toggle &
```
