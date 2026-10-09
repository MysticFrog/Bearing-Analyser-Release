# Bearing Analyser

Bearing Analyser is a Windows desktop tool for diagnosing rolling-element bearing faults
from vibration or acoustic recordings. It shows a live FFT spectrum, finds the peaks, and
matches them against the defect frequencies of the selected bearing (BPFO, BPFI, BSF and
FTF), including their harmonics and shaft-speed sidebands.

## Download and install

Get the latest installer from the [Releases](../../releases) page and run
`BearingAnalyser-Setup-<version>.exe`.

- **Just me** (the default) installs to
  `C:\Users\<you>\AppData\Local\Programs\Bearing Analyser` and needs no administrator
  rights.
- **Anyone who uses this PC** installs to `C:\Program Files\Bearing Analyser` and asks
  for an administrator password.
- You can choose a different folder in the installer.
- Installing over an earlier version keeps your settings, bearing libraries and licence.
- Uninstall from **Settings > Apps**. Settings, libraries and the licence are kept unless
  you choose to remove them.

For unattended installs: `/S` (silent), `/allusers`, `/D="C:\Folder"`, `/nodesktop`, and
`/uninstall`.

## Features

**Signal sources**
- Live input from any Windows audio device or supported plugin hardware
- Audio and video files (WAV, FLAC, MP3, OGG, M4A, MP4, MKV) with play, pause, seek and repeat
- Audio playback through any output device, following the Windows default, with its own volume control
- Record live input to FLAC

**Spectrum analysis**
- FFT sizes from 1,024 to 131,072 points, Hann or flat-top window, optional zero-padding
- Sub-bin peak frequency and amplitude estimation
- Adaptive peak detection against an estimated noise floor
- Peak-hold trace, with an option to hide the live trace
- Click any peak to label its frequency
- Amplitude scale that grows with the largest peak, or a fixed reference
- Automatic shaft RPM detection

**Filters and envelope analysis**
- High-pass, low-pass, notch and 3-band EQ filters, applied without phase shift
- Envelope (demodulation) spectrum for finding bearing fault repetition rates

**Bearing fault matching**
- Searchable bearing libraries; import from CSV, JSON or JSONL (see below) and edit models in the app
- Matched peaks highlighted against fault frequencies, harmonics and sidebands, with an
  option to hide the match markers
- Fault frequency overlays for several bearings at once
- Low, medium and high noise presets

**Noise reduction**
- Log-MMSE, Wiener or spectral-subtraction noise reduction, applied to the spectrum,
  the peak detection and the audio you hear
- Noise estimate from a captured section of the recording, a live capture, an adaptive
  floor tracker, or white and pink noise models
- Noise settings and FFT settings can be saved alongside a recording and load
  automatically when it is opened

**General**
- Dark, light and system themes
- Interface in English, Chinese, Japanese, Korean, Russian, French, Spanish, Italian and German

## Importing bearings

Open the **Library Manager** and choose **Import CSV...** or **Import JSON/JSONL...**. Each
file becomes a new library named after the file. Example files are installed in
`examples\bearing-import` in the installation folder, and are also in this repository's
[examples/bearing-import](examples/bearing-import) folder. The bearing values in them are
illustrative only; use the geometry and fault frequencies published by the bearing maker.

Each bearing needs its geometry, from which the fault frequencies are calculated. Fault
frequencies given in the file are used instead of the calculated ones.

| Field | CSV header | JSON key | Unit |
|---|---|---|---|
| Name | `Bearing Number` (or `Name`) | `name` | text |
| Number of rolling elements | `Number of Balls` (or `Rolling Elements`) | `rolling_elements` | whole number |
| Ball (roller) diameter | `BD mm` (or `Ball Diameter mm`) | `ball_diameter_mm` | mm |
| Pitch diameter | `Pitch mm` (or `Pitch Diameter mm`) | `pitch_diameter_mm` | mm |
| Contact angle | `Angle` (or `Contact Angle deg`) | `contact_angle_deg` | degrees |
| Outer race fault | `BPFO` | `manual_frequencies.bpfo` | see below |
| Inner race fault | `BPFI` | `manual_frequencies.bpfi` | see below |
| Ball spin | `BSF` | `manual_frequencies.bsf` | see below |
| Cage (train) | `FTF` | `manual_frequencies.ftf` | see below |
| Speed the fault values are for | - | `manual_frequencies_reference_rpm` | RPM |

