# Sony WH-1000XM5: Improper Authentication Vulnerability Fixed in v2.4.1

## 0. Executive Summary

- This report details an authentication vulnerability identified in Sony's WH-1000XM5 product.

- CVSS:3.0/AV:A/AC:L/PR:N/UI:N/S:U/C:L/I:H/A:H

- We responsibly disclosed our findings to the Sony in Aug. 2024. On Oct. 31, 2024, Sony acknowledged the findings.

## 1. Vulnerability Type

[CWE-287](https://cwe.mitre.org/data/definitions/287.html)

## 2. Vulnerability Details

A malicious third party (hereinafter referred to as the attacker) can impersonate a device that has been paired with the WH-1000XM5, allowing the attacker's device to connect to the WH-1000XM5 even when it is not in pairing mode and without requiring any operation from the WH-1000XM5 user.

Upon examining the Bluetooth packets, it appears that there is a flaw in the authentication process during re-connection of the WH-1000XM5.

## 3. Affected Specification

[WH-1000XM5](https://electronics.sony.com/audio/headphones/headband/p/wh1000xm5-b) (Firmware version: v2.3.1 and earlier)

## 4. PoC

This section presents the devices and setup required for the PoC, as well as the steps to reproduce this vulnerability.

### 4.1. Notation

- Alice: Victim, Master
- Bob: Victim, Slave
- Mallory: Attacker

### 4.2. PoC Devices

**Alice's Specification**

| Manufacturer | Model            | Operation System | Driver                         | Bluetooth Version |
| ------------ | ---------------- | ---------------- | ------------------------------ | ----------------- |
| Microsoft    | Surface Laptop 4 | Windows 11 Home  | Intel(R) Wireless Bluetooth(R) | 5.1               |

**Bob's Specification**

| Manufacturer     | Model      | Bluetooth Version | Firmware Version |
| ---------------- | ---------- | ----------------- | ---------------- |
| Sony Corporation | WH-1000XM5 | 5.2               | 2.1.0            |

**Mallory's Specification**

| Model                  | Operation System | System | Debian version | Kernel version | BlueZ version | Bluetooth Manufacturer | Bluetooth Version |
| ---------------------- | ---------------- | ------ | -------------- | -------------- | ------------- | ---------------------- | ----------------- |
| Raspberry Pi 4 Model B | Raspberry Pi OS  | 32bit  | 11 bullseye    | 6.1            | 5.55          | Cypress Semiconductor  | 5.0               |

### 4.3. PoC Setup

Pair the WH-1000XM5 and Surface Laptop 4 in advance.

### 4.4. PoC Procedure

1. Spoof the Bluetooth address of the Raspberry Pi and the Bluetooth name of its adapter to match those of the Surface Laptop 4.
2. Set the Bluetooth adapter state of the Raspberry Pi to Discoverable.
3. Leave the WH-1000XM5 idle for a while or manually turn off its power.
4. Turn off the Surface Laptop 4.
5. Turn on the WH-1000XM5 (NOTE: Be careful not to press the power button for more than 5 seconds to avoid entering pairing mode).

If you follow these steps in order, the WH-1000XM5 will pair and connect to the Raspberry Pi despite not being in pairing mode. Please note that the Raspberry Pi has **never previously paired** with the WH-1000XM5.

The following image is the sequence diagram of this vulnerability.

<img src="https://github.com/user-attachments/assets/6adaba13-275a-46aa-a8c3-1f1292360271" height="75%"/>

### 4.5. PoC Packets

- [v2.1.0](./packets/WH-1000XM5_v2.1.0.pcapng)
- [v2.3.1](./packets/WH-1000XM5_v2.3.1.pcapng)

## 5. Responsible Disclosure

On August 6, 2024, we reported this vulnerability to Sony via HackerOne.

On October 31, 2024, Sony acknowledged the findings.

On February 27, 2025, a patch for this vulnerability was released as v2.4.1.

## 6. References

- https://www.sony.jp/headphone/update/ (Access: 4 March, 2025)
- https://www.sony.com/electronics/support/wireless-headphones-bluetooth-headphones/wh-1000xm5/software/00278246 (Access: 4 March, 2025)
- https://www.sony-asia.com/electronics/support/wireless-headphones-bluetooth-headphones/wh-1000xm5/software/00278245 (Access: 4 March, 2025)

## 7. Acknowledgements from Sony

- In Japanese

![image1](https://github.com/user-attachments/assets/6101582c-ac6f-4a8d-92bd-23876a470ad1)

- In English

![image2](https://github.com/user-attachments/assets/1892ed58-bf09-4fd8-b1d0-c08979bf9da6)


## 8. Notes

Discoverer: Keiichiro KIMURA from ES3, Kobe University (as of  4 March, 2025)

Collaborators: Prof. Hiroki KUZUNO, Prof. Yoshiaki SHIRAISHI, and Prof. Masakatu MORII 

This report has been selected as Sony's Hacker of the Month for March 2025 on HackerOne!

![image3](https://github.com/user-attachments/assets/0499ee18-e92e-4bee-a66f-13bd51f769a1)



