# Bearing Analyser

Bearing Analyser is a Windows desktop tool for diagnosing rolling-element bearing faults
from vibration or acoustic recordings. It shows a live FFT spectrum, finds the peaks, and
matches them against the defect frequencies of the selected bearing (BPFO, BPFI, BSF and
FTF), including their harmonics and shaft-speed sidebands.

## Download

Get the latest installer from the [Releases](../../releases) page and run
`BearingAnalyser-Setup-<version>.exe`. The installer does not need administrator rights;
it installs to `%LOCALAPPDATA%\MonkeyCo\Bearing Analyser`.

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

**Bearing fault matching**
- Searchable bearing libraries; import from CSV, JSON or JSONL and edit models in the app
- Matched peaks highlighted against fault frequencies, harmonics and sidebands
- Fault frequency overlays for up to several bearings at once
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

## System requirements

- Windows 10 or 11 (64-bit)
- An audio input device or recording files to analyse

## Licensing

Bearing Analyser runs as a time-limited trial until a licence is installed.

1. Open the **About** window. It shows this computer's machine ID and the licence status.
2. Click **Licence Generation Request...**, fill in your details and send the request.
3. When you receive your licence files, click **Import Licence File...** in the About
   window to install them.

## Settings and data

Settings and bearing libraries are stored in `%LOCALAPPDATA%\MonkeyCo\Bearing Analyser`.

Published by MonkeyCo.
