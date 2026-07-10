[![DDraceNetwork](docs/assets/TClient_Logo_Horizontal.svg)](https://tclient.app) 

[![Build status](https://github.com/TaterClient/TClient/workflows/Build/badge.svg)](https://github.com/TaterClient/TClient/actions/workflows/build.yaml)
<!-- [![Code coverage](https://github.com/TaterClient/TClient/branch/master/graph/badge.svg)](https://codecov.io/gh/TaterClient/TClient/branch/master) -->
<!-- [![Translation status](https://hosted.weblate.org/widget/ddnet/ddnet/svg-badge.svg)](https://hosted.weblate.org/engage/ddnet/) -->

### Taters custom ddnet client with some modifications

Not guaranteed to be bug free, but I will try to fix them.

If ddnet devs are reading this and want to steal my changes please feel free.

Thanks to tela for the logo design, and solly for svg <3

### Links

[Discord](https://discord.gg/BgPSapKRkZ)
[Website](https://tclient.app)

### Installation

* Download the latest [release](https://github.com/sjrc6/TaterClient-ddnet/releases)
* Download a [nightly (dev/unstable) build](https://github.com/sjrc6/TaterClient-ddnet/actions/workflows/fast-build.yml?query=branch%3Amaster)
* [Clone](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) this repo and build using the [guide from DDNet](https://github.com/ddnet/ddnet?tab=readme-ov-file#cloning)

### Translation

FTAPI (a simple wrapper for Google translate) will work out of the box, however it will quickly become overloaded

This is a guide for setting up [libretranslate](https://docs.libretranslate.com/guides/installation/)

First you need an old version of python (3.8, 3.9 or 3.10), along with `pip`

If you do not have this you can use [conda](https://www.anaconda.com/docs/getting-started/miniconda/install#quickstart-install-instructions) to install it

```sh
conda create -n libretranslate python=3.9
conda activate libretranslate
```

Then you can install and run libretranslate, do note that this requires large libraries like `torch` so it's a couple of gigs

```sh
pip install libretranslate
libretranslate
```

You can then set `tc_translate_backend libretranslate`, the port is automatically 5000

### Scripting

TClient supports the [ChaiScript](https://chaiscript.com/) language for simple tasks

Add scripts to your config dir then run them with `chai [scriptname] [args]`

> [!CAUTION]
> There are no runtime restrictions, you can easily `while (true) {}` yourself or run out of memory, be careful!

```js
var a // Declare a variable
a = 1 // Set it
var b = 2 // Do both at once
var c = "strings"
var d = ["lists", 2] // not strongly typed
// var e, f = d // no list deconstruction
print(d[0] + to_string(d[1])) // explicit to_string required for string concat
var bass = "ba" + "s" + "s"
var ass = bass.substr(1, -1) // both indices required, use -1 for end
if (a == b) { // brackets required
	print("this will never happen") // output
} else if (c == "strings") { // string comparison
	exec("echo hello world") // run console stuff
}
var current_game_mode = state("game_mode") // Get the current game mode, all states you can get are listed below
def myfunc(a, b, c) { // yeah it uses def for function definition idk
	print(a, b, c)
	if (a == b) { return "early" }
	c // last statement returns like in rust
}
print(myfunc(1, 2, 3)) // prints "early"
for (var i = 0; i < 10; i += 1) { // for loops (c style)
	print(i) // auto converts to string, will throw if it cant
}
return "top level return"
```

Here is a list of states which are available:

| Return type | Call | Description |
| --- | -- | --- |
| `string` | `state("game_mode")` | Returns the current game mode name (e.g., “DM”, “TDM”, “CTF”). |
| `bool` | `state("game_mode_pvp")` | Whether the current mode is PvP. |
| `bool` | `state("game_mode_race")` | Whether the current mode is a race mode. |
| `bool` | `state("eye_wheel_allowed")` | Whether the “eye wheel” feature is allowed on this server. |
| `bool` | `state("zoom_allowed")` | Whether camera zoom is allowed. |
| `bool` | `state("dummy_allowed")` | Whether using a dummy client is allowed. |
| `bool` | `state("dummy_connected")` | Whether the dummy client is currently connected. |
| `bool` | `state("rcon_authed")` | Whether the client is authenticated with RCON (admin access). |
| `int` | `state("team")` | The player’s current team number. |
| `int` | `state("ddnet_team")` | The player’s DDNet team number. |
| `string` | `state("map")` | The name of the current or connecting map. |
| `string` | `state("server_ip")` | The IP address of the connected or connecting server. |
| `int` | `state("players_connected")` | Number of currently connected players. |
| `int` | `state("players_cap")` | Maximum number of players the server supports. |
| `string` | `state("server_name")` | The server’s name. |
| `string` | `state("community")` | The server’s community identifier. |
| `string` | `state("location")` | The player’s approximate map location (“NW”, “C”, “SE”, etc.). |
| `string` | `state("state")` | The client’s connection state (e.g., “online”, “offline”, “loading”, “demo”). |
| `int` | `state("id", string Name)` | Finds and returns a client ID by player name (exact or case-insensitive match). |
| `string` | `state("name", int Id)` | Returns the name of a player given their client ID. |
| `string` | `state("clan", int Id)` | Returns the clan name of a player given their client ID. |

```js
var what = include("thatscript.chai") // you can include other scripts, they use absolute paths from config dir
print(what) // prints "top level return"
if (!file_exists("file")) { // check if a file exists, also absolute from config dir
	throw("why doesn't this file exist")
}
```

There is also `math` and `re` modules

```js
import("math")
math.pi
math.e
math.pow(1, 2)
math.sqrt(3)
math.sin(1)
math.cos(1)
math.tan(1)
math.asin(1)
math.acos(1)
math.atan(1)
math.atan2(1, 1)
math.log(1)
math.log10(1)
math.log2(1)
math.ceil(1)
math.floor(1)
math.round(1)
math.abs(1)
```

```js
import("re")

if(re.test(re.compile(".+?ello.+?"), "hello")) { // re.test(r, string)
	print("hi")
}
re.match(re.compile("\\d"), "h3ll0", false, fun[](str, match, group) { // re.match(r, string, global, callback)
	print("not global: " + to_string(match) + " " + str)
})
re.match(re.compile("\\d"), "h3ll0", true, fun[](str, match, group) {
	print("global: " + to_string(match) + " " + str)
})
re.match(re.compile("(h3)l(l0)"), "h3ll0", false, fun[](str, match, group) {
	print("groups: " + to_string(match) + " " + to_string(group) + " " + str)
})
print(re.replace(re.compile("\\d"), "h3ll0", true, fun[](str, match, group) { // re.replace(r, string, global, callback)
	if (str == "3") {
		return "e"
	} else if (str == "0") {
		return "o"
	}
	return str
}))
```

### Settings Page

> [!NOTE]
> This is out of date

![image](https://github.com/user-attachments/assets/a6ccb206-9fed-48be-a2d2-8fc50a6be882)
![image](https://github.com/user-attachments/assets/9251509a-d852-41ac-bf6b-9a610db08945)
![image](https://github.com/user-attachments/assets/47dab977-1311-4963-a11a-81b78005b12b)
![image](https://github.com/user-attachments/assets/29bddfd9-fcf1-420c-b7e0-958493051a3c)
![image](https://github.com/user-attachments/assets/efe3528f-a962-4dc0-aa8c-9ca963c246e5)
![image](https://github.com/user-attachments/assets/9f15023d-2a27-44ee-8157-e76da53c875a)

![image](https://user-images.githubusercontent.com/22122579/182528700-4c8238c3-836e-49c3-9996-68025e7f5d58.png)

### Features

> [!NOTE]
> This is out of date

```
tc_run_on_join_console
tc_run_on_join_delay
tc_nameplate_ping_circle
tc_hammer_rotates_with_cursor
tc_freeze_update_fix
tc_show_center
tc_skin_name
tc_color_freeze
tc_freeze_stars
tc_white_feet
tc_white_feet_skin
tc_mini_debug
tc_last_notify
tc_last_notify_text
tc_last_notify_color
tc_cursor_in_spec
tc_render_nameplate_spec
tc_fast_input
tc_fast_input_others
tc_improve_mouse_precision
tc_frozen_tees_hud
tc_frozen_tees_text
tc_frozen_tees_hud_skins
tc_frozen_tees_size
tc_frozen_tees_max_rows
tc_frozen_tees_only_inteam
tc_remove_anti
tc_remove_anti_ticks
tc_remove_anti_delay_ticks
tc_unpred_others_in_freeze
tc_pred_margin_in_freeze
tc_pred_margin_in_freeze_amount
tc_show_others_ghosts
tc_swap_ghosts
tc_hide_frozen_ghosts
tc_pred_ghosts_alpha
tc_unpred_ghosts_alpha
tc_render_ghost_as_circle
tc_outline
tc_outline_in_entities
tc_outline_freeze
tc_outline_unfreeze
tc_outline_tele
tc_outline_solid
tc_outline_width
tc_outline_alpha
tc_outline_alpha_solid
tc_outline_color_solid
tc_outline_color_freeze
tc_outline_color_tele
tc_outline_color_unfreeze
tc_player_indicator
tc_player_indicator_freeze
tc_indicator_alive
tc_indicator_freeze
tc_indicator_dead
tc_indicator_offset
tc_indicator_offset_max
tc_indicator_variable_distance
tc_indicator_variable_max_distance
tc_indicator_radius
tc_indicator_opacity
tc_indicator_inteam
tc_indicator_tees
tc_profile_skin
tc_profile_name
tc_profile_clan
tc_profile_flag
tc_profile_colors
tc_profile_emote
tc_auto_verify
tc_rainbow
tc_rainbow_others
tc_rainbow_mode
tc_reset_bindwheel_mouse
add_profile
add_bindwheel
remove_bindwheel
delete_all_bindwheel_binds
+bindwheel_execute_hover
+bindwheel
tc_regex_chat_ignore
tc_color_freeze_darken
tc_color_freeze_feet
tc_spec_menu_ID
tc_limit_mouse_to_screen
```

If your distribution doesn't ship with a `rustc` that is new enough, you can use `rustup` which automatically provides `rustc` 1.85.0 and above (this command removes `rustc` and reinstalls it as part of `rustup`.):
```sh
sudo apt install rustup
```

In case the `rustc` dependency doesn't have the required version for any reason:
```sh
sudo apt install rustup-1.85
```

On older distributions like Ubuntu 18.04 don't install `google-mock`, but instead set `-DDOWNLOAD_GTEST=ON` when building to get a more recent gtest/gmock version.

On older distributions `rustc` version might be too old, to get an up-to-date Rust compiler you can use [rustup](https://rustup.rs/) with stable channel instead or try the `rustc-mozilla` package.

Or on CentOS, RedHat and AlmaLinux like this:

```sh
sudo yum install cargo cmake ffmpeg-devel freetype-devel gcc gcc-c++ git glew-devel glslang gmock-devel gtest-devel libcurl-devel libnotify-devel libogg-devel libpng-devel libx264-devel ninja-build openssl-devel opus-devel opusfile-devel python3 rust SDL2-devel spirv-tools sqlite-devel vulkan-devel wavpack-devel
```

Or on Fedora like this:

```sh
sudo dnf install cargo cmake ffmpeg-devel freetype-devel gcc gcc-c++ git glew-devel glslang gmock-devel gtest-devel libcurl-devel libnotify-devel libogg-devel libpng-devel make ninja-build openssl-devel opus-devel opusfile-devel python SDL2-devel spirv-tools sqlite-devel vulkan-devel wavpack-devel x264-devel
```

Or on Arch Linux like this:

```sh
sudo pacman -S --needed base-devel cmake curl ffmpeg freetype2 git glew glslang gmock libnotify libpng ninja opusfile python rust sdl2 spirv-tools sqlite vulkan-headers vulkan-icd-loader wavpack x264
```

Or on Gentoo like this:

```sh
emerge --ask dev-build/ninja dev-db/sqlite dev-lang/rust-bin dev-libs/glib dev-libs/openssl dev-util/glslang dev-util/spirv-headers dev-util/spirv-tools media-libs/freetype media-libs/glew media-libs/libglvnd media-libs/libogg media-libs/libpng media-libs/libsdl2 media-libs/libsdl2[vulkan] media-libs/opus media-libs/opusfile media-libs/pnglite media-libs/vulkan-loader[layers] media-sound/wavpack media-video/ffmpeg net-misc/curl x11-libs/gdk-pixbuf x11-libs/libnotify
```

Or on Void Linux like this:

```sh
sudo xbps-install -S base-devel cargo cmake ffmpeg6-devel freetype-devel git glew-devel glslang gtest-devel libcurl-devel libnotify-devel libogg-devel libpng-devel ninja openssl-devel opus-devel opusfile-devel sqlite-devel SPIRV-Tools-devel vulkan-loader wavpack-devel x264-devel SDL2-devel
```

On macOS you can use [homebrew](https://brew.sh/) to install build dependencies like this:

```sh
brew install cmake ffmpeg freetype glew glslang googletest libpng molten-vk ninja opusfile rust SDL2 spirv-tools vulkan-headers wavpack x264
```

If you don't want to use the system libraries, you can pass the `-DPREFER_BUNDLED_LIBS=ON` parameter to cmake.

DDNet requires additional libraries, some of which are bundled for the most common platforms (Windows, Mac, Linux, all x86 and x86\_64) for convenience and the official builds. The bundled libraries for official builds are now in the ddnet-libs submodule. Note that when you build and develop locally, you should ideally use your system's package manager to install the dependencies, instead of relying on ddnet-libs submodule, which does not contain all dependencies anyway (e.g. openssl, vulkan). See the previous section for how to get the dependencies. Alternatively see our [Building Guide](docs/BUILDING.md) for how to disable some features and their dependencies (for example, `-DVULKAN=OFF` won't require Vulkan).

## Building on Linux and macOS

To compile DDNet yourself, execute the following commands in the source root:

```sh
cmake -Bbuild -GNinja
cmake --build build
```

## Building on Windows with the Visual Studio IDE

Download and install some version of [Microsoft Visual Studio](https://www.visualstudio.com/) (At the time of writing, MSVS Community 2022) with **C++ support**.

You'll have to install both [Python 3](https://www.python.org/downloads/) and [Rust](https://rustup.rs/) as well.

Make sure the MSVC build tools, C++ CMake-Tools and the latest Windows SDK version appropriate to your windows version are selected in the installer.

Now open up your Project folder, Visual Studio should automatically detect and configure your project using CMake.

On your tools hotbar next to the triangular "Run" Button, you can now select what you want to start (e.g game-client or game-server) and build it.

## Building on Windows with standalone MSVC build tools

First off you will need to install the following dependencies:

- [MSVC Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/),
- [Python 3](https://www.python.org/downloads/windows/),
- [Rust](https://www.rust-lang.org/tools/install).

To compile with the Vulkan graphics backend (disabled by default), you also need to install the [Vulkan SDK](https://vulkan.lunarg.com/sdk/home).

To compile and build DDNet on Windows, use your IDE of choice either with a CMake integration (e.g Visual Studio Code), or by ~~**deprecated**~~ using the CMake GUI.

Configure CMake to use the MSVC Build Tools appropriate to your System by your IDE's instructions.

If you're using Visual Studio Code, you can use the [CMake Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools) extension to configure and build the project.

You can then open the project folder in Visual Studio Code and press `Ctrl+Shift+P` to open the command palette, then search for `CMake: Configure`.

This will open up a prompt for you to select a kit, select your `Visual Studio` version and save it. You can now use the GUI (bottom left) to compile and build your project.


<a href="https://repology.org/metapackage/ddnet/versions">
	<img src="https://repology.org/badge/vertical-allrepos/ddnet.svg?header=" alt="Packaging status" align="right">
</a>

## Installation from Repository

Debian/Ubuntu

```sh
sudo apt-get install ddnet
```

MacOS

```sh
brew install --cask ddnet
```

Fedora

```sh
sudo dnf install ddnet
```

Arch Linux

```sh
yay -S ddnet
```

FreeBSD

```sh
sudo pkg install DDNet
```

Windows (Scoop)
```cmd
scoop bucket add games
scoop install games/ddnet
```

## Benchmarking

Detailed instructions can be found in [`docs/BENCHMARKING.md`](docs/BENCHMARKING.md).

## Working with the official DDNet Database

Detailed instructions can be found in [`docs/DATABASE.md`](docs/DATABASE.md).

## Debugging

Detailed instructions can be found in [`docs/DEBUGGING.md`](docs/DEBUGGING.md).

## Better Git Blame

First, use a better tool than `git blame` itself, e.g. [`tig`](https://jonas.github.io/tig/). There's probably a good UI for Windows, too. Alternatively, use the GitHub UI, click "Blame" in any file view.

For `tig`, use `tig blame path/to/file.cpp` to open the blame view, you can navigate with arrow keys or kj, press comma to go to the previous revision of the current line, q to quit.

Only then you could also set up git to ignore specific formatting revisions:

```sh
git config blame.ignoreRevsFile formatting-revs.txt
```

## (Neo)Vim Syntax Highlighting for config files

Copy the file detection and syntax files to your vim config folder:

```sh
# vim
cp -R other/vim/* ~/.vim/

# neovim
cp -R other/vim/* ~/.config/nvim/
```
