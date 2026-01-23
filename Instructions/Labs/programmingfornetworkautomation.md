# Programming for Network Automation

---

## Lab Assistant for Network Automation

### Lab 1.1 – Hello Network Engineer

**Coding assistant**

```
# lab1_1_hello_network.py
engineer_name = "YOUR_NAME"  # ให้ผู้เรียนเปลี่ยนเป็นชื่อท่าน
devices_count = 5

print("Hello Network Engineer,", engineer_name)
print("วันนี้เราจะจัดการ network devices จำนวน", devices_count, "ตัว")
# ใช้ f-string
print(f"{engineer_name} จะเริ่มเรียน Network Automation วันนี้!")
```
---

### Lab 1.2 – Manage list of IP Address

**Coding assistant**

```
# lab2_2_device_list.py
devices = [
    "10.0.0.1",
    "10.0.0.2",
    "10.0.0.3",
    "10.0.1.1",
    "10.0.1.2"
]

print("มี devices ทั้งหมด:", len(devices))
# แสดงทีละตัวพร้อมเลขลำดับ
index = 1
for ip in devices:
    print(f"Device {index}: {ip}")
    index += 1
```
---
### Lab 1.3 – Read file device.txt and create report

**Coding assistant**

```
# lab1_3_device_report.py
devices = []
with open("C:\\NetAutomation\\inventory\\deviceIP.txt") as f:
    for line in f:
        ip = line.strip()
        if ip:
            devices.append(ip)

print("พบ devices ทั้งหมด:", len(devices))

with open("C:\\NetAutomation\\inventory\\device_report.txt", "w") as report:
    report.write(f"Total devices: {len(devices)}\n")
    report.write("Device list:\n")
    index = 1
    for ip in devices:
        report.write(f"{index}. {ip}\n")
        index += 1

print("สร้างไฟล์ device_report.txt เรียบร้อยแล้ว")

```
---
### Lab 1.4 – Check connectivity

**Coding assistant**

```
# lab1_4_check_connect.py
import subprocess
import platform

def ping_device(ip_address, count=5, timeout_sec=2):
    system = platform.system().lower()

    if system == "windows":
        cmd = ["ping", "-n", str(count), "-w", str(timeout_sec * 1000), ip_address]
    else:
        cmd = ["ping", "-c", str(count), "-W", str(timeout_sec), ip_address]

    result = subprocess.run(cmd, capture_output=True, text=True)
    if result.returncode == 0:
        print(f"Device {ip_address} is reachable.")
    else:
        print(f"Device {ip_address} is NOT reachable.")

if __name__ == "__main__":
    ping_device("10.0.0.103")

```
---
### Lab 2 Create device inventory tool

**Coding assistant**

