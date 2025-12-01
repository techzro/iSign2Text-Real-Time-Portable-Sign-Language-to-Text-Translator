# Run Silent-Voice: Sign Language Translator — Step-by-Step

Two ways to run the project are provided below.  
- **Method 1 — Quick (Edge Impulse, easiest)**: run inference from the Edge Impulse dashboard using the QR / browser link. Fast demo — **not very robust**.  
- **Method 2 — Robust (recommended)**: clone the repo and run the OpenMV script on your **Arduino Nicla Vision**. This runs fully on the device, streams MJPEG over Wi-Fi, and shows live labels on the video.

Use Method 1 for a quick demo. Use Method 2 for stable, repeatable use and competition demo.

---

## Method 1 — Quick: Run from Edge Impulse (Easiest)
1. Open this Edge Impulse project in your browser:  
   `https://studio.edgeimpulse.com/public/841431/live`

2. On the **Project Dashboard** (right side) find the **Run this model** panel.  
   - There will be a **QR code** and a button **Launch in browser**.

3. From your phone or laptop:
   - **Option A (phone):** Scan the QR code with your phone camera. Open the resulting link.
   - **Option B (laptop):** Click **Launch in browser**.

4. The browser will load the live inference demo page.  
   - Point your camera (or upload an image) and watch the model run inference and show predicted labels.  
   - This requires an internet connection and uses the Edge Impulse hosted demo. Good for quick verification.

> ⚠️ Notes:
> - This is the fastest path for reviewers, but results may be less robust than running the model locally (latency, network dependencies, webcam differences).
> - Use this method for quick checks and for the judges who want an immediate demo.

---

## Method 2 — Robust: Run Locally on the Arduino Nicla Vision (Recommended)
> This is the production/demo mode: model runs on device, streams MJPEG over Wi-Fi, displays detected sign labels on the stream.

### Requirements
- Arduino **Nicla Vision** with OpenMV-compatible firmware (the board must support OpenMV scripts)  
- **OpenMV IDE** installed (Windows)  
- USB cable (micro/USB-C depending on your board)  
- A smartphone or laptop to act as hotspot (or a Wi-Fi network)  
- Repo cloned locally, containing the `OpenMV/` folder with these files:
  - `main.py`  
  - `trained.tflite`  
  - `labels.txt`

---

### Step A — Clone repo & prepare files
1. Clone the GitHub repository:
   ```bash
   git clone https://github.com/<your-repo>/Silent-Voice.git
   cd Silent-Voice/OpenMV
    ```

2. Ensure the following files exist in `OpenMV/`:

  - `main.py`
  - `trained.tflite`
  - `labels.txt`

---

### Step B — Connect Nicla Vision & Copy Files
1. Connect your **Arduino Nicla Vision** to the PC via USB.
2. Open **OpenMV IDE**.
3. In the OpenMV file browser, **copy or drag-and-drop** the following files onto the Nicla Vision filesystem:

   - `main.py`  
   - `trained.tflite`  
   - `labels.txt`

> **Tip:** If device storage is nearly full, delete old files to free up space.

---

### Step C — Edit Wi-Fi Credentials
1. Open `main.py` (from the board or local folder) in **OpenMV IDE**.
2. Find the credentials section and update the SSID and password:

```python
SSID = "YourHotspotName"
KEY  = "YourHotspotPassword"
```
3. Save the changes to the device.

---

### Step D — Run the Script
1. In OpenMV IDE, open main.py from the Nicla Vision filesystem.
2. Click Run.

You should see console output similar to:
```
Trying to connect to "YourHotspotName"...
Trying to connect to "YourHotspotName"...
WiFi Connected  ('34.13.2.140', '255.2.255.0', '10.123.2.150', '56.3.252.10')
Waiting for connections...
Connected to 34.13.2.150:49354
```

**Important:**
  - The first IP in the tuple (e.g., 34.13.2.140) is the device IP.
  - The MJPEG streaming server listens on port 8080.

---

### Step E — View the Live Stream & Predictions
On any device connected to the same hotspot, open a browser and navigate to:
```
http://<NICLA-IP>:8080
```

**Example:**
```
http://34.13.2.140:8080
```

**You will see:**
  - Live MJPEG video streamed from Nicla Vision
  - Real-time bounding box and gesture label overlay

##  🛠 Troubleshooting & Tips

### ❌ No Wi-Fi Connection?
- Verify hotspot is active and the **SSID/password (PSK)** is correct.  
- Ensure your phone hotspot allows device connections  
  *(some phones restrict tethering by default)*.  
- Try using a different Wi-Fi network or router.  

---

### ❌ Device IP Not Reachable From Phone?
- Confirm both the **Nicla Vision** and your phone/laptop are on the **same hotspot**.  
- Restart the hotspot.  
- Disable firewall on laptop if accessing from a PC.  

---

### ❌ Model Fails to Load or `ml.Model(...)` Throws Memory Error
- Ensure `trained.tflite` is small enough to fit in the board memory.  
- Remove large/unused files from Nicla storage.  
- If still failing, try:
  - Loading model with `load_to_fb=False` *(requires small code change)*  
  - Rebuilding a **smaller model** from Edge Impulse  

---

### ❌ Video Too Slow or Choppy
- Lower JPEG quality (default is `quality=35`):  
  ```python
  cframe = img.to_jpeg(quality=25, copy=True)
  ```
  - Reduce frame resolution if required.
  - Ensure your hotspot provides enough bandwidth.

---

### ❌ Predictions Flicker Between Labels
  - Add smoothing (majority vote of last 3 predictions).
  - Increase the confidence threshold:
```
min_confidence = 0.8
```

---

### ❌ Socket Errors
  - Restart the script and Nicla Vision.
  - Ensure port 8080 is not blocked by your device or network.

---

### Example Quick Edits (in main.py)
**🔹 Update Wi-Fi Credentials**
```
SSID = "MyPhoneHotspot"
KEY  = "mypassword123"
```

**🔹 Adjust Confidence Threshold**
```
min_confidence = 0.8
```



**🔹 Reduce JPEG Quality to Lower Bandwidth**
```
cframe = img.to_jpeg(quality=25, copy=True)
```

---

### 🛑 How to Stop the Stream
  - In **OpenMV IDE**, press **Stop** or `CTRL + C` in the serial console.
  - To permanently disable streaming, **remove or rename** `main.py` from the Nicla Vision filesystem.

---