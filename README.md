# Speech Equalizer

A speech processing tool with a GUI-based 3-band audio equalizer. Load a `.wav` audio file, adjust frequency band gains via sliders, and visualize the before/after spectrograms.

## Features

- **3-Band Equalizer** — Adjust Low (300–600 Hz), Mid (1000–2000 Hz), and High (2000–3400 Hz) frequency bands
- **GUI Controls** — Tkinter-based interface with gain sliders (±10 dB)
- **Spectrogram Visualization** — Side-by-side comparison of original and equalized signal spectrograms
- **Format Conversion** — Automatic `.m4a` to `.wav` conversion via pydub (requires [FFmpeg](https://ffmpeg.org/download.html))
- **Normalized Output** — Equalized audio is peak-normalized to prevent clipping

## Requirements

- Python 3.10+
- [FFmpeg](https://ffmpeg.org/download.html) (only if converting `.m4a` files)

## Setup

```bash
# Clone the repository
git clone https://github.com/<your-username>/Speech-Project.git
cd Speech-Project

# Install dependencies
pip install -r requirements.txt
```

## Usage

1. Place your `.wav` audio file in the project directory.
2. Open `main.ipynb` in Jupyter Notebook.
3. Update the `input_file` variable to point to your audio file.
4. Run all cells — the equalizer GUI will open.
5. Adjust the Low, Mid, and High gain sliders, then click **Apply Equalization**.
6. The equalized audio is saved as `equalized_speech.wav` and spectrograms are displayed.

## Project Structure

```
Speech-Project/
├── main.ipynb          # Main notebook — equalizer with GUI and spectrogram plots
├── requirements.txt    # Python dependencies
├── .gitignore
└── README.md
```

## How It Works

1. The audio signal is loaded and converted to mono.
2. Three 4th-order Butterworth bandpass filters isolate the Low, Mid, and High frequency bands.
3. Each band is multiplied by a user-specified gain (converted from dB to linear scale).
4. The filtered bands are summed and peak-normalized.
5. The result is saved to disk, and spectrograms of both the original and equalized signals are plotted.

## License

This project is provided for educational purposes.