```
# lab2_device_inventory.py
"""
Device Inventory Tool
A Python module for managing network device inventories

This tool allows you to:
- Read device information from CSV files
- Format inventory data as JSON or YAML
- Add and remove devices from the inventory
- Use as a command-line tool or import as a module
"""

import csv
import json
import yaml  # PyYAML library for YAML formatting
import argparse  # For command-line argument parsing
import sys

# TASK 1 FUNCTIONS - Completed

def read_inventory(filename):
    """
    Reads device inventory from a CSV file and returns structured data.
    
    Args:
        filename (str): Path to the CSV inventory file
        
    Returns:
        list: List of dictionaries, each representing a device
    """
    inventory_data_list = []
    with open(filename, "r") as file:
        for row in csv.DictReader(file):
            inventory_data_list.append(row)
            
    return inventory_data_list

def get_device_data(inventory, device_name):
    """
    Retrieves the data for a specific device from an inventory list.

    This function iterates through a list of device dictionaries and returns
    the dictionary corresponding to the device with the matching name.

    Args:
        inventory (list): A list of dictionaries, where each dictionary
                          represents a device and is expected to have a "Name" key.
        device_name (str): The name of the device to search for within the inventory.

    Returns:
        dict or None: The dictionary containing the device's data if found,
                      otherwise None.
    """
    for device in inventory:
        if device["Name"] == device_name:
            return device
    return None


# TASK 2 FUNCTIONS - Formatting functions (not yet completed)

def format_inventory_json(inventory_data):
    """
    Converts inventory data to formatted JSON string.
    
    Args:
        inventory_data (list): List of device dictionaries
        
    Returns:
        str: JSON formatted string with proper indentation
    """
    inventory_json = json.dumps(inventory_data, indent=4)
    return inventory_json

# UNCOMMENT THE YAML FUNCTION - This function is similar to JSON formatting
# but converts to YAML format instead
'''
def format_inventory_yaml(inventory_data):
    """
    Converts inventory data to formatted YAML string.
    
    Args:
        inventory_data (list): List of device dictionaries
        
    Returns:
        str: YAML formatted string with proper indentation
    """
    inventory_yaml = yaml.dump(inventory_data, indent=4)
    return inventory_yaml
'''

# TASK 3 FUNCTIONS - Device management functions (not yet completed)

def add_device(inventory, new_device):
    """
    Adds a new device to the inventory list.
    
    Args:
        inventory (list): The current list of devices
        new_device (dict): Dictionary containing the new device details
    """
    # COMPLETE THE FUNCTION - append the new device to inventory
    pass

def save_inventory(filename, inventory):
    """
    Saves the inventory list back to a CSV file.
    
    Args:
        filename (str): The CSV file path to save to
        inventory (list): The inventory data to save
    """
    fieldnames = ["Name", "Management IP", "Username", "Password", "Description"]
    # COMPLETE THE FUNCTION - write inventory to CSV file
    pass

# UNCOMMENT THIS FUNCTION FOR TASK 3 - Command-line interface
'''
def setup_cli():
    """
    Sets up the command-line interface for the inventory tool.
    
    Returns:
        argparse.ArgumentParser: Configured argument parser
    """
    parser = argparse.ArgumentParser(description="Manage network device inventory")
    subparsers = parser.add_subparsers(dest="command")
    
    # Add subcommand for adding devices
    add_parser = subparsers.add_parser("add", help="Add a new device")
    add_parser.add_argument("--name", required=True, help="Device name")
    add_parser.add_argument("--ip", required=True, help="Management IP address")
    add_parser.add_argument("--user", required=True, help="Username")
    add_parser.add_argument("--password", required=True, help="Password")
    add_parser.add_argument("--desc", required=True, help="Device description")
    
    return parser
'''


if __name__ == "__main__":
    # Load inventory data
    inventory_data = read_inventory("C:\\NetAutomation\\inventory\\inventory.csv")
    print(f"Loaded {len(inventory_data)} devices from inventory")
    
    # Test getting specific device data
    r1_info = get_device_data(inventory_data, "R1-Core")
    if r1_info:
        print(f"Found device: {r1_info['Name']} at {r1_info['Management IP']}")
    else:
        print("Device not found")

```
### Task 2 convert format function เป็น JSON และ YAML

**Coding assistant**

```
# Test JSON formatting
    json_output = format_inventory_json(inventory_data)
    print("JSON Format:")
    print(json_output)
    print()
    
    # Test YAML formatting  
    yaml_output = format_inventory_yaml(inventory_data)
    print("YAML Format:")
    print(yaml_output)

```
### Task 3 Modify the Inventory

**Coding assistant**

```
def add_device(inventory, new_device):
    """
    Adds a new device to the inventory list.
    
    Args:
        inventory (list): The current list of devices
        new_device (dict): Dictionary containing the new device details
    """
    inventory.append(new_device)

def save_inventory(filename, inventory):
    """
    Saves the inventory list back to a CSV file.
    
    Args:
        filename (str): The CSV file path to save to
        inventory (list): The inventory data to save
    """
    fieldnames = ["Name", "Management IP", "Username", "Password", "Description"]
    with open(filename, "w", newline="") as file:
        writer = csv.DictWriter(file, fieldnames=fieldnames)
        writer.writeheader()
        writer.writerows(inventory)

```
**Coding assistant**

