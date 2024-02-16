<div align="center">
	<h1>uoscVLC</h1>
	<p>
		A pre-packaged custom fork of <a href="https://github.com/tomasklaen/uosc">uosc</a> that mirrors a VLC-style interface while using <a href="https://mpv.io">MPV player</a>.
	</p>
	<br/>
	<a href="https://user-images.githubusercontent.com/47283320/195073006-bfa72bcc-89d2-4dc7-b8dc-f3c13273910c.webm"><img src="https://github.com/tomasklaen/uosc/assets/47283320/9f99f2ae-3b65-4935-8af3-8b80c605f022" alt="Preview screenshot"></a>
</div>

Features:

-   UI elements hide and show based on their proximity to cursor instead of every time mouse moves. This provides 100% control over when you see the UI and when you don't. Click on the preview above to see it in action.
-   When timeline is unused, it can minimize itself into a small discrete progress bar.
-   Build your own context menu with nesting support by editing your `input.conf` file.
-   Configurable controls bar.
-   Fast and efficient thumbnails with [thumbfast](https://github.com/po5/thumbfast) integration.
-   UIs for:
    -   Selecting subtitle/audio/video track.
    -   [Downloading subtitles](#download-subtitles) from [Open Subtitles](https://www.opensubtitles.com).
    -   Loading external subtitles.
    -   Selecting stream quality.
    -   Quick directory and playlist navigation.
-   All menus are instantly searchable. Just start typing.
-   Mouse scroll wheel does multiple things depending on what is the cursor hovering over:
    -   Timeline: seek by `timeline_step` seconds per scroll.
    -   Volume bar: change volume by `volume_step` per scroll.
    -   Speed bar: change speed by `speed_step` per scroll.
    -   Just hovering video with no UI widget below cursor: your configured wheel bindings from `input.conf`.
-   Right click on volume or speed elements to reset them.
-   Transform chapters into timeline ranges (the red portion of the timeline in the preview).
-   And a lot of useful options and commands to bind keys to.

# How to install/update

Locate your MPV folder. It is typically located at `~/.config/mpv/` on Linux/MacOS or `\%APPDATA%\mpv\` on Windows. See the [Files section](https://mpv.io/manual/master/#files) in mpv's manual for more info.

> [!NOTE]
> If you have data in this folder already, please delete it or move it elsewhere (unless you know what you're doing?). This applies when updating from an older version of uoscVLC as well.

Extract the contents of the zip file found at [Releases](https://github.com/Anduril97/uoscVLC/releases) into the above-mentioned folder. Just the contents, your folder should look like this:

![MPV Config folder](https://github.com/Anduril97/MPVClean/assets/100987393/0d88f36e-0480-4127-a4ee-04dca91f871f)

That's it!
   
# Thumbnails

To enable thumbnails in timeline, install [thumbfast](https://github.com/po5/thumbfast). No other steps are necessary.

# Keyboard Shortcuts (basic)
This is not an extensive list; mpv has many more listed at [Keyboard Shortcuts](https://mpv.io/manual/master/#keyboard-control).
However, these are the custom shortcuts I made to resemble VLC's interface. You can change these however you like by editing the `inputs.conf` file.
```
spacebar -------- play/pause
right arrow ----- seek +3
left arrow ------ seek -3
shift+right ----- seek +30
shift+left ------ seek -30
m --------------- mute on/off
up arrow -------- add volume +5
down arrow ------ add volume -5
/ --------------- set volume to 100
[ --------------- playback speed -0.25
] --------------- playback speed +0.25
\ --------------- set playback speed to 1
t --------------- MPV window Stay on Top on/off (if you using Wayland this will not work, use Alt+Space and choose Always on top)
q --------------- quit (MPV default)
f --------------- toggle Fullscreen (MPV default)
right click ----- menu options
left click ------ Play/Pause; can also hold and drag window
double click ---- toggle fullscreen
```

## Navigation

These bindings are active when any **uoscVLC** menu is open (main menu, playlist, load/select subtitles,...):

-   `up`, `down` - Select previous/next item.
-   `left`, `right` - Back to parent menu or close, activate item.
-   `enter` - Activate item.
-   `esc` - Close menu.
-   `wheel_up`, `wheel_down` - Scroll menu.
-   `pgup`, `pgdwn`, `home`, `end` - Self explanatory.
-   `ctrl+f` or `\` - In case `menu_type_to_search` is disabled, these two trigger the menu search instead.
-   `ctrl+enter` - Submits a search in menus without instant search.
-   `ctrl+backspace` - Delete search query by word.
-   `shift+backspace` - Clear search query.
-   `ctrl+up/down` - Move selected item in menus that support it (playlist).
-   `del` - Delete selected item in menus that support it (playlist).
-   `shift+enter`, `shift+right` - Activate item without closing the menu.
-   `alt+enter`, `alt+click` - In file navigating menus, opens a directory in mpv instead of navigating to its contents.

Click on a faded parent menu to go back to it.

# Supported environments:

| Env | Works | Note |
|:---|:---:|---|
| Windows | ✔️ | |
| Linux (apt) | ✔️ | |
| Linux (flatpak) | ✔️ | |
| Linux (snap) | ❌ | We're not allowed to access commands like `curl` even if they're installed. (Or at least this is what I think the issue is.) |
| MacOS | ❌ | `(23) Failed writing body` error, whatever that means. |

## FAQ

#### Why is the release zip size in megabytes? Isn't this just a lua script?

We are limited in what we can do in mpv's lua scripting environment. To work around this, we include a binary tool (one for each platform), that we call to handle stuff we can't do in lua. Currently this means searching & downloading subtitles, accessing clipboard data, and in future might improve self updating, and potentially other things.

Other scripts usually choose to go the route of adding python scripts and requiring users to install the runtime. I don't like this as I want the installation process to be as seamless and as painless as possible. I also don't want to contribute to potential python version mismatch issues, because one tool depends on 2.7, other latest 3, and this one 3.9 only and no newer (real world scenario that happened to me), now have fun reconciling this. Depending on external runtimes can be a mess, and shipping a stable, tiny, and fast binary that users don't even have to know about is imo more preferable than having unstable external dependencies and additional installation steps that force everyone to install and manage hundreds of megabytes big runtimes in global `PATH`.

#### Why don't you have `uosc-{platform}.zip` releases and only include binaries for the concerned platform in each?

Then you wouldn't be able to sync your mpv config between platforms and everything _just work_.
