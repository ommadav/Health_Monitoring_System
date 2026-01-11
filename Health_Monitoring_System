#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <OneWire.h>
#include <DallasTemperature.h>

// -------- Pins --------
const int KY039_PIN     = A0;  // KY-039 analog signal
const int ONE_WIRE_BUS  = 2;   // DS18B20 data pin
// I2C OLED uses A4 (SDA) and A5 (SCL)

// -------- OLED --------
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

// -------- DS18B20 --------
OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature sensors(&oneWire);

// -------- Pulse detection (for KY-039) --------
// Sampling at ~200 Hz for PPG
const uint16_t sampleIntervalMs = 5;
unsigned long lastSampleMs = 0;

float dcLevel = 0;         // low-pass (baseline)
float acSignal = 0;        // AC component
float absAvg = 0;          // running estimate of amplitude
float bpm = 0;
unsigned long lastBeatMs = 0;
bool prevAbove = false;

const float dcAlpha   = 0.95; // higher -> slower baseline
const float acAlpha   = 0.6;  // smoothing for AC
const float ampAlpha  = 0.98; // running amplitude estimate
const uint16_t minBeatMs = 300;  // 200 BPM ceiling
const uint16_t maxBeatMs = 2000; // 30 BPM floor

// Optional smoothing of BPM
float bpmEMA = 0;
const float bpmAlpha = 0.85;

float tempC = 0;

void setup() {
  Serial.begin(9600);

  sensors.begin();

  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    // Change to 0x3D if your OLED needs it
    while (1) {
      Serial.println(F("SSD1306 init failed"));
      delay(1000);
    }
  }

  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0,0);
  display.println(F("Health Monitor"));
  display.println(F("KY-039 + DS18B20"));
  display.display();
  delay(1200);
}

void loop() {
  unsigned long now = millis();

  // Sample KY-039 at fixed interval
  if (now - lastSampleMs >= sampleIntervalMs) {
    lastSampleMs = now;

    int raw = analogRead(KY039_PIN); // 0..1023
    // Estimate DC baseline
    dcLevel = dcAlpha * dcLevel + (1.0 - dcAlpha) * raw;
    // AC component (signal - baseline)
    float ac = raw - dcLevel;
    // Smooth AC to reduce noise
    acSignal = acAlpha * acSignal + (1.0 - acAlpha) * ac;
    // Track typical amplitude (abs average)
    absAvg = ampAlpha * absAvg + (1.0 - ampAlpha) * fabs(acSignal);

    // Dynamic threshold relative to amplitude
    float thresh = max(6.0f, 0.5f * absAvg);

    bool above = (acSignal > thresh);
    // Detect rising edge with refractory period
    if (!prevAbove && above) {
      unsigned long ibi = now - lastBeatMs; // inter-beat interval
      if (ibi > minBeatMs && ibi < maxBeatMs) {
        float instBpm = 60000.0 / ibi;
        if (bpmEMA == 0) bpmEMA = instBpm;
        bpmEMA = bpmAlpha * bpmEMA + (1.0 - bpmAlpha) * instBpm;
        bpm = bpmEMA;
        Serial.print(F("BPM: ")); Serial.println(bpm, 1);
      }
      lastBeatMs = now;
    }
    prevAbove = above;
  }

  // Read temperature (about once per second)
  static unsigned long lastTemp = 0;
  if (now - lastTemp >= 1000) {
    lastTemp = now;
    sensors.requestTemperatures();
    tempC = sensors.getTempCByIndex(0);
    Serial.print(F("Temp: ")); Serial.print(tempC, 2); Serial.println(F(" C"));
  }

  // Update OLED ~10 fps
  static unsigned long lastDisp = 0;
  if (now - lastDisp >= 100) {
    lastDisp = now;
    drawScreen();
  }
}

void drawScreen() {
  display.clearDisplay();

  // Title
  display.setTextSize(1);
  display.setCursor(0, 0);
  display.println(F("KY-039 Pulse + Temp"));

  // BPM big
  display.setTextSize(2);
  display.setCursor(0, 16);
  display.print(F("BPM: "));
  if (bpm > 0) display.print(bpm, 0);
  else display.print(F("--"));

  // Temp
  display.setTextSize(2);
  display.setCursor(0, 40);
  display.print(F("T: "));
  if (tempC != -127.00) display.print(tempC, 1);
  else display.print(F("--.-"));
  display.print((char)247);
  display.print(F("C"));

  display.display();
}