```
# Set up command-line interface
    parser = setup_cli()
    
    # Parse arguments
    args = parser.parse_args()
    
    # Handle no command provided
    if args.command is None:
        print(inventory_data)
        sys.exit(0)
    
    # Handle the "add" command
    if args.command == "add":
        # Create new device dictionary from arguments
        new_device = {
            "Name": args.name,
            "Management IP": args.ip,
            "Username": args.user,
            "Password": args.password,
            "Description": args.desc
        }
        
        # Add device to inventory
        add_device(inventory_data, new_device)
        
        # Save updated inventory
        save_inventory("C:\\NetAutomation\\inventory\\inventory.csv", inventory_data)
        
        # Print success message
        print(f"Device '{args.name}' added and saved!")

```
---
### Lab 3.1 – Connect to Device by ssh with netmiko

**Coding assistant**

```
# lab3_1_ssh_show.py
from netmiko import ConnectHandler

# เปลี่ยนข้อมูล device ให้ตรงกับอุปกรณ์ที่จะ access
device = {
    "device_type": "cisco_ios",
    "host": "xx.xx.xx.xx",
    "username": "xxxx",
    "password": "xxxxx",
    #"secret": "xxxx",  # ถ้ามี enable
}

def main():
    print("Connecting to device:", device["host"])

    # เชื่อมต่อ
    connection = ConnectHandler(**device)

    # เข้า enable mode ถ้าจำเป็น
    connection.enable()

    # รันคำสั่ง show version
    output = connection.send_command("show version")
    print("=== show version ===")
    print(output)

    # ปิด connection
    connection.disconnect()

if __name__ == "__main__":
    main()

```
---
### Lab 3.2 – Connect to multiple devices by ssh with netmiko by using csv file

**Coding assistant**

```
# lab3_2_ssh_multi.py

import csv
import os
from netmiko import ConnectHandler


def read_devices_from_csv(filename: str):
    devices = []
    with open(filename, mode="r", encoding="utf-8-sig", newline="") as f:
        reader = csv.DictReader(f)
        for row in reader:
            dev = {
                "device_type": "cisco_ios",
                "host": row["IP"].strip(),
                "username": row["Username"].strip(),
                "password": row["Password"].strip(),
                # ถ้าใน CSV มีคอลัมน์ Secret ให้ปลดคอมเมนต์ และอย่าลืมใส่ค่าในไฟล์ CSV
                # "secret": row.get("Secret", "").strip(),
            }
            devices.append(dev)
    return devices


def main():
    csv_file = r"C:\NetAutomation\inventory\deviceconnect.csv"
    log_dir = r"C:\NetAutomation\logs"
    os.makedirs(log_dir, exist_ok=True)

    devices = read_devices_from_csv(csv_file)
    summary = []

    for dev in devices:
        host = dev["host"]
        print(f"Connecting to {host} ...")

        connection = None
        try:
            # เชื่อมต่อ
            connection = ConnectHandler(**dev)

            # เข้า enable mode เฉพาะกรณีมี secret
            if dev.get("secret"):
                connection.enable()

            # รันคำสั่ง
            output1 = connection.send_command("show vrf")
            print("=== show vrf ===")
            print(output1)

            output2 = connection.send_command("show ip interface brief")
            print("=== show ip interface brief ===")
            print(output2)

            summary.append((host, "SUCCESS"))

        except Exception as e:
            print(f"Error on {host}: {e}")
            summary.append((host, "FAILED"))

        finally:
            if connection:
                connection.disconnect()

    # เขียนสรุปผล
    result_file = os.path.join(log_dir, "batch_result.txt")
    with open(result_file, "w", encoding="utf-8") as f:
        for host, status in summary:
            f.write(f"{host}: {status}\n")

    print(f"สร้างไฟล์ {result_file} เรียบร้อย")


if __name__ == "__main__":
    main()

```
---
### Lab 4.1 – Automate configuration change by use the same configure with multiple device

**Coding assistant**

