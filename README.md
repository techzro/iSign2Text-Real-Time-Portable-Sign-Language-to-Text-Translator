# Silent-Voice: Sign Language Translator
Real-time Indian Sign Language (ISL) gesture recognition using Edge AI on the Arduino Nicla Vision  
On-device inference · Wi-Fi video streaming · Optional display mode

---

## 📌 Overview
**Silent-Voice** is a portable, real-time sign language translator powered entirely by **edge machine learning** running on the **Arduino Nicla Vision**.  
It recognizes **15 Indian Sign Language (ISL) gestures** using an **on-device object detection model** trained with **Edge Impulse**, and displays the gesture labels directly on:

- a **live Wi-Fi MJPEG video stream** accessible via a smartphone browser, or  
- an **optional display** connected to the Nicla Vision  

The entire inference pipeline runs **offline**, without any cloud or external computation.

---

## 📌 Features
- ✔ Real-time ISL gesture recognition  
- ✔ Entirely offline — no cloud required  
- ✔ Model trained using **MobileNetV2 0.35**  
- ✔ Input size **96×96**, **grayscale**, **INT8 optimized**  
- ✔ Wi-Fi MJPEG streaming using OpenMV  
- ✔ Optional display mode (TFT LCD / ST7789 / ILI9341)  
- ✔ 3,036-image custom dataset collected using Nicla Vision  
- ✔ Annotated with bounding boxes in Edge Impulse Labeling UI  
- ✔ Confusion matrix and validation metrics included  
- ✔ Works on any smartphone browser  

---

## 📌 Dataset Summary
**Total images:** 3,036  
**Classes:** 15 ISL gestures  
**Images per class:** ~200  
**Collected using:** Arduino Nicla Vision (OpenMV)  
**Annotated in:** Edge Impulse (manual bounding boxes)  
**Participants:** 4 subjects (ensures diversity)

### Class List:
`agree, angry, bad, come, fine, go, happy, hello, how, hungry, me, please, sorry, thank, you`


---

## 📌 Model Information
- **Model type:** MobileNetV2 0.35  
- **Impulse Input:** 96×96  
- **Color:** Grayscale  
- **Quantization:** INT8  
- **Validation Accuracy:** 93.1%  
- **Test Accuracy:** 90.17%  
- **Precision:** 0.96  
- **Recall:** 0.90  
- **F1 Score:** 0.93  
- **Inference Time:** 691 ms  
- **Peak RAM Usage:** 137.8 KB  
- **Flash Usage:** 81.9 KB  

---

## 📌 System Architecture
`Camera Frame → Model Inference → Gesture Bounding Box → Gesture Label Overlay → HTTP MJPEG Stream → Phone Browser (real-time output)`

Optional: `Camera Frame → Gesture Overlay → External Display`


---

## 📌 Project Structure

```
Silent-Voice/
│
├── README.md                    # Full documentation
├── LICENSE                      # MIT recommended
│
├── models/
│   ├── trained.tflite
│   ├── labels.txt
│   └── model_metadata.json
│
├── firmware/
│   ├── silent_voice_openmv.py   # Main inference + streaming script
│   ├── display_mode.py          # Optional display-connected script
│   └── wifi_credentials.txt     # Template
│
├── dataset/
│   ├── dataset_summary.md       # Class counts, methodology
│   └── samples/                 # (Optional few samples, dataset is private)
│
├── edge-impulse/
│   ├── impulse_screenshot.png
│   ├── confusion_matrix.png
│   ├── validation_metrics.png
│   └── project_readme.md        # Edge Impulse dashboard README
│
├── docs/
│   ├── architecture_diagram.png
│   ├── system_flowchart.png
│   ├── hardware_setup.png
│   └── methodology.pdf          # Optional
│
└── demo/
    ├── demo_video_link.txt
    ├── images/
    │   ├── hand_detection_sample.png
    │   ├── gesture_classification_sample.png
    └── streaming_screenshot.png
```

---

## 📌 Deployment (Nicla Vision + OpenMV)
### **1. Install OpenMV IDE**  
Download for Windows.

### **2. Flash Nicla Vision with latest OpenMV firmware**

### **3. Copy these files to Nicla Vision's storage:**
- `trained.tflite`  
- `labels.txt`  
- `silent_voice_openmv.py`  

### **4. Update Wi-Fi credentials**
Edit:
```python
SSID = "YourHotspotName"
KEY  = "YourPassword"

### **Run the script in OpenMV IDE**
### **6. Open the live video stream**

On your smartphone or laptop: http://<NICLA-IP>:8080

You will see:
-Live camera feed
-Gesture label drawn next to the detected sign

## 📌 Results

- 🎯 Stable real-time gesture recognition  
- 🎯 Robust performance across different users & lighting conditions  
- 🎯 Works instantly on any phone browser  
- 🎯 Accurate bounding box + gesture label overlay  
- 🎯 No app installation required  

---

## 📌 Demo

📺 **Video:** _Add your YouTube link here_  
📸 **Screenshots:** Available in `/demo/images/`  

---

## 📌 Edge Impulse Public Project

🔗 **Link:** _Add your EI public project link here_  

---

## 📌 Hardware Used

- Arduino Nicla Vision  
- USB-C Cable  
- Smartphone Hotspot  
- Optional: TFT LCD Display (ST7789 / ILI9341)  

---

## 📌 License

MIT License  

---

## 📌 Acknowledgements

- Edge Impulse  
- Arduino Nicla Vision  
- OpenMV  
- Dataset Volunteers  