### CSV

```csv
Bearing Number,Number of Balls,BD mm,Pitch mm,Angle,BPFO,BPFI,BSF,FTF
6205 Example,9,7.94,39.04,0,3.585,5.415,2.357,0.398
6310 Example,8,17.46,77.50,0,3.099,4.901,2.106,0.387
7205 Example,12,7.14,38.50,40,,,,
```

- The first line is the header. Headers are matched ignoring case, spaces and punctuation,
  and the columns may be in any order.
- Commas separate values and `.` is the decimal point. Fields cannot be quoted, so a name
  cannot contain a comma.
- The fault columns are **orders** (multiples of shaft speed), as bearing makers usually
  publish them. All four must be filled in for any to be used; leave all four empty to have
  them calculated.
- Rows missing a name or any geometry value are skipped.

### JSON

An array of bearings:

```json
[
  {
    "name": "6205 Example",
    "rolling_elements": 9,
    "ball_diameter_mm": 7.94,
    "pitch_diameter_mm": 39.04,
    "contact_angle_deg": 0.0,
    "manual_frequencies": { "bpfo": 3.585, "bpfi": 5.415, "bsf": 2.357, "ftf": 0.398 },
    "manual_frequencies_reference_rpm": 60.0
  },
  {
    "name": "7205 Example",
    "rolling_elements": 12,
    "ball_diameter_mm": 7.14,
    "pitch_diameter_mm": 38.5,
    "contact_angle_deg": 40.0,
    "manual_frequencies": null
  }
]
```

### JSON Lines

The same objects, one per line, with no surrounding brackets and no commas between lines:

```json
{"name":"6205 Example","rolling_elements":9,"ball_diameter_mm":7.94,"pitch_diameter_mm":39.04,"contact_angle_deg":0.0,"manual_frequencies":{"bpfo":3.585,"bpfi":5.415,"bsf":2.357,"ftf":0.398},"manual_frequencies_reference_rpm":60.0}
{"name":"7205 Example","rolling_elements":12,"ball_diameter_mm":7.14,"pitch_diameter_mm":38.5,"contact_angle_deg":40.0,"manual_frequencies":null}
```

In both JSON formats `manual_frequencies` is optional: leave it out or set it to `null` to
have the fault frequencies calculated. When given, all four values are required, in **Hz
at `manual_frequencies_reference_rpm`**, and they scale with the running speed. For
orders, as in the CSV, set the reference to `60`; for Hz measured or published at a known
speed, give that speed (for example `1480`).

## System requirements

- Windows 10 or 11 (64-bit)
- An audio input device or recording files to analyse

## Licensing

Bearing Analyser runs as a time-limited trial until a licence is installed. A licence is
for one PC and is a single purchase, not a subscription: **12 months** or **perpetual**.

1. Open the **About** window, choose the term under **Buy a licence**, and click
   **Buy online...** to pay securely through Stripe. (When the trial has ended, the same
   options are on the trial screen.)
2. Your licence file (`.blic`) is emailed to you, usually within a few minutes.
3. Click **Import Licence File...** to install it.

Once a licence is installed, the purchase options are hidden; they return when a 12-month
licence expires. Licence questions: licences@monkeyco.net.

## Settings and data

Settings (`settings.json`), bearing libraries (`libraries`) and the licence (`licence`) are
stored in the installation folder, for example
`%LOCALAPPDATA%\Programs\Bearing Analyser`. An all-users install under Program Files
cannot be written by ordinary accounts, so there they are stored in
`%LOCALAPPDATA%\MonkeyCo\Bearing Analyser` instead. Data from earlier versions, which used
that folder, is copied across the first time the new version runs.

Published by MonkeyCo.
