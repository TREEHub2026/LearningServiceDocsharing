# LAB Basic Linux + Shell Script

---

## Prepare Linux Server for Lab

### For Initial Setup before start to do lab

**Prompt assistant**

```
ช่วยจำลองตัวเองเป็น Linux server Ubuntu 24.04 server เพื่อใช้ในการทำ lab
กติกา
เมื่อพิมพ์ คำสั่ง ไป ให้ตอบกลับเหมือนทำงาน อยู่บน Linux จริงทุกประการ
username admin password pass
username root password admin
ใช้ shell เป็น bash shell
ตอนนี้ระบบมี disk ใหม่เพิ่มเข้ามาแล้ว 1 ลูก ขนาด 200G (/dev/sdb) แต่ยังไม่ได้ใช้งาน
```
---

### Lab 3.1 – Configure Networking

**Coding assistant for 00-installer-config.yaml **

```
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: false
      addresses:
        - 192.168.56.100/24
      routes:
        - to: default
          via: 192.168.56.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
```
---
### Lab 4.1 – Basic running Shell Script

**Coding assistant for hello.sh**

```
#!/bin/bash

echo "Enter your name:"
read name

echo "Hello world"
echo "My name is $name"

```
---
### Lab 4.2 – Create user from file by use shell script

**Coding assistant for autoadduser.sh**

```
#!/bin/bash

INPUT="users.txt"
DEFAULT_PASSWORD="Welcome123"

while IFS= read -r username
do
  [ -z "$username" ] && continue

  if id "$username" >/dev/null 2>&1; then
    echo "User $username already exists"
  else
    sudo adduser --disabled-password --gecos "" "$username"
    echo "$username:$DEFAULT_PASSWORD" | sudo chpasswd
    echo "User $username created with default password"
  fi
done < "$INPUT"

```
---

