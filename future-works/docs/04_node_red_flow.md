### NODE-RED (JSON Flow):

```json
[
    {
        "id": "743335467df2f25b",
        "type": "tab",
        "label": "Flow 1",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "mqtt_in_temp_db2",
        "type": "mqtt in",
        "z": "743335467df2f25b",
        "name": "Sub Temp",
        "topic": "sensors/+",
        "qos": "0",
        "datatype": "json",
        "broker": "emqx-broker",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 380,
        "y": 260,
        "wires": [
            [
                "change_extract_temp",
                "ed09c3b92c028f6e"
            ]
        ]
    },
    {
        "id": "change_extract_temp",
        "type": "change",
        "z": "743335467df2f25b",
        "name": "Extract .value",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.value",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 580,
        "y": 260,
        "wires": [
            [
                "ui_gauge_temp",
                "ui_chart_temp",
                "cc2b3053203a4bbc"
            ]
        ]
    },
    {
        "id": "ui_gauge_temp",
        "type": "ui-gauge",
        "z": "743335467df2f25b",
        "name": "Temperature",
        "group": "group_dashboard2_esp32",
        "order": 1,
        "value": "payload",
        "valueType": "msg",
        "width": "0",
        "height": "0",
        "gtype": "gauge",
        "title": "Temperature",
        "alwaysShowTitle": false,
        "units": "",
        "icon": "",
        "prefix": "",
        "suffix": "",
        "segments": [
            {
                "from": "0",
                "color": "#5cd65c",
                "text": "",
                "textType": "label"
            },
            {
                "from": "30",
                "color": "#ffaa00",
                "text": "",
                "textType": "label"
            },
            {
                "from": "35",
                "color": "#ff0000",
                "text": "",
                "textType": "label"
            }
        ],
        "min": 0,
        "max": "50",
        "sizeThickness": "",
        "sizeGap": "",
        "sizeKeyThickness": "",
        "className": "",
        "x": 810,
        "y": 220,
        "wires": [
            []
        ]
    },
    {
        "id": "ui_chart_temp",
        "type": "ui-chart",
        "z": "743335467df2f25b",
        "group": "group_dashboard2_esp32",
        "name": "Temp History",
        "label": "Temperature History",
        "order": 2,
        "chartType": "line",
        "category": "temperature",
        "categoryType": "msg",
        "xAxisLabel": "",
        "xAxisProperty": "",
        "xAxisPropertyType": "timestamp",
        "xAxisType": "time",
        "xAxisFormat": "",
        "xAxisFormatType": "auto",
        "xmin": "",
        "xmax": "",
        "yAxisLabel": "",
        "yAxisProperty": "",
        "yAxisPropertyType": "property",
        "ymin": "10",
        "ymax": "40",
        "bins": "",
        "stackSeries": false,
        "pointRadius": "",
        "showLegend": true,
        "removeOlder": 1,
        "removeOlderUnit": "3600",
        "removeOlderPoints": "",
        "colors": [
            "#1f77b4",
            "#aec7e8",
            "#ff7f0e",
            "#2ca02c",
            "#98df8a",
            "#d62728",
            "#ff9896",
            "#9467bd",
            "#c5b0d5"
        ],
        "textColor": [
            "#666666"
        ],
        "textColorDefault": true,
        "gridColor": [
            "#e5e5e5"
        ],
        "gridColorDefault": true,
        "width": "0",
        "height": "0",
        "className": "",
        "interpolation": "linear",
        "x": 810,
        "y": 300,
        "wires": [
            []
        ]
    },
    {
        "id": "mqtt_in_hum_db2",
        "type": "mqtt in",
        "z": "743335467df2f25b",
        "name": "Sub Hum",
        "topic": "sensors/+",
        "qos": "0",
        "datatype": "json",
        "broker": "emqx-broker",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 380,
        "y": 380,
        "wires": [
            [
                "change_extract_hum"
            ]
        ]
    },
    {
        "id": "change_extract_hum",
        "type": "change",
        "z": "743335467df2f25b",
        "name": "Extract .value",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.value",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 580,
        "y": 380,
        "wires": [
            [
                "ui_gauge_hum",
                "f129248aa5ecbcec"
            ]
        ]
    },
    {
        "id": "ui_gauge_hum",
        "type": "ui-gauge",
        "z": "743335467df2f25b",
        "name": "Humidity",
        "group": "group_dashboard2_esp32",
        "order": 3,
        "value": "payload",
        "valueType": "msg",
        "width": "0",
        "height": "0",
        "gtype": "donut",
        "title": "Humidity",
        "alwaysShowTitle": false,
        "units": "",
        "icon": "",
        "prefix": "",
        "suffix": "",
        "segments": [
            {
                "from": "0",
                "color": "#0094ce",
                "text": "",
                "textType": "label"
            }
        ],
        "min": 0,
        "max": "100",
        "sizeThickness": "",
        "sizeGap": "",
        "sizeKeyThickness": "",
        "className": "",
        "x": 800,
        "y": 380,
        "wires": [
            []
        ]
    },
    {
        "id": "ui_switch_manual_db2",
        "type": "ui-switch",
        "z": "743335467df2f25b",
        "name": "Manual Control",
        "label": "LED Switch (Manual)",
        "group": "group_dashboard2_esp32",
        "order": 4,
        "width": "0",
        "height": "0",
        "passthru": true,
        "topic": "",
        "className": "",
        "onvalue": "1",
        "onvalueType": "num",
        "offvalue": "0",
        "offvalueType": "num",
        "x": 600,
        "y": 560,
        "wires": [
            [
                "func_format_json_db2"
            ]
        ]
    },
    {
        "id": "func_format_json_db2",
        "type": "function",
        "z": "743335467df2f25b",
        "name": "Format JSON",
        "func": "// รับค่า 1 หรือ 0 จาก Switch\n// แปลงเป็น JSON format ที่ ESP32 ต้องการ\nmsg.payload = {\n    \"value\": msg.payload\n};\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 810,
        "y": 560,
        "wires": [
            [
                "mqtt_out_switch_db2"
            ]
        ]
    },
    {
        "id": "mqtt_out_switch_db2",
        "type": "mqtt out",
        "z": "743335467df2f25b",
        "name": "Pub Switch",
        "topic": "sensors/switch/switch-003",
        "qos": "0",
        "retain": "false",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "emqx-broker",
        "x": 1010,
        "y": 560,
        "wires": []
    },
    {
        "id": "mqtt_in_switch_status",
        "type": "mqtt in",
        "z": "743335467df2f25b",
        "name": "Sub Switch Status",
        "topic": "sensors/+",
        "qos": "0",
        "datatype": "json",
        "broker": "emqx-broker",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 310,
        "y": 580,
        "wires": [
            [
                "change_extract_switch"
            ]
        ]
    },
    {
        "id": "change_extract_switch",
        "type": "change",
        "z": "743335467df2f25b",
        "name": "Extract .value",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.value",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 600,
        "y": 660,
        "wires": [
            [
                "ui_switch_manual_db2"
            ]
        ]
    },
    {
        "id": "cc2b3053203a4bbc",
        "type": "debug",
        "z": "743335467df2f25b",
        "name": "debug 1",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "false",
        "statusVal": "",
        "statusType": "auto",
        "x": 800,
        "y": 140,
        "wires": []
    },
    {
        "id": "f129248aa5ecbcec",
        "type": "debug",
        "z": "743335467df2f25b",
        "name": "debug 2",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "false",
        "statusVal": "",
        "statusType": "auto",
        "x": 800,
        "y": 460,
        "wires": []
    },
    {
        "id": "ed09c3b92c028f6e",
        "type": "debug",
        "z": "743335467df2f25b",
        "name": "debug 3",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "false",
        "statusVal": "",
        "statusType": "auto",
        "x": 620,
        "y": 60,
        "wires": []
    },
    {
        "id": "emqx-broker",
        "type": "mqtt-broker",
        "name": "EMQX Broker",
        "broker": "emqx",
        "port": "1883",
        "clientid": "node-red-client",
        "autoConnect": true,
        "usetls": false,
        "protocolVersion": "4",
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthPayload": "",
        "birthMsg": {},
        "closeTopic": "",
        "closeQos": "0",
        "closePayload": "",
        "closeMsg": {},
        "willTopic": "",
        "willQos": "0",
        "willPayload": "",
        "willMsg": {},
        "userProps": "",
        "sessionExpiry": ""
    },
    {
        "id": "group_dashboard2_esp32",
        "type": "ui-group",
        "name": "ESP32-003 Climate",
        "page": "page_dashboard2_home",
        "width": "6",
        "height": "1",
        "order": 1,
        "showTitle": true,
        "className": "",
        "visible": "true",
        "disabled": "false",
        "groupType": "default"
    },
    {
        "id": "page_dashboard2_home",
        "type": "ui-page",
        "name": "Home Automation",
        "ui": "8b91bd651150fbac",
        "path": "/home",
        "icon": "home",
        "layout": "grid",
        "theme": "theme_default",
        "breakpoints": [
            {
                "name": "Default",
                "px": "0",
                "cols": "3"
            },
            {
                "name": "Tablet",
                "px": "576",
                "cols": "6"
            },
            {
                "name": "Small Desktop",
                "px": "768",
                "cols": "9"
            },
            {
                "name": "Desktop",
                "px": "1024",
                "cols": "12"
            }
        ],
        "order": 2,
        "className": "",
        "visible": "true",
        "disabled": "false"
    },
    {
        "id": "8b91bd651150fbac",
        "type": "ui-base",
        "name": "UI Name",
        "path": "/dashboard",
        "appIcon": "",
        "includeClientData": true,
        "acceptsClientConfig": [
            "ui-notification",
            "ui-control"
        ],
        "showPathInSidebar": false,
        "headerContent": "page",
        "navigationStyle": "default",
        "titleBarStyle": "default",
        "showReconnectNotification": true,
        "notificationDisplayTime": 1,
        "showDisconnectNotification": true,
        "allowInstall": true
    },
    {
        "id": "theme_default",
        "type": "ui-theme",
        "name": "Default Theme",
        "colors": {
            "surface": "#ffffff",
            "primary": "#0094ce",
            "bgPage": "#eeeeee",
            "groupBg": "#ffffff",
            "groupOutline": "#cccccc"
        }
    },
    {
        "id": "25250dcab081d5ec",
        "type": "global-config",
        "env": [],
        "modules": {
            "@flowfuse/node-red-dashboard": "1.30.0"
        }
    }
]
```

[🔙 กลับสู่หน้า Project Guide](./04_project_guide.md##-3-สร้างหน้าจอควบคุม-node-red-flow)
