# 📡 Terminal → Server Communication  
> Part of the **WebSocket+JSON Protocol v2.8**  
> WebSocket Port: `7788` | Protocol: RFC6455 | Format: JSON (UTF-8 keys)

This section documents how biometric or access control terminals communicate with the central server using WebSocket+JSON. All keys are lowercase, and string values are UTF-8 encoded.

---

## 🔐 1. Register Terminal

### 📘 Description  
The terminal must first register itself with the server to establish identity and capability before any data (logs, users) is transmitted. This ensures secure and structured communication between terminal and cloud.

---

### 📤 Terminal Sends

```json
{
  "cmd": "reg",                        // Command type: registration
  "sn": "ZX0006827500",               // Serial number (unique device identifier)
  "cpusn": "123456789",               // CPU serial number (hardware ID)
  "devinfo": {
    "modelname": "tfs30",             // Terminal model
    "usersize": 3000,                 // Maximum number of users the device can store
    "fpsize": 3000,                   // Maximum number of fingerprints
    "cardsize": 3000,                 // Maximum number of RFID cards
    "pwdsize": 3000,                  // Maximum number of passwords
    "logsize": 100000,                // Log storage capacity
    "useduser": 1000,                 // Currently used user slots
    "usedfp": 1000,                   // Currently used fingerprint slots
    "usedcard": 2000,                 // Currently used card slots
    "usedpwd": 400,                   // Currently used password slots
    "usedlog": 100000,                // Total number of logs stored
    "usednewlog": 5000,               // Number of new logs pending sync
    "fpalgo": "thbio3.0",             // Fingerprint algorithm version (e.g., thbio1.0/thbio3.0)
    "firmware": "th600w v6.1",        // Firmware version
    "time": "2016-03-25 13:49:30",    // Current terminal time
    "mac": "00-01-A9-01-00-01"        // MAC address of the device
  }
}


📥 Server Responds (Success)
json
Copy
Edit
{
  "ret": "reg",                             // Response to 'reg' command
  "result": true,                           // Indicates registration success
  "cloudtime": "2016-03-25 13:49:30",       // Server time for synchronization
  "nosenduser": true                        // Tells terminal whether to auto-send user data
}
⚠️ Server Responds (Failure)
json
Copy
Edit
{
  "ret": "reg",                             // Response to 'reg' command
  "result": false,                          // Registration failed
  "reason": "did not reg"                   // Human-readable failure reason
}
📝 Notes
sn and cpusn must be unique and consistent per terminal device.

If nosenduser is true, the terminal should wait for the server to explicitly request user data.

Registration must be completed before logs or user data can be transmitted.

cloudtime helps synchronize the terminal’s internal clock with server time.
