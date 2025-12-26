### ไฟล์ : `main.c`

```cpp
#include <WiFi.h>
#include <HTTPClient.h>
#include <PubSubClient.h>
#include <ArduinoJson.h>
#include <time.h>

// ================== CONFIG (แก้ไขตรงนี้) ==================
const char* ssid     = "YOUR_WIFI_NAME";      // ชื่อ WiFi
const char* password = "YOUR_WIFI_PASS";      // รหัส WiFi
const char* server_ip = "192.168.1.XXX";      // IP ของคอมพิวเตอร์ที่รัน Docker (เช็คด้วย ipconfig/ifconfig)

// ตั้งค่า Port
const int   mqtt_port    = 1883;
const int   api_port     = 5000;

// ตั้งค่า Topic
const char* sub_topic  = "sensors/switch/switch-003";      // Topic รอรับคำสั่ง
const char* temp_topic = "sensors/temperature/esp32-003";  // Topic ส่งอุณหภูมิ
const char* hum_topic  = "sensors/humidity/esp32-003";     // Topic ส่งความชื้น

// Hardware
#define FAN_PIN 2   // พัดลม

// Objects
WiFiClient espClient;
PubSubClient mqttClient(espClient);
String apiUrl = "http://" + String(server_ip) + ":" + String(api_port) + "/database/write";

void connectWiFi() {
    Serial.print("Connecting WiFi");
    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
    }
    Serial.println("\nWiFi connected");
}

void connectMQTT() {
    while (!mqttClient.connected()) {
        Serial.print("Connecting MQTT...");
        if (mqttClient.connect("esp32-client-003")) { // Client ID ห้ามซ้ำ
            Serial.println("OK");
            mqttClient.subscribe(sub_topic); // เริ่มรอรับคำสั่ง
        } else {
            delay(2000);
        }
    }
}

// ฟังก์ชันรับคำสั่งจาก MQTT
void mqttCallback(char* topic, byte* payload, unsigned int length) {
    StaticJsonDocument<200> doc;
    deserializeJson(doc, payload, length);
    
    if (String(topic) == sub_topic) {
        int value = doc["value"] | -1;
        if (value == 1) {
            digitalWrite(FAN_PIN, HIGH); // เปิดพัดลม
            Serial.println("COMMAND: FAN ON");
        } else if (value == 0) {
            digitalWrite(FAN_PIN, LOW);  // ปิดพัดลม
            Serial.println("COMMAND: FAN OFF");
        }
    }
}

// ฟังก์ชันส่ง API (Log ลง Database)
void sendToAPI(const char* measurement, const char* val, const char* unit) {
    if (WiFi.status() != WL_CONNECTED) return;
    StaticJsonDocument<200> doc;
    doc["measurement"] = measurement;
    doc["tags"]["device_id"] = "esp32-003";
    doc["fields"]["value"] = ArduinoJson::serialized(val);
    doc["fields"]["unit"]  = unit;
    
    String body;
    serializeJson(doc, body);
    
    HTTPClient http;
    http.begin(apiUrl);
    http.addHeader("Content-Type", "application/json");
    http.POST(body);
    http.end();
}

void setup() {
    Serial.begin(115200);
    pinMode(FAN_PIN, OUTPUT);
    digitalWrite(FAN_PIN, LOW);
    connectWiFi();
    mqttClient.setServer(server_ip, mqtt_port);
    mqttClient.setCallback(mqttCallback);
}

void loop() {
    if (!mqttClient.connected()) connectMQTT();
    mqttClient.loop(); // สำคัญมาก! ต้องมีเพื่อรับข้อมูล

    static unsigned long lastRun = 0;
    if (millis() - lastRun >= 5000) { // ทุก 5 วินาที
        lastRun = millis();
        
        // จำลองค่า Temp (20.00 - 35.00)
        float temp = random(2000, 3500) / 100.0;
        char tempStr[10]; snprintf(tempStr, sizeof(tempStr), "%.2f", temp);
        
        // ส่ง MQTT
        String payload = "{\"value\": " + String(tempStr) + "}";
        mqttClient.publish(temp_topic, payload.c_str());
        Serial.println("Pub Temp: " + payload);

        // ยิง API (Optional)
        sendToAPI("temperature", tempStr, "celsius");
    }
}

```

[🔙 กลับสู่หน้า Project Guide](./04_project_guide.md#2-โปรแกรม-esp32-hardware-code)


