# konasute-sdvx-linux-script
Script for producing a working SDVX installation under Linux.

## Requirements

Should be usable on most common modern Linux distros. Requires `pipewire` for your audio server.

## Usage

Download the current release, extract it somewhere in your system, and execute `install-sdvx.sh` on a terminal.
Follow instructions as displayed - you likely will be asked to sudo to install the tooling (`wine`, `winetricks`, `msitools` and `curl`) through your package manager.

## Post-installation

After opening the launcher, the game should direct you to the login page on your browser if you haven't already logged in. The launcher then should notice that you don't have any of the game files, and ask if it's OK to open the updater; it should then light the UPDATE button, which you should press. Expect a 2 hour download because the bandwidth is throttled very hard for downloading the game files. Once it's done, you should be able to return to the main launcher and open the settings.

Under the audio settings tab (オーディオ設定), select the WASAPI Exclusive mode (`WASAPI (排他モード)`) option, set the latency buffer to 10ms, and select the buffering mode to timer (`タイマー駆動`).
Configure your controller under the key config tab (`キーコンフィグ`). You can either bind each button individually by double clicking each cell, or bind in one go using the top-left button.

**If some of your buttons do not register as you press them on your controller**, you will have to generate a remap string for your controller.
* Download the [SDL2 Gamepad Tool](https://generalarcade.com/gamepadtool/), run it, and then click the `Create A New Mapping` button.
* Follow the instructions on screen until you have gone through all the inputs of your controller - *including* turning each knob clockwise and counter-clockwise - then press the Skip button until it asks for the gamepad name. Don't change the name, just press OK. Then, press the `Copy Gamepad GUID` button.
* On your file explorer, navigate to `~/.local/share/applications`. Open the `sdvx.desktop` file in a text editor, and in the Exec field, insert, before the `wine` command, `SDL_GAMECONTROLLERCONFIG="string"`. Paste the string inside the double quotes, replacing the `string` text.
* The resulting Exec field should look like this: `Exec=env WINEPREFIX=/home/<user>/.local/share/wineprefixes/Konasute LANG=ja_JP PULSE_LATENCY_MSEC=10 SDL_GAMECONTROLLERCONFIG="030068abc0160000dc27000011010000,Gamepad,a:b0,b:b1,x:b2,y:b3,back:b4,start:b5,leftstick:b6,rightstick:b7,leftshoulder:b8,rightshoulder:b9,dpup:b10,platform:Linux,crc:ab68," wine "Z:\\home\\<user>\\Games\\SOUND VOLTEX EXCEED GEAR\\launcher\\modules\\launcher.exe" %u`

If you would like to experiment with even lower audio latency, you can try changing the `PULSE_LATENCY_MSEC` variable in the `~/.local/share/applications/sdvx.desktop` file to something lower; be warned that there is a certain point where audio may start producing crackles or not play at all. You may also try and change the game's latency buffer under the audio options tab.

## Feeling notes

Under Windows, I normally play at -11ms judge offset, 0ms draw offset. Under Linux, I have to play with -20ms judge offset, 0ms draw offset. I am able to play without feeling like there is a desync between song and buttons, and my criticals seem to be fairly balanced - managed to PUC a 16 under this setup.

The game is able to deduce if you are running it on a 120Hz monitor or higher under Linux. I run my LG Ultragear 35" at 165Hz, and while this normally would have the game slow down, and force software delay to keep the framerate, it seems to be very stable.