```
# lab4_1_automate_sameconfig.py

import csv
import os
from netmiko import ConnectHandler

def read_devices_from_csv(filename: str):
    devices = []
    with open(filename, mode="r", encoding="utf-8-sig", newline="") as f:
        reader = csv.DictReader(f)
        for row in reader:
            dev = {
                "device_type": "cisco_ios",
                "host": row["IP"].strip(),
                "username": row["Username"].strip(),
                "password": row["Password"].strip(),
                # ถ้าใน CSV มีคอลัมน์ Secret ให้ปลดคอมเมนต์ และอย่าลืมใส่ค่าในไฟล์ CSV
                # "secret": row.get("Secret", "").strip(),
            }
            devices.append(dev)
    return devices


def main():
    csv_file = r"C:\NetAutomation\inventory\deviceconnect.csv"
    log_dir = r"C:\NetAutomation\logs"
    os.makedirs(log_dir, exist_ok=True)

    devices = read_devices_from_csv(csv_file)

    config_commands = [
        "logging host 10.1.1.1",
        "logging buffered 64000"
    ]

    summary = []

    for dev in devices:
        host = dev["host"]
        print(f"Connecting to {host} ...")

        connection = None
        try:
            # เชื่อมต่อ
            connection = ConnectHandler(**dev)

            # เข้า enable mode เฉพาะกรณีมี secret
            if dev.get("secret"):
                connection.enable()

            # รันคำสั่ง
            output = connection.send_config_set(config_commands)
            print(output)

            summary.append((host, "SUCCESS"))

        except Exception as e:
            print(f"Error on {host}: {e}")
            summary.append((host, "FAILED"))

        finally:
            if connection:
                connection.disconnect()

    # เขียนสรุปผล
    result_file = os.path.join(log_dir, "batch41_config_result.txt")
    with open(result_file, "w", encoding="utf-8") as f:
        for host, status in summary:
            f.write(f"{host}: {status}\n")

    print(f"สร้างไฟล์ {result_file} เรียบร้อย")

if __name__ == "__main__":
    main()

```
---
### Lab 4.2 – Automate configuration change by use the configure file with multiple device

**Coding assistant**

```
# lab4_2_automate_fileconfig.py

import csv
import os
from netmiko import ConnectHandler

def read_devices_from_csv(filename: str):
    devices = []
    with open(filename, mode="r", encoding="utf-8-sig", newline="") as f:
        reader = csv.DictReader(f)
        for row in reader:
            dev = {
                "device_type": "cisco_ios",
                "host": row["IP"].strip(),
                "username": row["Username"].strip(),
                "password": row["Password"].strip(),
                # "secret": (row.get("Secret") or "").strip(),
                "configfile": row["Configfile"].strip(),
            }
            # ถ้าอยากใช้ secret ให้ปลดคอมเมนต์ด้านบน และถ้ามีค่าว่างก็ลบทิ้ง
            if dev.get("secret") == "":
                dev.pop("secret", None)

            devices.append(dev)
    return devices

def main():
    csv_file = r"C:\NetAutomation\inventory\deviceconnect_config.csv"
    config_dir = r"C:\NetAutomation\configs"
    log_dir = r"C:\NetAutomation\logs"

    os.makedirs(config_dir, exist_ok=True)
    os.makedirs(log_dir, exist_ok=True)

    devices = read_devices_from_csv(csv_file)

    summary = []

    for dev in devices:
        host = dev["host"]
        print(f"Connecting to {host} ...")

        config_filename = dev["configfile"]
        config_path = os.path.join(config_dir, config_filename)

        if not os.path.isfile(config_path):
            msg = f"Config file not found: {config_path}"
            print(f"Error on {host}: {msg}")
            summary.append((host, "FAILED", msg))
            continue

        connection = None
        try:
            # ส่งเฉพาะพารามิเตอร์ที่ ConnectHandler ใช้
            conn_params = {k: v for k, v in dev.items() if k != "configfile"}
            connection = ConnectHandler(**conn_params)

            if conn_params.get("secret"):
                connection.enable()

            output = connection.send_config_from_file(config_file=config_path)
            print(output)

            summary.append((host, "SUCCESS", f"Applied {config_filename}"))

        except Exception as e:
            print(f"Error on {host}: {e}")
            summary.append((host, "FAILED", str(e)))

        finally:
            if connection:
                connection.disconnect()

    # เขียนสรุปผล
    result_file = os.path.join(log_dir, "batch42_config_result.txt")
    with open(result_file, "w", encoding="utf-8") as f:
        for host, status, detail in summary:
            f.write(f"{host}: {status} - {detail}\n")

    print(f"สร้างไฟล์ {result_file} เรียบร้อย")

if __name__ == "__main__":
    main()

```
---
### Lab 4.3 – Automate multiple devices Backup configure & Compliance check 

**Coding assistant**

