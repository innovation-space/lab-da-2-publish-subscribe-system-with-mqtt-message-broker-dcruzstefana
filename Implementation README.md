# Node-RED MQTT Publish–Subscribe System

This project demonstrates a **publish–subscribe architecture** using **Node-RED** and the **MQTT protocol**.  
The system simulates a **smart campus monitoring** scenario with multiple sensor publishers, a wildcard subscriber, and a real-time dashboard for data visualization.

---

## System Overview

The implementation consists of:
- Multiple MQTT publishers simulating sensor data
- A single MQTT broker for message routing
- A wildcard MQTT subscriber
- A Node-RED Dashboard for live visualization

The system follows a **loosely coupled architecture**, where publishers and subscribers do not directly interact with each other.

---

## MQTT Broker Configuration

- **Broker:** test.mosquitto.org  
- **Port:** 1883  
- **Protocol:** MQTT 3.1.1  

The broker is configured once in Node-RED and reused by all MQTT nodes.

---

## MQTT Topics and QoS Levels

| Component | Topic | QoS |
|---------|------|-----|
| Temperature Sensor | `campus/building1/temperature` | 1 |
| Air Quality Sensor | `campus/building1/air` | 1 |
| System Status | `campus/status` | 2 |
| Wildcard Subscriber | `campus/#` | 2 |

---

## Publishers

### Temperature Publisher
- Generates simulated temperature values
- Triggered periodically using an Inject node
- Publishes numeric data to `campus/building1/temperature`
- Uses **QoS 1** for at-least-once delivery

### Air Quality Publisher
- Simulates Air Quality Index (AQI) values
- Publishes data to `campus/building1/air`
- Uses **QoS 1**

### Status Publisher
- Publishes system status messages such as `"ACTIVE"`
- Uses **QoS 2** to ensure exactly-once delivery

---

## Subscriber and Wildcard Subscription

- A single MQTT In node subscribes to `campus/#`
- This wildcard subscription receives messages from all campus-related topics
- Messages are routed internally using switch nodes based on `msg.topic`

This demonstrates the **core publish–subscribe model**, where one subscriber can listen to multiple publishers.

---

## Message Filtering and Routing

- Switch nodes filter incoming messages by topic
- Temperature data is routed only to the temperature visualization
- Air quality data is routed only to the AQI visualization
- This ensures correct and isolated data handling for each dashboard component

---

## Dashboard Visualization

The Node-RED Dashboard provides real-time visualization of sensor data:

- **Temperature Gauge:** Displays live temperature values (°C)
- **Air Quality Gauge:** Displays live AQI values
- Dashboard updates automatically as MQTT messages arrive

The dashboard can be accessed at:
http://localhost:1880/ui


---

## Quality of Service (QoS) Demonstration

- **QoS 1** is used for sensor data to guarantee delivery at least once
- **QoS 2** is used for system status messages to guarantee exactly-once delivery
- This demonstrates how MQTT supports different reliability levels depending on data criticality

---

## How to Run the Project

1. Install Node-RED (installation done using docker container)
2. Install the `node-red-dashboard` package
3. Import the provided Node-RED flow JSON
4. Configure the MQTT broker (test.mosquitto.org, port 1883)
5. Click **Deploy**
6. Open the dashboard at `http://localhost:1880/ui`

---

## Screenshots

The repository includes:
- Node-RED flow screenshot
- MQTT debug log output
- Dashboard UI showing live data

---

## Conclusion

This project successfully demonstrates an MQTT-based publish–subscribe system using Node-RED, including multiple publishers, a wildcard subscriber, QoS differentiation, and real-time data visualization. The architecture highlights the scalability and decoupling benefits of MQTT for IoT systems.

---