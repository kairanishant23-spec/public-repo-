# BioGuard 🛡️

**AI-Powered Biometric Liveness & Identity Verification System**

BioGuard is a state-of-the-art, browser-based biometric authentication system designed to detect and prevent spoofing attacks (such as printed photos, pre-recorded videos, and 3D masks). It ensures that the person in front of the camera is a **live human being** and verifies their identity against an enrolled biometric profile—all running **100% locally on the device** without the need for external servers or cloud processing.

---

## 🌟 Key Features

*   **Zero-Server Privacy Architecture**: All AI processing runs entirely within the user's browser. Biometric data never leaves the device, making it highly secure and GDPR-compliant by design.
*   **Remote Photoplethysmography (rPPG)**: Analyzes subtle micro-color changes in the user's skin under ambient light to detect a genuine human heartbeat (pulse). This makes it virtually impossible to spoof the system with a static photo or screen.
*   **Facial Geometry Matching**: Uses Google's MediaPipe FaceMesh to extract 468 facial landmark points in real-time (at 30fps) to map the unique geometry of the user's face and match it against their enrolled profile.
*   **Dynamic Challenge-Response**: Issues randomized behavioral prompts (e.g., "Smile," "Turn Head Left/Right") that require real-time physical interaction. This prevents attackers from bypassing the system using pre-recorded video loops.
*   **Blink Detection (Eye Aspect Ratio)**: Calculates the mathematical ratio of vertical and horizontal eye landmarks to detect natural blinking patterns, further ensuring live human presence.
*   **PDF Audit Reports**: Generates a tamper-proof, downloadable session report containing all biometric scores, session IDs, and the final access verdict for compliance and record-keeping.

---

## 🛠️ Technology Stack

BioGuard is built using vanilla web technologies, proving that complex AI systems can run smoothly on the edge.

*   **Frontend Interface**: Pure HTML5, CSS3, and Vanilla JavaScript.
*   **Computer Vision**: [MediaPipe FaceMesh](https://google.github.io/mediapipe/solutions/face_mesh.html) for real-time 3D facial landmark tracking.
*   **Signal Processing (DSP)**: 
    *   **Cooley-Tukey FFT (Fast Fourier Transform)**: Converts the raw PPG optical signal from the time domain into the frequency domain to isolate the exact cardiac peak frequency (BPM).
    *   **Biquad Bandpass Filter**: Cleans the raw optical signal by removing ambient noise and isolating the 45–180 BPM cardiac frequency band.
*   **Storage**: Browser `localStorage` for secure, on-device retention of the enrolled biometric geometric template.

---

## 🚀 Use Cases

*   **Banking & FinTech**: Secure KYC (Know Your Customer) onboarding, fraud-free account access, and high-value transaction approvals.
*   **Online Education**: Identity verification and continuous anti-cheating monitoring for remote examinations.
*   **Healthcare**: Securing access to patient portals and telemedicine platforms.
*   **Enterprise Security**: Employee attendance tracking and secure zero-trust system access.

---

## 📋 How It Works

1.  **Enrollment (Registration)**: The user looks at the camera. The system maps their facial geometry, extracts a mathematical signature of their face, and saves it locally on the device.
2.  **Verification Phase 1 (Liveness)**: During a login attempt, the system calculates the user's live heart rate via rPPG, tracks eye blinks, and measures facial stability to ensure they are a real person.
3.  **Verification Phase 2 (Challenge-Response)**: The user is prompted to perform a randomized action (like smiling). The system tracks the facial landmarks to verify the action was completed.
4.  **Verification Phase 3 (Matching)**: The live facial geometry is compared against the stored template from enrollment.
5.  **Verdict**: If all checks (Heartbeat + Blink + Challenge + Identity Match) pass, access is granted, and an audit report is generated.

---

## 💻 Running the Project Locally

Because this project relies on local camera access and advanced browser APIs, it must be served over a local web server (opening the HTML file directly via `file://` will block camera permissions due to browser security policies).

### Prerequisites
*   A modern web browser (Chrome, Edge, Firefox, or Safari).
*   A working webcam.
*   Python (or any basic local web server).

### Setup Instructions

1.  Clone this repository to your local machine.
2.  Open your terminal or command prompt and navigate to the project directory.
3.  Start a local HTTP server. For example, if you have Python installed, run:
    ```bash
    python -m http.server 8080
    ```
4.  Open your web browser and navigate to: `http://localhost:8080`
5.  Allow camera permissions when prompted by the browser.

---

## 🔒 Security & Privacy Notice

BioGuard is a **privacy-first** application. 
- It operates completely offline after the initial page load. 
- It does not connect to any external databases or APIs (other than fetching the open-source MediaPipe script).
- You can wipe your biometric enrollment data at any time by clearing your browser's local storage or clicking the "Reset" button within the app.

---

*Designed and developed as a comprehensive solution to modern biometric security challenges.*
