# Resolve360

**Version 0.1.0 (alpha)** - a GPU 360 video reframer for DaVinci Resolve.

<img width="418" height="561" alt="Resolve360" src="https://github.com/user-attachments/assets/975ad002-b6cd-409e-9ece-10fde89b92fb" />

Resolve360 takes 2:1 equirectangular 360 footage and lets you aim a virtual camera at it with **Field Of View**, **Up/Down**, **Left/Right**, **Rotate** and **Distortion** controls, directly inside DaVinci Resolve. Every control can be keyframed, and it works on both the Edit page and the Fusion page.

> **This is an early alpha.** It has been tested on one setup (see [Tested on](#tested-on)). There are probably workflows we have not thought of, and bugs on Windows and Linux that we have not been able to check. **Testing and feedback are very welcome** - (see [Testing and feedback](#testing-and-feedback)). **Known Bugs** - There are some know bugs (see [Known Bugs](#bugs)) before posting a Issues request.

## Credits and why
- **KartaVR/Insta360** I have long used and been grateful for KartaVR and its Reframe360 Ultra tool by Andrew Hazelden (Kartaverse). This was originally the only way you could edit 360 video clips straight in Resolve. Andrew has stated that he is retiring his support of the tool. This latest version of Resolve broke my old path install so I tried to reinstall Reactor and the plug-in to no success at all. It seems like it was a macOS bug for Reactor Standalone. This sent me into a panic that I couldn't finish my video project on time! So descided to make another, no secondary install needed, light weight and functional that is how this has been born.

- **Behavior reference:** Insta360 Studio's Reframe tool. The look and feel of the Field Of View and Distortion controls was tuned by comparing Resolve360's output against Studio's at matching settings. I also want to further push this and hope we can get to direct importation of Insta360 footage like the GoPro reframe pluggin does for Adobe Premiere Pro.

## Tested on
- **DaVinci Resolve Studio 21.1.2** on **macOS 27.2** (Metal)

Not yet tested: Windows, Linux, NVIDIA/AMD GPUs, the free edition of Resolve, other Resolve versions, and footage from cameras other than Insta360. Reports from any of these are very useful.

## Install
You copy two files into Resolve's user Fusion folder and restart Resolve. Please note this is paths for macOS I am unclear how Linux distros or Windows files these Resolve folder structures

The macOS path is the one we tested. The Windows and Linux paths follow Blackmagic's usual defaults but are **untested**; if yours differs, please tell us.

1. Download the latest zip from the [Releases](https://github.com/1ichthys/Resolve360/releases) page and unzip it. (Or clone the repository: `git clone https://github.com/1ichthys/Resolve360.git`.)
2. Put `Reframe360.fuse` in `~/Library/Application Support/Blackmagic Design/DaVinci Resolve/Fusion/Fuses/`. In Finder, use Go → Go to Folder and paste that path.
3. Put `Reframe360.setting` in `~/Library/Application Support/Blackmagic Design/DaVinci Resolve/Fusion/Templates/Edit/Effects/`. 
**Create the Templates/Edit/Effects folders if they don’t exist.**
4. Restart DaVinci Resolve.

Future updates will keep the plug-in id so there should be no lost of keyframes or need to re-add effect with future builds.

## Features
- Field Of View (horizontal), Up/Down, Left/Right and Rotate controls, all keyframeable
- **Distortion** slider: 0 is a flat (rectilinear) view, about 1 is a stereographic "planet" look, and above 1 gives a round fisheye disc with black outside it
- Four preset buttons: Linear, Ultra, Wide and Fisheye (they set Field Of View and Distortion)
- Seam-safe sampling across the 360 degree wrap-around
- Automatically skips Resolve's letterbox or pillarbox bars when the clip scaling is set to **Fit**
- GPU accelerated, using Fusion's built-in compute kernel support (no separate SDK or build step)

## Use
- **Edit page:** open the Effects panel, find **Resolve360** under Effects, and drag it onto your 360 clip.
- **Fusion page:** add the tool from the Fuses category, or press Shift+Space and search for "Resolve360".

## Important: timeline scaling
Resolve scales a clip to the timeline *before* an Edit-page effect sees it. By default it may crop the clip, which throws away the left and right edges of the 360 picture and shows up as a visible seam.

If your timeline is not 2:1, set the clip's scaling to **Fit**:
- On the clip: Inspector -> Video -> Retime and Scaling -> Scaling -> **Fit**, or
- For the whole project: Project Settings -> Image Scaling -> Mismatched resolution files -> **Scale entire image to fit**.

Adding the effect through the Fusion page avoids this pre-scaling.
### Controls
| Control | What it does |
|---|---|
| Linear / Ultra / Wide / Fisheye | Presets that set Field Of View and Distortion |
| Field Of View | Horizontal field of view in degrees (1 to 179) |
| Up/Down | Looks up (positive) or down (negative) |
| Left/Right | Looks right (positive) or left (negative) |
| Rotate | Rolls the view, in degrees |
| Distortion | 0 = flat, about 1 = stereographic, above 1 = fisheye disc |
| Reset All | Resets the controls at the current frame (Fusion page version only; on the Edit page use Resolve's built-in reset arrow) |

**Known bugs** Resolve's built in reset button sets all controls back to default state but the timeline viewer does not change until a slider is adjusted.

## Known limitations
- Input should be 2:1 equirectangular video.
- If Resolve crops the clip before the effect runs (Fill scaling), the lost picture cannot be recovered by the plugin.
- Nothing is drawn beyond about 160 degrees from the view direction.
- If you see faint horizontal streaks exactly at the 360 degree seam, open `Resolve360.fuse`, find `float band = 0.55f;` and change it to `0.0f`. This turns off the optional seam brightness blend.
- Alpha software: expect rough edges.

## Testing and feedback
We are actively looking for testers. The most helpful things to try:
- Windows and Linux installs (do the files load, do the paths work?)
- NVIDIA and AMD GPUs
- The free edition of Resolve, and Resolve versions other than 21
- Footage from other 360 cameras and at other resolutions (5.7K, 8K)
- Odd timeline sizes and scaling modes

Please open an [issue](https://github.com/1ichthys/Resolve360/issues) (there is a template) and include your Resolve version and edition, operating system, GPU, where you used it (Edit or Fusion page), your clip and timeline resolution, and the scaling mode. If the effect fails to load, open the Fusion page, choose Workspace -> Console, and paste any red text. Reports of things that **work** are just as welcome as bug reports.

Resolve360 is an **independent implementation**. It is not a port, decompilation or reverse-engineering of KartaVR or Insta360 Studio, and it contains no code from either. It builds on the ideas of those tools.

## Disclaimer
Not affiliated with or endorsed by Blackmagic Design, Insta360 or the KartaVR / Kartaverse project. DaVinci Resolve, Fusion and Insta360 are trademarks of their respective owners. This software is provided as is, without warranty.

## License
Resolve360 is licensed under the GNU General Public License v2.0 (GPL-2.0). See [LICENSE](LICENSE).
