Network Analyser

This project is a simple Python script designed to monitor the ARP table for duplicate MAC addresses, which could indicate a potential ARP spoofing attack. When a duplicate MAC address is detected, the script sends a predefined message to a specified server.

Features
Monitors ARP table for duplicate MAC addresses.
Sends an HTTP POST request to a server when an attack is detected.
Prints the current time and IP address of the duplicate MAC address.

Requirements
Python 3.x
Internet connection (to send HTTP POST requests)
Access to the ARP table (administrator/root permissions may be required)


Installation
Clone the repository:
git clone https://github.com/yusufdalbudak/networkanalyser_vol2.git

Navigate to the project directory:
cd networkanalyser_vol2


Ensure you have Python 3.x installed. You can download it from python.org.

Usage
Open the netwdiscover.py file and review the code to understand its functionality.

Run the script with administrator/root privileges:

python netwdiscover.py

import os
import time
import datetime
import http.client

conn = http.client.HTTPConnection("45.84.188.101")
message = ''' ... '''  # ASCII art and message

headers = { "Content-type": "application/x-www-form-urlencoded", "Accept": "text/plain" }

print("Açıldı.")

def print_arp_table(conn, message, headers):
    current_time = datetime.datetime.now().time()
    arp_table = os.popen("arp -a").read()
    mac_list = []
    print("Güvendeyiz...")
    for line in arp_table.split("\n"):
        if "dynamic" in line:
            ip = line.split()[0]
            mac = line.split()[1]

            if mac in mac_list:
                print("Saldırı altındayız...")
                conn.request("POST", "", message.encode('utf-8'), headers)
                response = conn.getresponse()
                print(response.status, response.reason)
                print(current_time)
                print("IP adresi : " + ip)
                conn.close()
                break
            else:
                mac_list.append(mac)

while True:
    print_arp_table(conn, message, headers)
    time.sleep(5)



Functionality
print_arp_table: This function reads the ARP table and checks for duplicate MAC addresses. If a duplicate is found, it sends an HTTP POST request with a predefined message to a specified server.


License
This project is licensed under the MIT License - see the LICENSE file for details.









