# MQTT_Computer-network-system-performance

# Scenario

![Scenario Image](https://github.com/user-attachments/assets/1f995b81-a938-4042-b797-b2ee4af770e8)


## Scenario Description

This scenario uses a real IoT device (ESP32 + Ultrasonic) to objectively evaluate the time required to send MQTT packets under normal everyday conditions. The goal is to compare the packet sending time at three QoS levels (0, 1, 2) using different packet counts: 500, 1000, 5000, and 10,000 packets, with a delay of 100ms between each packet.

---

## System Components

The system consists of three main components, connected through different networks:

### a. IoT Device Node
- **Network Address:** 172.20.10.0/28 (specifically device 172.20.10.12)
- **Device:** An IoT car device that uses MQTT to send data.
- **Function:** Sends distance data from the Ultrasonic sensor as MQTT packets to the MQTT Broker on the HiveMQ Cloud platform.

### b. HiveMQ Cloud (MQTT Broker)
- **Platform:** HiveMQ Cloud, deployed on AWS (Amazon Web Services).  
  *Note:* The free tier does not allow customization of the MQTT Broker, so the status for evaluating dropped or lost packets cannot be viewed. In the paid version, customization is still not possible, but you gain access to view the number of incoming packets at a cost of $0.34/hour.
- **Role:** Serves as the communication intermediary (MQTT Broker) by receiving data from the IoT device and forwarding it to other clients.
- **Functions:**
  - Receives data from the IoT device (as either a subscriber or publisher).
  - Forwards data to other applications or client services (e.g., Node-RED).

### c. Node-RED and InfluxDB
- **Network Address:** 192.168.53.0/24 (specifically: 192.168.53.2)
- **Components:**
  1. **Node-RED:**
     - A visual programming tool that receives data from HiveMQ (MQTT Broker).
     - Processes or transforms the received data.
     - Connects with the InfluxDB database.
  2. **InfluxDB:**
     - A database optimized for time-series data.
     - Stores data from the IoT device for analysis and monitoring.
- **Function:** Node-RED retrieves data from the broker and transfers it into the InfluxDB database for long-term storage.

---

## Protocol and Data Flow

- The IoT device (172.20.10.12) sends data via MQTT to HiveMQ Cloud (broker on AWS).
- HiveMQ Cloud forwards the data to Node-RED (192.168.53.2) through MQTT.
- Node-RED processes the received data and stores it in InfluxDB for further analysis.
