# 🌡️ DHT11 Temperature & Humidity Monitor using STM32 Nucleo-F446RE

This beginner-friendly project reads temperature and humidity values from a **DHT11 sensor** and displays the readings via **UART** on a serial terminal. It uses an **STM32 Nucleo-F446RE** board and demonstrates how to interface sensors with a microcontroller using the **HAL (Hardware Abstraction Layer)** approach in STM32CubeIDE.

---

## 🧰 Hardware Requirements

To build this project, you'll need:

| Component                | Purpose                                           |
|--------------------------|---------------------------------------------------|
| STM32 Nucleo-F446RE      | Main microcontroller for controlling and reading  |
| DHT11 Sensor             | To measure temperature and humidity               |
| Jumper Wires             | For wiring connections between sensor and board   |
| Breadboard (optional)    | For prototyping and easier wiring                 |
| USB Cable                | Powers the board and enables programming/debugging|

---

## 🔌 Wiring the Sensor

Here’s how to connect the DHT11 to your Nucleo board:

| DHT11 Pin | STM32 Pin      | Explanation                                      |
|-----------|----------------|--------------------------------------------------|
| VCC       | 3.3V or 5V      | Powers the DHT11 (either voltage works)         |
| DATA      | PA1             | Connected to GPIO pin PA1 for data communication|
| GND       | GND             | Common ground                                    |

> ✅ **Tip**: Place a 4.7kΩ pull-up resistor between DATA and VCC to ensure stable sensor readings.

---

## 🛠️ Software Setup & Tools

### Required Tools:

- **STM32CubeIDE** – for coding, compiling, and flashing the board  
- **HAL Drivers** – manage GPIO, timing, and UART  
- **Serial Terminal** – to view readings (like Tera Term or PuTTY)

### Configuration Steps:

1. **GPIO Setup**:  
   - PA1 configured as Input with Pull-up  
2. **UART Setup**:  
   - UART enabled with baud rate set to 9600 bps (or modify as needed)  
3. **Timing Functions**:  
   - Use `HAL_Delay` and timer-based functions for reading data

---

## 📁 Project Structure

DHT11-Temperature-Humidity-Monitor/ ├── Core/ │ ├── Inc/ │ │ └── dht11.h // Function declarations and macros │ ├── Src/ │ │ └── dht11.c // DHT11 communication and decoding logic │ └── main.c // Main loop and UART output handling ├── .project files // Auto-generated STM32CubeIDE files └── README.md // Project documentation


---

## 🧪 How the Code Works

### 1. **Start Signal**
The MCU sends a LOW signal on PA1 for at least 18ms to wake the sensor.

### 2. **Sensor Response**
DHT11 pulls the line LOW and then HIGH briefly to acknowledge.

### 3. **Data Transfer**
The sensor sends 40 bits:
- 8 bits: Humidity integer
- 8 bits: Humidity decimal (DHT11 always sends 0)
- 8 bits: Temperature integer
- 8 bits: Temperature decimal (DHT11 always sends 0)
- 8 bits: Checksum (should equal sum of previous 4 bytes)

### 4. **Bit Timing**
Each bit is signaled by the length of HIGH pulse:
- ~26-28us → 0  
- ~70us → 1

### 5. **Data Parsing**
Code reads these timings and constructs bytes to convert into human-readable values.

### 6. **Output**
Results are printed via UART:
```bash
Temperature: 29°C
Humidity: 62%
```
⚙️ Code Explanation – How It Works
This section breaks down how the STM32 Nucleo-F446RE communicates with the DHT11 sensor to collect data and display it over UART.

1. 🟢 Start Signal
The microcontroller sends a LOW signal on GPIO pin PA1 for at least 18ms. This acts as a wake-up call for the sensor to begin communication.

2. 🟡 Sensor Response
Once the start signal ends, DHT11 responds by pulling the line LOW for ~80μs, then HIGH for another ~80μs — this handshake confirms the sensor is ready.

3. 📦 Data Transmission
The sensor sends 40 bits of data in this order:

8 bits: Humidity Integer

8 bits: Humidity Decimal (always 0 for DHT11)

8 bits: Temperature Integer

8 bits: Temperature Decimal (always 0 for DHT11)

8 bits: Checksum → (sum of the first 4 bytes)

4. ⏱️ Bit Timing
Each bit is encoded using pulse-widths:

LOW for ~50μs → Start of bit

Then:

~26–28μs HIGH → Bit = 0

~70μs HIGH → Bit = 1

Your STM32 code reads these pulse widths using precise timing functions to reconstruct the binary data from the sensor.

5. 🧠 Parsing & Validation
The 5 received bytes are stored and interpreted:

Temperature and humidity are extracted.

Checksum is calculated and compared to the 5th byte to ensure data integrity.

6. 🖨️ UART Output
Parsed data is printed to the serial terminal via UART:

bash
Temperature: 29°C
Humidity: 62%
💬 You can view this output using Tera Term, PuTTY, or any serial monitor at the configured baud rate.ots, or sensor error-handling to make this even more educational. I can help polish the comments in your `main.c` and `dht11.c` files too, if you'd like. You're building this the right way: hands-on and open for learning 💪
