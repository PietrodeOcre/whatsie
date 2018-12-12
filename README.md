__Whatsie is not available anymore. Please use the [official client](). The following is a final version of the abandoned, unofficial _ Web_ client Whatsie. This project is/was not affiliated with Whap Inc. in any way. It will not be updated.__

# Whatsie

A simple &amp; beautiful desktop client for [Wasap Web](). Chat without distractions on OS X, Windows and Linux. Not affiliated with  or . This is **NOT** an official product.

![Whatsie Screenshot](./screenshot.png)

## Features

- Themes &amp; Mini Mode
- Native Notifications (with reply on OS X)
- Spell Checker &amp; Auto Correct
- Keyboard Shortcuts
- Launch on OS startup

## How to install

**Note:** If you download from the [releases page](https://github.com/gsantner/Whatsie/releases), be careful what version you pick. Releases that end with `-beta` are beta releases, the ones that end with `-dev` are development releases, and the rest are stable. If you're unsure which to pick, opt for stable. Once you download the app, you'll be able to switch to another channel from the menu. __None of these will be updated__

### OS X

1. Download [whatsie-x.x.x-osx.dmg][LR] or [whatsie-x.x.x-osx.zip][LR]
2. Open or unzip the file and drag the app into the `Applications` folder

### Windows

*Installer (recommended):*

1. Download [whatsie-x.x.x-win32-setup.exe][LR]
2. Run the installer, wait until it finishes

*Portable:*

1. Download [whatsie-x.x.x-win32-portable.zip][LR]
2. Extract the zip wherever you want (e.g. a flash drive) and run the app from there

### Linux

**Ubuntu, Debian 8+ (deb package):**

1. Download [whatsie-x.x.x-linux-arch.deb][LR]
2. Double click and install, or run `dpkg -i whatsie-x.x.x-linux-arch.deb` in the terminal
3. Start the app with your app launcher or by running `whatsie` in a terminal

You can also use `apt-get` (recommended):

```
# Download the gpg key to make sure the deb you download is correct
sudo apt-key adv --keyserver pool.sks-keyservers.net --recv 6DDA23616E3FE905FFDA152AE61DA9241537994D

# Add the repository to your sources list (skip if you've done this already)
# Replace <channel> with stable, beta or dev (pick stable if you're unsure)
echo "deb https://dl.bintray.com/aluxian/deb <channel> main" |
  sudo tee -a /etc/apt/sources.list.d/aluxian.list

# Install Whatsie
sudo apt-get update
sudo apt-get install whatsie
```

**Fedora, CentOS, Red Hat (RPM package):**

1. Download [whatsie-x.x.x-linux-arch.rpm][LR]
2. Double click and install, or run `rpm -ivh whatsie-x.x.x-linux-arch.rpm` in the terminal
3. Start the app with your app launcher or by running `whatsie` in a terminal

You can also use `yum` (recommended):

```
# Add the repository to your repos list (skip if you've done this already)
sudo wget https://bintray.com/aluxian/rpm/rpm -O /etc/yum.repos.d/bintray-aluxian-rpm.repo

# Install Whatsie
sudo yum install whatsie.i386     # for 32-bit distros
sudo yum install whatsie.x86_64   # for 64-bit distros
```

**Arch Linux (AUR):**

1. Simply run `yaourt -S whatsie`
3. Start the app with your app launcher or by running `whatsie` in a terminal

Repository URL: https://aur.archlinux.org/packages/whatsie/

[LR]: https://github.com/gsantner/Whatsie/releases 

## Build

> **Note:** for some tasks, a GitHub access token might be required (if you get errors, make sure you have this token). After you generate it (see [here](https://help.github.com/articles/creating-an-access-token-for-command-line-use/) if you need help;  `repo` permissions are enough), set it as an env var:
> - Unix: `export GITHUB_TOKEN=123`
> - Windows: `set GITHUB_TOKEN=123`

### Install pre-requisites

If you want to build `deb` and `rpm` packages for Linux, you also need [fpm](https://github.com/jordansissel/fpm). To install it on OS X:

```
sudo gem install fpm
brew install rpm
```

### Install dependencies

Global dependencies:

```
npm install -g gulp
```

Local dependencies:

```
npm install
cd src && npm install
```

### Native modules

The app uses native modules. Make sure you rebuild the modules before building the app:

```
gulp rebuild:<32|64>
```

### Build and watch

During development you can use the `watch` tasks, which have live reload. As you edit files in `./src`, they will be re-compiled and moved into the `build` folder:

```
gulp watch:<darwin64|linux32|linux64|win32>
```

If you want to build it just one time, use `build`:

```
gulp build:<darwin64|linux32|linux64|win32>
```

For production builds, set `NODE_ENV=production` or use the `--prod` flag. Production builds don't include dev modules.

```
gulp build:<darwin64|linux32|linux64|win32> --prod
NODE_ENV=production gulp build:<darwin64|linux32|linux64|win32>
```

To see detailed logs, run every gulp task with the `--verbose` flag.

> If you don't specify a platform when running a task, the task will run for the current platform.

### App debug logs

To see debug messages while running the app, set the `DEBUG` env var. This will print logs from the main process.

```
export DEBUG=whatsie:*
```

To open the webview dev tools, type this in the main dev tools console:

```
wv.openDevTools();
```

If you want to automatically open the webview dev tools, use:

```
localStorage.autoLaunchDevTools = true; // on
localStorage.removeItem('autoLaunchDevTools'); // off
```

### Pack

#### OS X

You'll need to set these env vars:

```
SIGN_DARWIN_IDENTITY
SIGN_DARWIN_KEYCHAIN_NAME
SIGN_DARWIN_KEYCHAIN_PASSWORD
```

Pack the app in a neat .dmg:

```
gulp pack:darwin64:<dmg:zip> [--prod]
```

This uses [node-appdmg](https://www.npmjs.com/package/appdmg) which works only on OS X machines.

#### Windows

You'll need to set these env vars:

```
SIGNTOOL_PATH=
SIGN_WIN_CERTIFICATE_FILE=
SIGN_WIN_CERTIFICATE_PASSWORD=
```

Create an installer. This will also sign every executable inside the app, and the setup exe itself:

```
gulp pack:win32:installer [--prod]
```

Or, if you prefer, create a portable zip. This will also sign the executable:

```
gulp pack:win32:portable [--prod]
```

These tasks only work on Windows machines due to their dependencies: [Squirrel.Windows](https://github.com/Squirrel/Squirrel.Windows) and Microsoft's SignTool.

#### Linux

Create deb/rpm packages:

```
gulp pack:<linux32|linux64>:<deb|rpm> [--prod]
```

Make sure you've installed [fpm](https://github.com/jordansissel/fpm). 