```
# lab4_3_automate_backup.py

import csv
import re
from pathlib import Path
from typing import List, Dict

from netmiko import ConnectHandler

REQUIRED_CONFIG_PREFIXES = [
    "service timestamps log datetime",
    "logging buffered",
]

# คำสั่ง config ที่ต้องการให้เหมือนกันทุกเครื่อง
CONFIG_COMMANDS = [
    "service timestamps log datetime msec localtime show-timezone",
    "logging host 10.1.1.1",
    "logging buffered 64000",
]

def read_devices_from_csv(filename: str) -> List[Dict[str, str]]:
    devices = []
    with open(filename, mode="r", encoding="utf-8-sig", newline="") as f:
        reader = csv.DictReader(f)

        # ตรวจคอลัมน์ที่จำเป็น
        required_cols = {"IP", "Username", "Password"}
        if not reader.fieldnames or not required_cols.issubset(set(reader.fieldnames)):
            raise ValueError(
                f"CSV ต้องมีคอลัมน์ {sorted(required_cols)} แต่พบ {reader.fieldnames}"
            )

        for row in reader:
            dev = {
                "device_type": "cisco_ios",
                "host": row["IP"].strip(),
                "username": row["Username"].strip(),
                "password": row["Password"].strip(),
            }

            # รองรับ Secret (Enable password) แบบ optional
            secret = (row.get("Secret") or "").strip()
            if secret:
                dev["secret"] = secret

            devices.append(dev)

    return devices

def sanitize_filename(name: str) -> str:
    """ทำชื่อไฟล์ให้ปลอดภัย (กัน / \\ : * ? ฯลฯ)"""
    name = (name or "").strip()
    return re.sub(r'[\\/:*?"<>|]+', "_", name) or "UNKNOWN"

def check_compliance(config_text: str) -> List[str]:
    """คืนรายการ config ที่ขาด (เช็คแบบ prefix ต่อบรรทัด)"""
    lines = [ln.strip() for ln in config_text.splitlines() if ln.strip()]
    missing = []
    for prefix in REQUIRED_CONFIG_PREFIXES:
        found = any(ln.startswith(prefix) for ln in lines)
        if not found:
            missing.append(prefix)
    return missing

def get_hostname(conn, fallback_host: str) -> str:
    hostname_line = conn.send_command("show run | include ^hostname", read_timeout=20)
    hostname = fallback_host
    if hostname_line:
        for ln in hostname_line.splitlines():
            ln = ln.strip()
            if ln.startswith("hostname "):
                parts = ln.split()
                if len(parts) >= 2:
                    hostname = parts[1].strip()
                    break
    return hostname

def main():
    csv_file = r"C:\NetAutomation\inventory\deviceconnect.csv"
    log_dir = Path(r"C:\NetAutomation\logs")
    backup_dir = Path(r"C:\NetAutomation\backup")

    log_dir.mkdir(parents=True, exist_ok=True)
    backup_dir.mkdir(parents=True, exist_ok=True)

    devices = read_devices_from_csv(csv_file)

    results: List[Dict[str, str]] = []

    for dev in devices:
        host = dev["host"]
        print(f"Connecting to {host} ...")

        connection = None
        try:
            # เก็บ session log แยกไฟล์ (ช่วย debug)
            session_log = log_dir / f"{sanitize_filename(host)}_session.log"
            dev_with_log = dict(dev)
            dev_with_log["session_log"] = str(session_log)

            connection = ConnectHandler(**dev_with_log)

            # เข้า enable mode ถ้ามี secret
            if dev_with_log.get("secret"):
                connection.enable()

            # ดึง running-config
            running = connection.send_command("show running-config", read_timeout=180)

            hostname = get_hostname(connection, fallback_host=host)
            safe_hostname = sanitize_filename(hostname)

            # backup เป็นไฟล์ .txt
            backup_file = backup_dir / f"{safe_hostname}_backup.txt"
            backup_file.write_text(running, encoding="utf-8")
            print("Backup saved to:", str(backup_file))

            # ตรวจ compliance ก่อน
            missing_before = check_compliance(running)

            # ถ้าขาด ให้ใส่ config แล้ว save แล้วตรวจซ้ำ
            if missing_before:
                print(f"{host} missing: {missing_before} -> applying config ...")
                connection.send_config_set(CONFIG_COMMANDS)
                # บันทึก config
                try:
                    connection.save_config()
                except Exception:
                    connection.send_command("write memory", read_timeout=60)

                running_after = connection.send_command(
                    "show running-config", read_timeout=180
                )
                missing_after = check_compliance(running_after)
            else:
                missing_after = []

            status = "PASS" if not missing_after else "FAIL"
            missing_str = ";".join(missing_after)

            results.append(
                {
                    "hostname": hostname,
                    "ip": host,
                    "status": status,
                    "missing": missing_str,
                }
            )

        except Exception as e:
            print("Error with", host, ":", e)
            results.append(
                {
                    "hostname": "UNKNOWN",
                    "ip": host,
                    "status": "ERROR",
                    "missing": str(e),
                }
            )
        finally:
            if connection:
                connection.disconnect()

    # เขียนสรุปผล
    result_file = log_dir / "batch43_check_result.csv"
    with open(result_file, "w", newline="", encoding="utf-8") as f:
        fieldnames = ["hostname", "ip", "status", "missing"]
        writer = csv.DictWriter(f, fieldnames=fieldnames)
        writer.writeheader()
        writer.writerows(results)

    print(f"สร้างไฟล์ {result_file} เรียบร้อย")

if __name__ == "__main__":
    main()

```
---
### Lab 5.1 – Read JSON and show interface status

