# Raspberry-Pi-Configuration-Set-up & Gait Analysis

## 🛠️ Hardware Troubleshooting & Fix (DeskPi Case & Pi 4)

These were the problems I encountered during setup and the troubleshooting steps I took are listed below: 

If your Raspberry Pi 4 inside a DeskPi case fails to boot and displays a **solid red LED** and a **repeating 4-flash green LED pattern**, the internal bootloader firmware has lost synchronization with the microSD card's partition table.

### Step-by-Step Resolution
1. **Isolate Hardware:** Remove the Pi 4 from the DeskPi case. Test it bare to rule out internal ribbon cable connection issues.
2. **Flash Recovery Firmware:** Open **Raspberry Pi Imager** on your computer. Go to `Choose OS` -> `Misc Utility Images` -> `Bootloader (Pi 4 family)` -> `SD Card Boot`. Flash this to your SD card.
3. **Reset the Pi:** Insert the card into the bare Pi and power it on. Wait until the green LED blinks in a **steady, continuous heartbeat pattern**, then unplug the power.
4. **Flash the OS:** Reinsert the card into your computer and use Raspberry Pi Imager to flash a fresh copy of **Raspberry Pi OS (64-bit)**. 
5. **Reassemble Carefully:** Put the Pi back into the DeskPi case. Ensure the internal microSD dummy adapter sits perfectly flush and the mounting screws are only finger-tight to avoid flexing the board and breaking pin contact.

---

## 🏃‍♂️ Gait Analysis & Model Training

The Raspberry Pi 4 (64-bit OS) serves as the primary edge computing node for this project. It is utilized to:
* Run the Python-based data pipelines to **analyse and simulate human gait patterns**.
* Preprocess streaming sensor data and **train the Convolutional Neural Network (CNN) model** directly on-device for real-time pattern recognition.

The model trained and the codes are included in the repository :) 

