

### **Project Name: Miles Remote Heater Controller**

#### **1. Core Functionality**

* **Remote Power Control:** Toggles a 120V space heater ON/OFF via a secure web dashboard accessible from any browser.
* **Real-Time Telemetry:** Displays live status of:
* **Heater State** (ON/OFF)
* **Temperature** (Currently Raspberry Pi CPU temp)
* **Cellular Signal Strength** (%)
* **System Vitality** (Last update time & IP address)


* **Visual Verification:** Captures and displays a low-resolution photo of the environment every 2 hours (or on demand) to verify the heater is physically responding.
* **Historical Data Logging:** Tracks temperature trends, user actions (who clicked what), and system reliability over a 24+ hour period.
* **Safety Automations:**
* **"Dead Man's Switch":** If the system loses power or crashes, you receive an email alert after 30 minutes (via Healthchecks.io).
* **Overheat Protection:** (Currently disabled in code) Logic exists to cut power if temperature exceeds 85°F.


* **Remote Maintenance:** A hidden "SSH" toggle allows you to open a secure maintenance tunnel to update code without physical access.

---

#### **2. Software & Cloud Services Architecture**

| Service | Role | Cost Tier | Credentials/Keys Used |
| --- | --- | --- | --- |
| **GitHub Pages** | **The Frontend.** Hosts the `index.html` dashboard. This is the "App" you load on your phone. | Free | Public Repo URL |
| **HiveMQ Cloud** | **The Messenger.** Uses MQTT protocol to send instant commands (ON/OFF) and receive live status updates (Temp/Signal) between the Pi and the Web. | Free | Broker URL, Port 8883 (Pi), Port 8884 (Web), User/Pass |
| **ThingSpeak** | **The Historian.** Stores data points every 10 minutes to generate the 3 graphs (Temp, Actions, Health). | Free | Channel ID, Write API Key (Pi), Read API Key (Web) |
| **Healthchecks.io** | **The Watchdog.** Expects a "heartbeat" ping from the Pi every 10 minutes. If it misses 2 pings (20 min grace), it alerts you. | Free | Unique Ping URL (UUID) |
| **Cloudflare Tunnel** | **The Backdoor.** Provides secure SSH access (`ssh miles@...`) over the cellular network without needing a static IP or port forwarding. | Free | `cloudflared` token (on Pi) |
| **Soracom** | **The Carrier.** Provides the cellular internet connection for the hardware. | Paid | SIM ICCID, APN Configuration |

---

#### **3. Hardware Stack**

* **Controller:** Raspberry Pi Zero 2 W
* **Modem:** Soracom Onyx LTE USB Modem
* **Camera:** Raspberry Pi Camera Module
* **Switching:** Relay Module (Connected to GPIO 17)
* **Power:** * Pi: USB Power Supply
* Heater: Wall Outlet (Switched via Relay)



---

#### **4. Data Economy (Monthly Estimate)**

* **Total Usage:** ~34 MB / Month
* **Breakdown:**
* **Live Controls (MQTT):** ~13 MB (Tiny packets every 30s)
* **Photos:** ~11 MB (One 30KB image every 2 hours)
* **History/Graphs:** ~5 MB (Data points every 10 mins)
* **Overhead:** ~5 MB (DNS, Time Sync, etc.)
* **SSH Tunnel:** 0 MB (Unless activated by user)



---

#### **5. File Manifest**

**On the Raspberry Pi (`/home/miles/`)**

1. **`mqtt_heater.py`**: The "Brain." A Python script that:
* Reads sensors (Temp, Signal).
* Controls GPIO (Relay).
* Connects to HiveMQ, ThingSpeak, and Healthchecks.
* Takes photos.


2. **`/etc/systemd/system/heater.service`**: The "Immortality Spell." Ensures the Python script auto-starts on boot and restarts if it crashes.

**On GitHub**

1. **`index.html`**: The "Face." A single file containing the HTML layout, CSS styling, and JavaScript logic to draw the graphs and buttons.

---

#### **6. Maintenance Reference**

* **To Update Code:** Toggle "SSH ON" -> Terminal: `ssh miles@admin.miles-remote.com`
* **To View Status:** Go to `https://[your-github-username].github.io/miles-remote/`
* **To Reset System:** Unplug power -> Plug back in (Script auto-starts).
