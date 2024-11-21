# Connect to Eduroam using Raspberry Pi

This repository provides a step-by-step configuration to connect your Raspberry Pi to the **Eduroam** Wi-Fi network using **wpa_supplicant** and a custom `interfaces` configuration.

## Table of Contents

- [Introduction](#introduction)
- [Files Overview](#files-overview)
- [Configuration Steps](#configuration-steps)
- [Customizing Configuration](#customizing-configuration)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Introduction

Eduroam is a Wi-Fi service that allows students, researchers, and staff of participating institutions to connect to secure Wi-Fi networks globally. This repository provides two configuration files for setting up Eduroam on a **Raspberry Pi**: `interfaces` for network configuration and `wpa_supplicant.conf` for the Eduroam-specific settings.

---

## Files Overview

There are two essential configuration files in this repository:

1. **`interfaces`**: This file configures the network interfaces on your Raspberry Pi (eth0 and wlan0).
2. **`wpa_supplicant.conf`**: This file contains the Wi-Fi settings, including authentication methods for connecting to Eduroam.

### `interfaces` File :

```bash
auto lo
iface lo inet loopback

auto eth0
allow-hotplug eth0
iface eth0 inet manual

auto wlan0
allow-hotplug wlan0
iface wlan0 inet manual
wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
iface default inet dhcp
```

eth0: Configured for manual IP addressing (not used in this case but allows Ethernet fallback).
wlan0: Configured for wireless connectivity using wpa_supplicant.

### `wpa_supplicant.conf` File :
```bash
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=FR

network={
	ssid="eduroam"
	key_mgmt=WPA-EAP
	pairwise=CCMP
	eap=TTLS
	identity=YOUR_UNIVERSITY_ID
	anonymous_identity="anonymous@unilim.fr"
	password=YOUR_PASSWORD
	ca_cert="/home/pi/.cat_installer/ca.pem"
	altsubject_match="DNS:wifi-ttls.unilim.fr"
	phase2="auth=MSCHAPV2"
}
```
country: Set to France (FR), adjust according to your location.

ssid: Network name (eduroam).

eap: Set to TTLS for Eduroam.

identity: Your institution-specific username (replace YOUR_UNIVERSITY_ID).

password: Your Eduroam password (replace YOUR_PASSWORD).

ca_cert: Path to the certificate used for secure connection.

phase2: Specifies the secondary authentication phase (MSCHAPV2).


## Configuration Steps

### Clone the repository:
```bash
git clone https://github.com/yourusername/connecttoeduroamusingraspberrypi.git
cd connecttoeduroamusingraspberrypi
```

### Update the interfaces file:

#### Place the interfaces file in /etc/network/:
```bash
sudo cp interfaces /etc/network/
```

### Update the wpa_supplicant.conf file:

#### Place the wpa_supplicant.conf file in /etc/wpa_supplicant/:
```bash
sudo cp wpa_supplicant.conf /etc/wpa_supplicant/
```

#### Edit the wpa_supplicant.conf:

Open the file with a text editor and replace YOUR_UNIVERSITY_ID and YOUR_PASSWORD with your actual credentials.
```bash
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```
#### Reboot your Raspberry Pi:

    sudo reboot

After rebooting, your Raspberry Pi should automatically connect to the Eduroam network.
## Customizing Configuration

    Identity: Replace YOUR_UNIVERSITY_ID with your actual university or institution login (e.g., johndoe@university.edu).
    Password: Replace YOUR_PASSWORD with your actual Eduroam password.
    Country: Set the country code (country=FR) to match your region if you're outside of France.

## Troubleshooting

    Wi-Fi connection fails: Ensure your device supports WPA-EAP and TTLS, and check that the correct ca_cert file path is used.
    Error messages related to authentication: Double-check that the correct credentials are placed in the wpa_supplicant.conf file, especially the identity and password fields.

## License

This repository is licensed under the MIT License.

