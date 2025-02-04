# Gun-dtection-system-project
 Developed a real-time gun detection system using computer vision techniques. The project leverages OpenCV to capture video feeds, process images, and detect objects (specifically guns) based on a pre-trained Haar cascade classifier. The application is intended for security purposes to detect firearms in a live environment and alert the user.
import os
import cv2
import imutils
import numpy as np

# Path to cascade file
file_path = r"C:\Users\Manoj kumar\Downloads\cascade.xml"

# Check if the file exists
if os.path.exists(file_path):
    print("File exists!")
else:
    print("File does not exist!")

# Load the cascade classifier
gun_cascade = cv2.CascadeClassifier(file_path)

# Check if cascade loaded properly
if gun_cascade.empty():
    print("Error loading cascade file")
else:
    print(f"Cascade file loaded successfully from: {file_path}")

# Start video capture
camera = cv2.VideoCapture(0)
firstFrame = None
gun_exist = None

while True:
    ret, frame = camera.read()
    if not ret:
        print("Failed to grab frame")
        break

    # Resize and convert to grayscale
    frame = imutils.resize(frame, width=700)
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Detect guns in the image
    gun = gun_cascade.detectMultiScale(gray, 1.3, 5, minSize=(100, 100))

    if len(gun) > 0:
        gun_exist = True

    # Draw a rectangle around the detected gun
    for (x, y, w, h) in gun:
        frame = cv2.rectangle(frame, (x, y), (x + w, y + h), (255, 0, 0), 2)

    # Skip the first frame
    if firstFrame is None:
        firstFrame = gray
        continue

    # Show the live feed
    cv2.imshow("Security feed", frame)
    key = cv2.waitKey(1) & 0xFF

    if key == ord('q'):
        break
                                   
if gun_exist:
    print("GUNS DETECTED")
else:
    print("GUN DID NOT DETECTED")

# Close the camera and all windows
camera.release()
cv2.destroyAllWindows()
