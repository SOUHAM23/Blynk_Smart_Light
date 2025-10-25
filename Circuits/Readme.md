# **Blynk\_Smart\_Converter: IoT Smart Appliance Controller**

This repository contains the code and documentation for "Blynk\_Smart\_Light," a project that converts any standard AC appliance (like a light, fan, or outlet) into a "smart" device.

Using an ESP8266 microcontroller, a relay module, and the Blynk IoT platform, this project allows you to control your appliances from anywhere in the world using your smartphone. It also provides a foundation for integrating manual (physical) switch control alongside IoT functionality.

## **Features**

* **Remote Control:** Turn any AC appliance on or off remotely using the Blynk mobile app or web dashboard.  
* **Dual Control Ready:** The system can be controlled entirely via IoT or expanded to work in parallel with your existing manual wall switches.  
* **Cost-Effective:** Built with common and inexpensive components like the ESP8266 and a relay module.  
* **Simple & Lightweight:** Uses the BlynkSimpleEsp8266.h library for easy and stable integration with the Blynk server.

## **How It Works**

1. **Connectivity:** The ESP8266 connects to your local Wi-Fi network and authenticates with the Blynk cloud server using your unique Auth Token.  
2. **Blynk App:** A simple button widget in your Blynk project is tied to a Virtual Pin (e.g., V1).  
3. **IoT Control:** When you press the button in the app, Blynk sends a signal to the ESP8266. The ESP8266 code (using the BLYNK\_WRITE(V1) function) catches this signal and toggles a specific GPIO pin (e.g., D1) HIGH or LOW.  
4. **Relay Activation:** This GPIO pin is connected to the IN (signal) pin of the relay module. When the pin goes HIGH, it activates the relay, and when it goes LOW, it deactivates it.  
5. **Appliance Control:** The relay acts as a high-power automated switch. The "hot" wire of your AC appliance's power cord is cut and wired through the COM (Common) and NO (Normally Open) terminals of the relay.  
   * When the relay is **activated**, the circuit is closed, and the appliance turns **ON**.  
   * When the relay is **deactivated**, the circuit is open, and the appliance turns **OFF**.  
6. **Manual Control (Optional):** To add manual control, a physical switch (like your existing wall switch) can be wired to another GPIO pin on the ESP8266. The code can then be written to:  
   * Detect a state change from the physical switch.  
   * Toggle the relay's state.  
   * Update the Blynk app (Blynk.virtualWrite(V1, ...) ) to keep the app's button in sync with the physical switch's state.

## **Hardware Required**

* **Microcontroller:** 1 x ESP8266 (e.g., NodeMCU or WEMOS D1 Mini)  
* **Actuator:** 1 x 5V Single Channel Relay Module (ensure its AC rating matches your appliance and mains voltage)  
* **Appliance:** Any AC appliance you wish to control (e.g., a lamp, fan, coffee maker).  
* **Miscellaneous:**  
  * Jumper Wires  
  * A separate 5V power supply for the ESP8266 and relay (a good quality phone charger works well).  
  * AC power cord and socket (for creating a controllable extension cord) or access to your home's wiring.  
  * (Optional) A physical switch for manual control.

## **Software & Setup**

### **1\. Arduino IDE**

1. Make sure you have the Arduino IDE installed.  
2. Install the ESP8266 board definitions. You can find instructions by searching "ESP8266 Arduino IDE setup."  
3. Install the required Arduino libraries through the Library Manager (Sketch \> Include Library \> Manage Libraries...):  
   * Blynk by Volodymyr Shymanskyy (this package includes BlynkSimpleEsp8266.h).  
   * ESP8266WiFi (this is usually included with the ESP8266 board definitions).

### **2\. Blynk Setup**

1. Create a Blynk account at [blynk.cloud](https://blynk.cloud/) or through the Blynk IoT mobile app.  
2. Create a new Project (Template).  
3. In the template, define a **Datastream** (Virtual Pin) for your relay. For example:  
   * **Name:** "Relay Control"  
   * **Pin:** V1  
   * **Data Type:** Integer  
   * **Min:** 0, **Max:** 1  
4. Create a **Device** from your template to get your unique **Blynk Auth Token**, **Template ID**, and **Device Name**.  
5. In the web or mobile dashboard, add a "Switch" widget and assign it to the V1 datastream.

### **3\. Code Configuration**

1. Open the .ino (Arduino sketch) file included in this repository.  
2. Inside the code file, find and modify the following credentials at the top of the sketch:  
   * BLYNK\_TEMPLATE\_ID  
   * BLYNK\_DEVICE\_NAME  
   * BLYNK\_AUTH\_TOKEN  
   * ssid\[\] (Your Wi-Fi Name)  
   * pass\[\] (Your Wi-Fi Password)  
3. Ensure the RELAY\_PIN definition in the code matches the GPIO pin you have connected your relay to on the ESP8266.

### **4\. Wiring**

#### **DANGER: HIGH VOLTAGE**

Working with AC mains voltage (110V-240V) is extremely dangerous and can cause serious injury or death.

* **DISCONNECT ALL POWER** before making any connections.  
* Ensure all high-voltage connections are properly insulated and secured.  
* If you are unsure about any part of this, **STOP** and consult a qualified electrician.  
* Use this information at your own risk.

#### **1\. Low-Voltage Wiring (ESP8266 to Relay)**

* ESP8266 5V (VIN) \-\> Relay VCC  
* ESP8266 GND \-\> Relay GND  
* ESP8266 D1 (or your RELAY\_PIN) \-\> Relay IN

#### **2\. High-Voltage Wiring (Appliance to Relay)**

1. Take the AC power cord for your appliance.  
2. Identify the **"Hot"** wire (often Black in the US, Brown in the EU).  
3. **CAREFULLY** cut **ONLY** the "Hot" wire.  
4. Connect one end of the cut "Hot" wire to the COM (Common) terminal on the relay.  
5. Connect the other end of the cut "Hot" wire to the NO (Normally Open) terminal on the relay.  
6. The "Neutral" and "Ground" wires remain untouched and connected as normal.

*(This setup ensures the appliance is OFF by default and turns ON when the relay is activated).*

## **License**

This project is open-source. Feel free to modify and use the code. Please provide attribution if you share it.