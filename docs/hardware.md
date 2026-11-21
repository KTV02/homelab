# Hardware

## Home server

| Component | Details |
|---|---|
| Board | Raspberry Pi 5, 8 GB RAM, ARM64 |
| OS | Debian 13 (Raspberry Pi OS base), booting from NVMe |
| System disk | 1 TB NVMe SSD (OS, applications, databases) |
| Fast data disk | 1 TB NVMe SSD (hot media tier, working data) |
| Capacity disk | 8 TB NAS-grade HDD (Seagate IronWolf) |
| Cloud storage disk | 2 TB HDD dedicated to OpenCloud |
| Additional disk | 1 TB HDD (spare capacity) |
| Offline backup disk | 1 TB portable SSD, connected only for backup runs |
| Network | Wired Gigabit Ethernet |

## Forest hut node

| Component | Details |
|---|---|
| Board | Raspberry Pi 3B, 32-bit OS (Debian 13 based) |
| Climate sensor | DHT20 / AHT20 temperature and humidity sensor over I²C |
| Motion sensor | PIR sensor on a GPIO input |
| Camera | Raspberry Pi Camera Module 3 Wide NoIR (12 MP, without IR filter) |
| Heating | Shelly smart relay (Gen2/Gen3) switching the heating, controlled through its local API |
| Network | Wi-Fi to the hut router |

## Why a Raspberry Pi?

- Very low idle power draw for a 24/7 machine
- PCIe on the Pi 5 makes NVMe storage viable, removing the SD card as a reliability risk
- ARM64 is well supported by Debian and almost every container image I need
- The constraint of 8 GB RAM forces deliberate resource planning (see [job coordination](services/job-coordinator.md))
