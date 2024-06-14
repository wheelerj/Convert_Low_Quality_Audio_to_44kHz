
# Audio Sample Rate Converter

## Purpose
This Python script is designed to convert low-quality audio MP3 files to a standard sample rate of 44,100 Hz, which is a common requirement for podcast uploads. By ensuring that all audio files meet this standard, the script helps maintain consistent audio quality across all podcast episodes.

## Requirements
- Python 3.x
- `ffmpeg`
- `pydub`
- `tqdm`

## Installation
Before running the script, you need to install the required dependencies. You can install `ffmpeg` and the necessary Python packages using the following commands:

```bash
!apt-get install -y ffmpeg
!pip install pydub tqdm
```

## Usage
1. **Clone the Repository**: First, clone this repository to your local machine.

```bash
git clone https://github.com/yourusername/audioconverter.git
cd audioconverter
```

2. **Set Up Directories**: Ensure you have two directories: one for the input MP3 files (`input`) and one for the output files (`output`). Update the paths in the script if your directories are different.

```plaintext
/content/drive/MyDrive/input
/content/drive/MyDrive/output
```

3. **Run the Script**: Execute the script to start converting your MP3 files.

```python
# Import necessary libraries
import os
from pydub import AudioSegment
from tqdm import tqdm

# Define the function to convert sample rates
def convert_sample_rate(raw_directory, pending_directory, target_sample_rate=44100):
    mp3_files = []
    for root, _, files in os.walk(raw_directory):
        for file in files:
            if file.endswith('.mp3'):
                mp3_files.append(os.path.join(root, file))
    
    for file_path in tqdm(mp3_files, desc="Converting audio files", unit="file"):
        file = os.path.basename(file_path)
        audio = AudioSegment.from_file(file_path)

        if audio.frame_rate < target_sample_rate:
            print(f'Converting {file} from {audio.frame_rate}Hz to {target_sample_rate}Hz...')
            converted_audio = audio.set_frame_rate(target_sample_rate)
            converted_file_path = os.path.join(pending_directory, file)
            converted_audio.export(converted_file_path, format='mp3')
            print(f'Saved converted file to {converted_file_path}')
        else:
            print(f'Skipping {file}, sample rate is {audio.frame_rate}Hz')

# Specify directories
mp3_directory1 = '/content/drive/MyDrive/input'
pending_directory = '/content/drive/MyDrive/output'

# Ensure the pending directory exists
os.makedirs(pending_directory, exist_ok=True)

# Run the conversion function
convert_sample_rate(mp3_directory1, pending_directory)
```

4. **Check the Output**: The converted MP3 files will be saved in the output directory you specified.

## Additional Information
- The script checks the sample rate of each MP3 file in the input directory. If the sample rate is less than 44,100 Hz, it converts the file to 44,100 Hz and saves it in the output directory.
- A progress bar is displayed to indicate the conversion progress of the files.
- Files that already have a sample rate of 44,100 Hz or higher are skipped and not converted.

## Contribution
Feel free to contribute to this project by opening issues or submitting pull requests. Any enhancements, bug fixes, or suggestions are welcome!

## License
This project is licensed under the MIT License. See the LICENSE file for more details.

## Contact
If you have any questions or need further assistance, please open an issue or contact the repository owner at [your.email@example.com].
