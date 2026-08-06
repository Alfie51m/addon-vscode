# Home Assistant Community Add-on: Studio Code Server

This add-on runs [code-server](https://github.com/coder/code-server), which
gives you a Visual Studio Code experience straight from the browser. It allows
you to edit your Home Assistant configuration directly from your web browser,
directly from within the Home Assistant frontend.

This is a personal fork of the original [Studio Code Server][upstream]
add-on by Franck Nijhof, maintained here to keep the bundled editor and
extensions current. It bundles:

- **code-server**: `v4.131.0` (VS Code `1.131.0`)
- **Home Assistant CLI**: `5.2.0`
- Refreshed extensions, including the Home Assistant Config Helper, YAML,
  ESPHome, Prettier, Error Lens, indent-rainbow, Material Design Icons
  Intellisense, and Log File Highlighter
- Refreshed `esphome` and `yamllint` backend tooling

The add-on has the Home Assistant, MDI icons and YAML extensions pre-installed
and pre-configured right out of the box. This means that auto-completion works
instantly, without the need for configuring anything.

## Installation

The installation of this add-on is pretty straightforward and not different in
comparison to installing any other Home Assistant add-on.

1. Click the Home Assistant My button below to open the add-on on your Home
   Assistant instance.

   [![Open this add-on in your Home Assistant instance.][addon-badge]][addon]

1. Click the "Install" button to install the add-on.
1. Start the "Studio Code Server" add-on.
1. Check the logs of the "Studio Code Server" add-on to see if everything went
   well.
1. Click the "OPEN WEB UI" button to open Studio Code Server.

**Note**: _This add-on always builds locally against this repository's
`Dockerfile` — there is no prebuilt image to pull. Installs and updates will
therefore take a few minutes while Home Assistant compiles the add-on on your
host._

## Configuration

**Note**: _Remember to restart the add-on when the configuration is changed._

Example add-on configuration:

```yaml
log_level: info
config_path: /share/my_path
packages:
  - mariadb-client
init_commands:
  - ls -la
```

**Note**: _This is just an example, don't copy and paste it! Create your own!_

### Option: `log_level`

The `log_level` option controls the level of log output by the addon and can
be changed to be more or less verbose, which might be useful when you are
dealing with an unknown issue. Possible values are:

- `trace`: Show every detail, like all called internal functions.
- `debug`: Shows detailed debug information.
- `info`: Normal (usually) interesting events.
- `warning`: Exceptional occurrences that are not errors.
- `error`: Runtime errors that do not require immediate action.
- `fatal`: Something went terribly wrong. Add-on becomes unusable.

Please note that each level automatically includes log messages from a
more severe level, e.g., `debug` also shows `info` messages. By default,
the `log_level` is set to `info`, which is the recommended setting unless
you are troubleshooting.

### Option: `config_path`

This option allows you to override the default path the add-on will open
when accessing the web interface. For example, use a different
configuration directory like `/share/myconfig` instead of `/config`. If set
to `/root` then all the common folders of HA such as `/config`, `/ssl`,
`/share`, etc. will appear as subfolders for each access.

When not configured, the addon will automatically use the default: `/config`

### Option: `packages`

Allows you to specify additional [Ubuntu packages][ubuntu-packages] to be
installed in your shell environment (e.g., Python, PHP, Go).

**Note**: _Adding many packages will result in a longer start-up
time for the add-on._

### Option: `init_commands`

Customize your VSCode environment even more with the `init_commands` option.
Add one or more shell commands to the list, and they will be executed every
single time this add-on starts.

## Resetting your VSCode settings to the add-on defaults

The add-on updates your settings to be optimized for use with Home Assistant.
As soon as you change a setting, the add-on will stop doing that since it
might be destructive. However, in case you changed some things, but want to
return to the defaults as delivered by this add-on, do the following:

1. Open the Visual Studio Code editor.
1. Click on `Terminal` in the top menu bar and click on `New Terminal`.
1. Execute the following command in the terminal window: `reset-settings`.
1. Done!

## Known issues and limitations

- Can this add-on run on a Raspberry Pi? Yes, but only if you run a 64 bits
  operating system. Also, see point below.
- This add-on currently only supports AMD64 and aarch64/ARM64 machines.
  Although we support ARM devices, please be aware, that this add-on is quite
  heavy to run, and requires quite a bit of RAM. We do not recommended to run
  it on devices with less than 4Gb of memory.
- **Do not use the root directory (`/`) as your workspace.** Opening the root
  directory causes severe performance issues, as VS Code will attempt to index
  the entire filesystem, resulting in excessive CPU and memory usage. Always
  use `/config` (the default) or another specific directory. The add-on will
  prevent startup if the root directory is configured as the workspace.
- "Visual Studio Code is unable to watch for file changes in this large
  workspace" (error ENOSPC)

  This issue is caused by your system not having enough file handles,
  which causes VSCode to be unable to watch all your files. For HassOS,
  currently the only option is to click on the little cog when the
  notification appears and tell it to not show again. In case you have
  a generic Linux setup (e.g., Ubuntu), follow this guide by Microsoft:

  <https://code.visualstudio.com/docs/setup/linux#_visual-studio-code-is-unable-to-watch-for-file-changes-in-this-large-workspace-error-enospc>

## Changelog & Releases

This repository keeps a change log using [GitHub's releases][releases]
functionality.

Releases are based on [Semantic Versioning][semver], and use the format
of `MAJOR.MINOR.PATCH`. In a nutshell, the version will be incremented
based on the following:

- `MAJOR`: Incompatible or major changes.
- `MINOR`: Backwards-compatible new features and enhancements.
- `PATCH`: Backwards-compatible bugfixes and package updates.

## Support

This is a personal fork maintained for my own Home Assistant instance. It is
not affiliated with or supported by the original Studio Code Server project
or its Discord/Community Forum channels.

If you run into an issue specifically with this fork, feel free to
[open an issue here][issue].

For the original, actively-supported project, see
[hassio-addons/addon-vscode][upstream].

## Authors & contributors

The original setup of this repository is by [Franck Nijhof][frenck]. This
fork's version bumps and modifications are maintained by [Alfie51m][me].

## License

MIT License

Copyright (c) 2019-2025 Franck Nijhof

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[addon-badge]: https://my.home-assistant.io/badges/supervisor_addon.svg
[addon]: https://my.home-assistant.io/redirect/supervisor_addon/?addon=a0d7b954_vscode&repository_url=https%3A%2F%2Fgithub.com%2Fhassio-addons%2Frepository
[frenck]: https://github.com/frenck
[issue]: https://github.com/Alfie51m/addon-vscode/issues
[me]: https://github.com/Alfie51m
[releases]: https://github.com/Alfie51m/addon-vscode/releases
[semver]: https://semver.org/spec/v2.0.0
[ubuntu-packages]: https://packages.ubuntu.com
[upstream]: https://github.com/hassio-addons/addon-vscode