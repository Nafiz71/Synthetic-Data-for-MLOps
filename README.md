# 📡 Ruckus H350 Wi-Fi Synthetic Log Generator

Simulates **realistic syslog-style access point (AP) logs** for the **Ruckus H350 Wi-Fi 6 wall-plate AP** deployed in **Multi-Dwelling Unit (MDU)** environments (apartments, student housing, hotels).

---

## 🎯 Purpose & Objectives

This project aims to:
- ✅ Generate **synthetic yet realistic Wi-Fi logs** to mimic actual AP behavior under different scenarios.
- ✅ Simulate network behavior under **default** and **optimized** AP configurations.
- ✅ Model key networking scenarios like **weak signals**, **client roaming**, **channel congestion**, **band steering**, and **AP reboots**.
- ✅ Create structured logs (human-readable and machine-ingestible) for **ML Ops**, **performance testing**, or **digital twin simulation**.

---

## 🏗️ Framework Overview

The log generation process is built around **six foundational factors** that critically impact wireless performance:

| Factor               | Purpose                                                                 |
|----------------------|-------------------------------------------------------------------------|
| `RSSI`               | Signal strength – affects association/disassociation logs              |
| `SNR`                | Signal quality – key for disconnects, retries, throughput issues       |
| `Band Steering`      | Forces capable clients to prefer 5GHz over congested 2.4GHz            |
| `BSS Minrate`        | Filters low-efficiency clients, enabling high-throughput environments  |
| `Mobility`           | Simulates client roaming and signal fluctuations                       |
| `Channel Utilization`| Reflects congestion and peak usage scenarios                           |

---

## 🧪 Scenarios Modeled

| Scenario Name      | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| `Baseline MDU`     | 3–4 clients, default config, mixed-band usage, some weak signals            |
| `Optimized MDU`    | 3–4 clients, optimized config, high RSSI/SNR, 5GHz preference               |
| `Peak Hour Load`   | 10–12 clients, high channel utilization, disconnects and retries            |
| `Smart Density`    | 10–12 clients, optimized config, load-balanced & fair airtime              |
| `Ping-Pong Edge`   | 1–2 clients, mobile devices constantly roaming across APs                  |

---

## 🔧 Configuration Profiles

The system supports **two configuration profiles**:

### 🔹 Default
- No band steering  
- No minimum data rate enforced  
- No load balancing  

### 🔸 Optimized
- Band steering enabled (encourages 5GHz)
- BSS minrate enforced (e.g., 12 Mbps)
- Load balancing across APs

---

## 👥 Device Behavior Profiles

| Device Type   | Traits                                     | Band Preference |
|---------------|--------------------------------------------|-----------------|
| `iPhone`      | Mobile, 5GHz preference, high throughput    | 5GHz            |
| `Laptop`      | Stationary/mobile hybrid, stable, efficient | 5GHz            |
| `Printer`     | Static IoT device, low throughput           | 2.4GHz          |

Other device profiles like `SmartTV`, `Camera`, and `Thermostat` are included but excluded from some scenarios.

---

## 🛠️ How It Works

- Log generation is **event-driven** (association, disassociation, roam, system, warning).
- Devices are initialized with MAC addresses and behavior parameters (RSSI, SNR, TxRate).
- Configurations determine how devices interact with the AP (band steering, roaming, min rate filters).
- Logs are printed in **syslog-style** (human-readable) and can be exported to CSV/JSON for analysis.

---

## 📂 Example Log Output

```log
Jul 05 14:00:04 RuckusH350-Apt3B wlan0: STA 88:11:22:01:5A associated - RSSI=-62dBm Band=5GHz TxRate=300Mbps
Jul 05 14:00:30 RuckusH350-Apt3B roam: STA 88:11:22:01:5A roamed to RuckusH350-Hallway - RSSI=-67dBm Band=5GHz
Jul 05 14:01:12 RuckusH350-Apt3B wlan0: Warning - STA 88:11:22:01:5A weak signal (RSSI=-80dBm)
Jul 05 14:02:45 RuckusH350-Apt3B wlan0: STA 88:11:22:01:5A disconnected: weak signal RSSI=-84dBm
