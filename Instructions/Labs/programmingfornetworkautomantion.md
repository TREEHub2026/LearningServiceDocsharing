# LAB Programming for Network Automation

---

## Lab 1 – Run your first Script

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
### Lab 1.2 – Manage list of IP Address

**Coding assistant**

```

```
---
### Lab 1.2 – Manage list of IP Address

**Coding assistant**

```

```
---
### Lab 1.2 – Manage list of IP Address

**Coding assistant**

```

```
---
### Lab 1.2 – Manage list of IP Address

**Coding assistant**

```

```
---








