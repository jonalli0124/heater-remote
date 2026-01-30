<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Documentation | Miles Remote Heater Controller</title>
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; line-height: 1.6; color: #e0e0e0; background: #121212; margin: 0; padding: 0; }
        .container { max-width: 900px; margin: 40px auto; padding: 20px; background: #1e1e1e; border-radius: 12px; box-shadow: 0 4px 20px rgba(0,0,0,0.5); }
        h1, h2, h3 { color: #ffffff; border-bottom: 1px solid #333; padding-bottom: 10px; }
        h1 { color: #007bff; }
        code { background: #2d2d2d; padding: 2px 6px; border-radius: 4px; font-family: "SFMono-Regular", Consolas, monospace; font-size: 0.9em; color: #ff79c6; }
        pre { background: #2d2d2d; padding: 15px; border-radius: 8px; overflow-x: auto; border: 1px solid #444; }
        table { width: 100%; border-collapse: collapse; margin: 20px 0; background: #252525; }
        th, td { padding: 12px; text-align: left; border: 1px solid #333; }
        th { background: #333; color: #007bff; }
        .badge { display: inline-block; padding: 4px 10px; border-radius: 20px; font-size: 12px; font-weight: bold; margin-right: 5px; color: white; }
        .badge-pi { background: #c51a4a; }
        .badge-cloud { background: #f38020; }
        .badge-io { background: #00897b; }
        .status-table td:first-child { font-weight: bold; color: #aaa; }
    </style>
</head>
<body>

<div class="container">
    <h1>🌡️ Miles Remote Heater Controller</h1>
    <div style="margin-bottom: 20px;">
        <span class="badge badge-pi">Hardware: Raspberry Pi</span>
        <span class="badge badge-io">Cloud: Adafruit IO</span>
        <span class="badge badge-cloud">Tunnel: Cloudflare</span>
    </div>

    <p>A high-efficiency, cellular-connected IoT system designed to manage remote environmental climate and security. This project bridges physical hardware with a secure, real-time web dashboard.</p>

    <h2>1. Core Functionality</h2>
    <ul>
        <li><strong>Remote Power Control:</strong> Toggles a 120V space heater ON/OFF via a secure web dashboard using Adafruit IO MQTT.</li>
        <li><strong>Real-Time Telemetry:</strong> 
            <ul>
                <li><strong>Heater State:</strong> Live relay status confirmation.</li>
                <li><strong>Temperature:</strong> Raspberry Pi CPU thermals (convertible to ambient).</li>
                <li><strong>Cellular Signal:</strong> Real-time signal percentage (%) via AT commands.</li>
            </ul>
        </li>
        <li><strong>Visual Verification:</strong> Captures a 480p low-data photo every 4 hours (or on-demand) to verify physical environment safety.</li>
        <li><strong>Secure Remote Access:</strong> Integrates <code>cloudflared</code> to enable encrypted SSH access only when toggled ON from the dashboard.</li>
    </ul>

    

    <h2>2. Software & Cloud Architecture</h2>
    <table>
        <thead>
            <tr>
                <th>Service</th>
                <th>Role</th>
                <th>Cost Tier</th>
                <th>Credentials Used</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><strong>GitHub Pages</strong></td>
                <td><strong>The Frontend.</strong> Hosts the dashboard for mobile/desktop access.</td>
                <td>Free</td>
                <td>Public Repo URL</td>
            </tr>
            <tr>
                <td><strong>Adafruit IO</strong></td>
                <td><strong>The Nerve Center.</strong> Manages MQTT commands, data logging, and image storage.</td>
                <td>Free</td>
                <td>AIO Key, Username</td>
            </tr>
            <tr>
                <td><strong>Cloudflare</strong></td>
                <td><strong>The Gatekeeper.</strong> Provides secure Tunneling for remote SSH access.</td>
                <td>Free</td>
                <td>Tunnel Token / Systemctl</td>
            </tr>
        </tbody>
    </table>

    <h2>3. Hardware Stack</h2>
    <ul>
        <li><strong>Controller:</strong> Raspberry Pi (Optimized for Zero 2 W or Pi 4)</li>
        <li><strong>Modem:</strong> Soracom Onyx or similar LTE USB Modem (<code>/dev/ttyUSB2</code>)</li>
        <li><strong>Camera:</strong> Raspberry Pi Camera Module</li>
        <li><strong>Switching:</strong> 5V Relay Module (Signal on <strong>GPIO 17</strong>)</li>
    </ul>

    <h2>4. Data Economy (Monthly Estimate)</h2>
    <p>Designed for low-bandwidth cellular plans (Approx. <strong>~45 MB / Month</strong>):</p>
    <ul>
        <li><strong>MQTT Heartbeat:</strong> ~15 MB (1-minute intervals)</li>
        <li><strong>Photos:</strong> ~22 MB (30KB optimized JPEGs)</li>
        <li><strong>SSH Tunneling:</strong> ~5 MB (Idle overhead)</li>
        <li><strong>System Overhead:</strong> ~3 MB (NTP/DNS)</li>
    </ul>

    <h2>5. File Manifest</h2>
    <h3>On the Raspberry Pi</h3>
    <pre>
/home/miles/miles_daemon.py   # The "Brain" (Python Hardware Logic)
/etc/systemd/system/miles.service # The "Guardian" (Auto-start script)
    </pre>

    <h3>On GitHub</h3>
    <pre>
index.html  # The "Face" (HTML5/JS Dashboard with Chart.js & Paho MQTT)
    </pre>

    <p style="margin-top: 40px; font-size: 0.8em; color: #666; text-align: center;">
        Documentation generated for the Miles Heater Project &copy; 2026
    </p>
</div>

</body>
</html>
