# Speech Equalizer

A digital signal processing (DSP) tool for speech audio equalization. Uses **Butterworth bandpass filters**, **Short-Time Fourier Transform (STFT)** based spectrogram analysis, and **dB-to-linear gain conversion** to process and visualize audio in the frequency domain.

## Features

- **3-Band Parametric Equalizer** — Independently adjust Low (300–600 Hz), Mid (1000–2000 Hz), and High (2000–3400 Hz) frequency bands with ±10 dB gain control
- **4th-Order Butterworth Bandpass Filters** — Designed using `scipy.signal.butter` in second-order sections (SOS) format for numerical stability, applied via `scipy.signal.sosfilt`
- **Spectrogram Visualization (FFT-based)** — Uses `scipy.signal.spectrogram` which internally computes the **Short-Time Fourier Transform (STFT)** to show the frequency content over time, plotted in dB scale (`10 * log10`) with Gouraud shading
- **dB-to-Linear Gain Conversion** — Gain values specified in decibels are converted to linear scale using `10^(dB/20)` before being applied to the filtered signal
- **Peak Normalization** — Equalized output is normalized by dividing by the maximum absolute amplitude to prevent clipping
- **Stereo-to-Mono Conversion** — Multi-channel audio is averaged across channels to produce a mono signal for processing
- **Audio Format Conversion** — Automatic `.m4a` → `.wav` conversion via pydub (requires [FFmpeg](https://ffmpeg.org/download.html))
- **In-Notebook Audio Playback** — Uses `IPython.display.Audio` for listening to both original and equalized audio directly in Jupyter

## Signal Processing Pipeline

```
Input (.wav / .m4a)
       │
       ▼
 Stereo → Mono (channel averaging)
       │
       ▼
 ┌─────────────────────────────────────────┐
 │     3-Band Butterworth Bandpass Filter   │
 │                                         │
 │  Low Band   (300–600 Hz)   × gain_low   │
 │  Mid Band   (1000–2000 Hz) × gain_mid   │
 │  High Band  (2000–3400 Hz) × gain_high  │
 └─────────────────────────────────────────┘
       │
       ▼
 Band Summation (filtered bands added together)
       │
       ▼
 Peak Normalization (÷ max|amplitude|)
       │
       ▼
 Output (equalized_speech.wav)
       │
       ▼
 STFT Spectrogram (original vs equalized)
```

## Technical Details

### Butterworth Filter Design
Each frequency band uses a **4th-order Butterworth bandpass filter** designed with normalized cutoff frequencies (`f / (fs/2)`). The filter is represented in **second-order sections (SOS)** format rather than transfer function (`b, a`) form for improved numerical stability, especially at higher filter orders.

### Spectrogram Analysis (FFT)
The spectrogram is computed using `scipy.signal.spectrogram`, which segments the signal into overlapping windows and computes the **Fast Fourier Transform (FFT)** on each segment (Short-Time Fourier Transform). The power spectral density is converted to decibels via `10 * log10(Sxx)` and rendered with Gouraud shading for smooth interpolation.

### Gain Model
User-specified gains in decibels are converted to linear multipliers:

```
gain_linear = 10^(gain_dB / 20)
```

For example, +5 dB → ×1.778, −3 dB → ×0.708.

## Requirements

- Python 3.10+
- [FFmpeg](https://ffmpeg.org/download.html) (only needed for `.m4a` conversion)

## Setup

```bash
# Clone the repository
git clone https://github.com/Abdullah-Masood-05/Speech-Project.git
cd Speech-Project

# Install dependencies
pip install -r requirements.txt
```

## Usage

1. Place your `.wav` audio file in the project directory.
2. Open `main.ipynb` in Jupyter Notebook.
3. Update the `input_file` variable to point to your audio file.
4. Run the cell — audio playback widgets appear for both original and equalized audio.
5. Adjust the `gain` dictionary values (in dB) to customize equalization.
6. The equalized audio is saved as `equalized_speech.wav` and original vs. equalized spectrograms are plotted side-by-side.

## Project Structure

```
Speech-Project/
├── main.ipynb          # Main notebook — equalizer + spectrogram visualization
├── requirements.txt    # Python dependencies
├── .gitignore
└── README.md
```

## Dependencies

| Library | Purpose |
|---------|---------|
| `numpy` | Array operations, signal normalization |
| `scipy` | Butterworth filter design, SOS filtering, spectrogram (FFT) |
| `matplotlib` | Spectrogram plotting with dB colorbar |
| `soundfile` | WAV file I/O (reading/writing audio) |
| `pydub` | Audio format conversion (m4a → wav) |

## License

This project is provided for educational purposes.
