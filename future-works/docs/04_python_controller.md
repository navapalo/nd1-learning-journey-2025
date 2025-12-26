### ไฟล์ : `main_controller.py`

```python
import paho.mqtt.client as mqtt
import json
import requests

# Config
MQTT_BROKER = "localhost" # รันบนเครื่องเดียวกันใช้ localhost ได้
SUB_TOPIC = "sensors/temperature/esp32-003"
PUB_TOPIC = "sensors/switch/switch-003"
API_URL = "http://localhost:5000/database/write"

current_state = -1 # เก็บสถานะปัจจุบันเพื่อไม่ให้ส่งคำสั่งซ้ำ

def log_api(action, temp):
    try:
        data = {
            "measurement": "system_events",
            "tags": {"device": "controller", "action": action},
            "fields": {"trigger_temp": float(temp)}
        }
        requests.post(API_URL, json=data)
        print(f" >> Logged to DB: {action}")
    except:
        print(" >> API Error")

def on_message(client, userdata, msg):
    global current_state
    try:
        payload = json.loads(msg.payload.decode())
        temp = float(payload['value'])
        
        print(f"Temp: {temp:.2f}°C", end="")

        # === LOGIC การตัดสินใจ ===
        if temp > 30.0: # ร้อนเกิน
            if current_state != 1:
                print(" -> HOT! Turn ON")
                client.publish(PUB_TOPIC, json.dumps({"value": 1}))
                log_api("switch_on", temp)
                current_state = 1
            else: print(" (Already ON)")
            
        elif temp < 25.0: # เย็นแล้ว
            if current_state != 0:
                print(" -> COOL. Turn OFF")
                client.publish(PUB_TOPIC, json.dumps({"value": 0}))
                log_api("switch_off", temp)
                current_state = 0
            else: print(" (Already OFF)")
        else:
            print(" (Normal)")
            
    except Exception as e:
        print(f"Error: {e}")

# Start
client = mqtt.Client()
client.on_message = on_message
client.connect(MQTT_BROKER, 1883, 60)
client.subscribe(SUB_TOPIC)
print("Controller Started... Waiting for data.")
client.loop_forever()

```
[🔙 กลับสู่หน้า Project Guide](./04_project_guide.md##-4-สร้างส่วนควบคุม-python-controller)


