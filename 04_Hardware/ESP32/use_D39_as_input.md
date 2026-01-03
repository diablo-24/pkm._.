how to use gpio 39 input for network reset(ip reset: reset panel IP to default)

GPIO 39 on the ESP32 is one of the **input-only** pins and is suitable for sensing changes from logic level 0 (LOW) to 1 (HIGH) or vice versa. Here’s a guide to help you use GPIO 39 as an input:

---
### 🔧 **GPIO 39 Key Facts**
- **Input-only**: Cannot be used as an output.
- **No internal pull-up/pull-down resistors**: You must use **external** resistors if needed.
- **ADC capable**: Also usable as an analog input (ADC1_CH3), but that’s optional in your case.

---
### ✅ **Basic Wiring**
If you're detecting a digital signal (e.g., a button or external sensor), wire it as follows:
#### Option 1: Pull-down resistor (detect HIGH when pressed)

```plaintext
 VCC (3.3V)
   |
  [Button]
   |
GPIO39 ----[10kΩ Resistor]---- GND
```

- Button connects between VCC and GPIO39.
- A **10kΩ pull-down resistor** ensures GPIO39 reads LOW when the button is not pressed.
- When pressed, GPIO39 reads HIGH.

#### Option 2: Pull-up resistor (detect LOW when pressed)
```plaintext
 GND
   |
  [Button]
   |
GPIO39 ----[10kΩ Resistor]---- 3.3V
```

- Inverted logic: default is HIGH, goes LOW when pressed.
- Use this only if your signal source prefers pull-ups.

---
### 🧪 **Arduino Example Code**
```cpp
#define INPUT_PIN 39

void setup() {
  Serial.begin(115200);
  pinMode(INPUT_PIN, INPUT); // GPIO 39 is input-only, no INPUT_PULLUP available
}

void loop() {
  int value = digitalRead(INPUT_PIN);
  Serial.println(value); // Will print 0 or 1
  delay(200);
}
```
---
### ⚠️ Important Notes

1. **No internal pull resistors**: You must use external pull-up or pull-down.
2. **3.3V logic only**: Don’t input 5V directly to GPIO39; use a voltage divider or level shifter if needed.
3. **Input-only**: Don’t try `digitalWrite()` or `pinMode(..., OUTPUT)` with GPIO39.

---
If you tell me what you're trying to connect to GPIO39 (e.g. button, sensor, signal line), I can help you with more tailored wiring and logic.