**Coding assistant for json**

```
{
  "hostname": "R1",
  "interfaces": [
    {
      "name": "GigabitEthernet0/0",
      "ip_address": "10.0.0.1",
      "status": "up",
      "protocol": "up"
    },
    {
      "name": "GigabitEthernet0/1",
      "ip_address": "10.0.1.1",
      "status": "down",
      "protocol": "down"
    }
  ]
}

```
**Coding assistant**

```
# lab5_1_read_filejson.py

import json

def main():
    with open("C:\\NetAutomation\\inventory\\check_interfaces.json") as f:
        data = json.load(f)

    hostname = data["hostname"]
    print("Hostname:", hostname)
    print("Interfaces:")
    print("----------------------")

    for intf in data["interfaces"]:
        name = intf["name"]
        ip = intf["ip_address"]
        status = intf["status"]
        protocol = intf["protocol"]

        print(f"{name:25} {ip:15} {status:5}/{protocol:5}")

if __name__ == "__main__":
    main()

```
---
### Lab 5.2 – use RESTAPI to get information from device

**Coding assistant**

```
#lab5_2_restconf_get_interfaces.py

import requests
import urllib3
from pprint import pprint

# ปิด warning เรื่อง self-signed certificate (ใน Lab)
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# TODO: ผู้เรียนเปลี่ยนค่าตาม Lab จริง
ROUTER_IP = "xx.xx.xx.xx"     # IP ของ IOS-XE
USERNAME = "xxxx"
PASSWORD = "xxxx"

# RESTCONF base URL
BASE_URL = f"https://{ROUTER_IP}/restconf/data"

# HTTP Headers ตาม RESTCONF (Cisco IOS-XE รองรับ JSON)
HEADERS = {
    "Accept": "application/yang-data+json"
}

# Endpoint: interfaces-state (operational data)
url = f"{BASE_URL}/ietf-interfaces:interfaces-state/interface"

print(f"Requesting RESTCONF data from {url}")

response = requests.get(
    url,
    headers=HEADERS,
    auth=(USERNAME, PASSWORD),
    verify=False  # ใน Lab ใช้ self-signed cert
)

print("HTTP Status Code:", response.status_code)

if response.status_code == 200:
    data = response.json()
    print("\nRaw JSON keys:")
    print(data.keys())

    print("\nOutput of all interfaces on Device:")
    # โครงสร้าง: {'ietf-interfaces:interface': [ {...}, {...} ]}
    interfaces = data.get("ietf-interfaces:interface", [])
    for intf in interfaces:
        name = intf.get("name")
        oper_status = intf.get("oper-status")
        phys_addr = intf.get("phys-address", "N/A")
        print(f"- {name}: status={oper_status}, mac={phys_addr}")
else:
    print("Error body:")
    print(response.text)

```
---






