Color detection with Python OpenCV (real time computer vision: https://opencv.org/) 

# Real-Time Multiple Color Detection using OpenCV

This project demonstrates real-time multiple color detection using a webcam and OpenCV. The program captures video from the webcam and detects red, green, and blue colors in the video feed. It then draws rectangles around the detected colors and labels them accordingly.

## Prerequisites

- Python 3.x
- OpenCV
- NumPy

## Installation

1. Clone this repository:
    ```bash
    git clone https://github.com/yourusername/color-detection.git
    cd color-detection
    ```

2. Install the required Python packages:
    ```bash
    pip install numpy opencv-python
    ```

## Usage

1. Run the script:
    ```bash
    python color_detection.py
    ```

2. The program will start capturing video from your webcam and display the video feed with detected colors. To quit the video stream, press the 'q' key.

## Code Explanation

The code performs the following steps:

1. **Capture Video from Webcam**:
    ```python
    webcam = cv2.VideoCapture(0)
    ```

2. **Process Each Frame**:
    ```python
    while True:
        _, imageFrame = webcam.read()
        hsvFrame = cv2.cvtColor(imageFrame, cv2.COLOR_BGR2HSV)
    ```

3. **Define Color Ranges and Masks**:
    ```python
    red_lower = np.array([136, 87, 111], np.uint8)
    red_upper = np.array([180, 255, 255], np.uint8)
    red_mask = cv2.inRange(hsvFrame, red_lower, red_upper)
    
    green_lower = np.array([25, 52, 72], np.uint8)
    green_upper = np.array([102, 255, 255], np.uint8)
    green_mask = cv2.inRange(hsvFrame, green_lower, green_upper)
    
    blue_lower = np.array([94, 80, 2], np.uint8)
    blue_upper = np.array([120, 255, 255], np.uint8)
    blue_mask = cv2.inRange(hsvFrame, blue_lower, blue_upper)
    ```

4. **Apply Morphological Transformations**:
    ```python
    kernel = np.ones((5, 5), "uint8")
    red_mask = cv2.dilate(red_mask, kernel)
    green_mask = cv2.dilate(green_mask, kernel)
    blue_mask = cv2.dilate(blue_mask, kernel)
    ```

5. **Detect Contours and Draw Rectangles**:
    ```python
    contours, _ = cv2.findContours(red_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    for contour in contours:
        area = cv2.contourArea(contour)
        if area > 300:
            x, y, w, h = cv2.boundingRect(contour)
            imageFrame = cv2.rectangle(imageFrame, (x, y), (x + w, y + h), (0, 0, 255), 2)
            cv2.putText(imageFrame, "Red Color", (x, y), cv2.FONT_HERSHEY_SIMPLEX, 1.0, (0, 0, 255))
    ```

6. **Display the Resulting Frame**:
    ```python
    cv2.imshow("Multiple Color Detection in Real-Time", imageFrame)
    ```

7. **Exit on 'q' Key Press**:
    ```python
    if cv2.waitKey(10) & 0xFF == ord('q'):
        webcam.release()
        cv2.destroyAllWindows()
        break
    ```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgements

- OpenCV documentation and tutorials
- NumPy documentation

