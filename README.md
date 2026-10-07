# Web nbtv

**Narrow-band television in your browser.** Turn a webcam, video or picture into a mechanical-television signal you can save as a WAV file, and watch NBTV recordings, or live signals from a microphone, on a virtual televisor. Everything runs in a single HTML file, so it works on Windows, macOS, Linux, ChromeOS, Android and iOS with nothing to install.

## Why this exists

This project is inspired by Dominic Beesley's **NBSC Player** and **ScrapeNBSC** recorder, available from the [NBSC downloads page](http://authorityfile.co.uk/NBSC/Home/Downloads). His software introduced NBSC, a clever way of adding NTSC-style colour to the Narrow-bandwidth Television Association's 32-line standard, and it remains one of the best tools for NBTV.

The last release of that software was in November 2018. It is a Windows-only .NET program, with a separate playback-only Java version from 2009. Nipkow is a fresh take on the same idea for today's devices: open a web page and it runs anywhere with a modern browser, including phones and tablets.

Nipkow is an independent project, not affiliated with Dominic Beesley, the NBSC site or the NBTVA. It was written from scratch using the published descriptions of the formats. No code from NBSC Player was used.

## Features

**Transmit**
- Use your camera, a video file or a still picture as the source
- Record the signal and download it as a WAV file (48 kHz, 16-bit)
- Optionally record sound on the second channel, from the camera's microphone or the video's audio
- Play the signal out of your speakers to send it to another device

**Receive**
- Play WAV or other audio files, looped if you like
- Listen live through a microphone or line input
- Automatic sync locking, black-level restoration and speed tracking
- Plays a file's soundtrack from the second channel

**Make a video**
- Convert an NBTV audio file to MP4, faster than real time
- Record whatever the televisor shows, live, straight to MP4
- Choose the televisor look or clean pixels, up to 1080 px

**Picture controls**
- Neon-lamp, grey or true-colour display
- Slit, square or graded aperture simulation
- Brightness, black level, saturation, hue, colour killer and notch filtering
- Portrait or wide picture, and mirroring

## Supported formats

| Format | Lines | Samples per line | Lines per second | Colour |
|---|---|---|---|---|
| NBTVA club standard | 32 | 48 | 400 | Black and white |
| NBTVA 48-line | 48 | 64 | 600 | Black and white |
| NBTVA 64-line | 64 | 96 | 800 | Black and white |
| Baird-style | 30 | 70 | 375 | Black and white |
| NBSC | 32 | 48 | 400 | 15 000 Hz subcarrier |
| X-NBSC | 32 | 48 | 400 | 15 006.25 Hz subcarrier |
| NBSC 64-line | 64 | 96 | 800 | 15 600 Hz subcarrier |

All formats run at 12.5 frames per second. Formats with the same number of lines will play on each other's settings, just without colour.

## How it works

**The signal.** A picture is scanned as vertical lines, each from bottom to top, working from right to left across the picture, the same order John Logie Baird used. Brightness becomes the height of the audio waveform. Each line ends with a short "blacker than black" sync pulse, except the last line of the frame. That missing pulse tells the receiver a new frame is starting.

**The receiver.** The decoder watches for sync pulses, measures the real line rate, and locks on even if a recording plays slightly fast or slow. It uses the sync pulses and the black gap after the last line to work out where black and white sit, so the picture holds steady when the volume changes. Each line is then drawn as a vertical strip on the televisor.

**Colour (NBSC).** NBSC adds colour the way American NTSC television did. Two colour-difference signals are carried on a 15 kHz subcarrier mixed in with the picture, and the whole first line of each frame carries a reference burst. The receiver locks its own oscillator to that burst every frame, then recovers the colour and combines it with the black-and-white picture. Black-and-white receivers just see a faint pattern, so the signal stays compatible.

**Video export.** The decoder produces one picture per frame, timed from the actual sync pulses. The browser's built-in WebCodecs encoder compresses them to H.264, and they are packaged as MP4.

## Using it

1. Open `index.html` in a recent browser, or visit your GitHub Pages address.
2. Choose a **Format** at the top.
3. To make a recording: choose **Use camera**, **Choose video** or **Choose picture**, press **Record**, then **Download WAV**.
4. To watch a recording: choose **Open audio file**.
5. To receive live: choose **Listen on microphone** and play a signal into your device. A cable from the sending device's headphone socket to the receiving device's microphone input works far better than speaker to air.
6. To make a video: use **Convert audio file to video**, or press **Record the televisor** while something is playing.

Test cards for checking playback are on the [NBSC downloads page](http://authorityfile.co.uk/NBSC/Home/Downloads).

## Putting it on GitHub Pages

1. Create a new repository and upload `index.html` and `README.md`.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/ (root)`, and save.
4. After a minute or so the app is live at `https://<your-username>.github.io/<repository-name>/`.

Camera and microphone access need a secure connection. GitHub Pages provides HTTPS, so this works there. When opening the file directly from disk, some browsers may block the camera or microphone.

## Browser support

| Feature | Chrome / Edge | Firefox | Safari |
|---|---|---|---|
| Transmit, receive, WAV export | Yes | Yes | Yes |
| Video export | Yes | Recent versions | Yes, but may be silent |

Video export downloads one small helper library, [mp4-muxer](https://github.com/Vanilagy/mp4-muxer) (MIT licence), from a public CDN the first time it's used. Everything else works offline.

## Known limitations

- **CCNC colour** isn't supported because no public description of how it encodes colour could be found. CCNC files play in black and white on the NBTVA 32-line setting.
- **NBSC filter variants:** NBSC Player's nine named filter combinations are simplified to two transmit options (colour detail, luminance notch).
- **Apertures:** three aperture shapes are simulated, compared with twelve in NBSC Player.
- **Matching other software's files:** colour from files made by other software may need a small **Hue** adjustment or **Reverse colour axis**, because some details of the colour burst aren't fully specified in the published description.
- **Wide mode** scan direction is based on the published description and may need the **Mirror** option for some files.
- **Baird 30-line mode** uses the modern missing-pulse sync rather than the original 1930s broadcast method.
- **Sync pulse widths** for the black-and-white formats are best estimates. The receiver measures pulses itself, so playback of other files is unaffected.

## Credits and further reading

- Dominic Beesley, [NBSC](http://authorityfile.co.uk/NBSC/Home/About): the NBSC colour format and the original NBSC Player
- [Narrow-bandwidth Television Association](https://www.nbtv.org.uk), for the 32-line club standard and decades of mechanical-television work
- John Logie Baird, who demonstrated the first working television in 1926
