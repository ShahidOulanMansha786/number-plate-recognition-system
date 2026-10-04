# Automatic Number Plate Recognition

This project reads car number plates from a video. It uses Python, YOLOv8, SORT, EasyOCR, and OpenCV.

## How It Works

1. **Find cars.** A pretrained YOLOv8 model finds cars, motorcycles, buses, and trucks in each frame.
2. **Track cars.** The SORT tracker gives each vehicle its own ID.
3. **Find plates.** A custom YOLOv8 model finds the license plates.
4. **Match plates to cars.** A plate belongs to a car if the plate box is inside the car box.
5. **Read the plate text.** The plate image is turned into grayscale and thresholded. Then EasyOCR reads the text.
6. **Fix the text.** Similar characters are corrected (for example, `O` and `0`) using the plate format.
7. **Fill missing frames.** Linear interpolation fills the frames where a car was not detected.
8. **Draw the result.** The final video shows the car box, the plate box, and the best plate text.

## Project Files

| File | What it does |
| --- | --- |
| `main.py` | Runs detection, tracking, and OCR. Saves the results to `test.csv`. |
| `util.py` | Helper functions: read the plate, fix the text, match a plate to a car, and write the CSV file. |
| `add_missing_data.py` | Fills missing frames and saves `test_interpolated.csv`. |
| `visualize.py` | Draws the results on the video and saves `out.mp4`. |
| `requirements.txt` | List of Python libraries. |

## Requirements

- Python 3
- The libraries in `requirements.txt`:
  - ultralytics
  - pandas
  - opencv-python
  - numpy
  - scipy
  - easyocr
  - filterpy

## Setup

1. Install the libraries:

   ```bash
   pip install -r requirements.txt
   ```

2. Download the SORT module from [this repository](https://github.com/abewley/sort). Put the `sort` folder in the project folder.

3. Download a sample video and name it `sample.mp4`. The project used [this video](https://www.pexels.com/video/traffic-flow-in-the-highway-2103099/).

4. Put the trained plate detector model here:

   ```
   models/license_plate_detector.pt
   ```

   The model was trained with YOLOv8 on the [Roboflow license plate dataset](https://universe.roboflow.com/roboflow-universe-projects/license-plate-recognition-rxg4e/dataset/4).

## How to Run

Run the files in this order:

```bash
python main.py
python add_missing_data.py
python visualize.py
```

After that, you will get these files:

- `test.csv`: the raw results
- `test_interpolated.csv`: the results with the missing frames filled
- `out.mp4`: the final video with the plate text

## Limits

- The plate format is fixed to 7 characters.
- Blurry plates, night videos, and other plate styles may not work well.
- EasyOCR runs on the CPU, so the code can be slow.

## Future Improvements

- Support more plate formats.
- Use a GPU to make it faster.
- Check the plate text across many frames to get a better result.