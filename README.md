# 🔍 Simple Port Scanner (Python)

A basic Python-based port scanner that checks for open TCP ports on a target IP address over a given port range.

---

## 📌 Features
- Custom IP and port range input
- Shows open ports
- Clean terminal output
- Built using Python

---

## 🚀 How to Run

### 1. Requirements
- Python 3
- Kali Linux

---

### 2. Steps to Run

```bash
# Step 1: Update Kali system packages
sudo apt update

# Step 2: (If needed) Install Python 3
sudo apt install python3 -y

# Step 3: Clone or create project folder and navigate to it
mkdir simple-port-scanner && cd simple-port-scanner

# Step 4: Add the script simple_scanner.py here
# (You can use nano or any editor)
nano simple_scanner.py

# Step 5: Run the scanner
python3 simple_scanner.py <target_ip> <start_port> <end_port>

```

### Command

```
python3 simple_scanner.py 45.33.32.156 22 22
```

### Python File

```python
import socket
import sys
import time

def simple_port_scanner(target_ip: str, start_port: int, end_port: int):
    """
    Performs a basic TCP port scan on a target IP address within a specified port range.

    Args:
        target_ip (str): The IP address of the target machine.
        start_port (int): The starting port number for the scan.
        end_port (int): The ending port number for the scan.
    """
    print(f"[*] Scanning target: {target_ip}")
    print(f"[*] Port range: {start_port}-{end_port}")
    print("-" * 40)

    open_ports = []

    # Iterate through the specified port range
    for port in range(start_port, end_port + 1):
        try:
            # Create a new socket object for IPv4 and TCP
            s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

            # Set a timeout to prevent the scanner from hanging
            s.settimeout(0.5) # Shorter timeout for faster scans (0.5 seconds)

            # Attempt to connect to the target IP and port
            # connect_ex returns 0 on success, an error code otherwise
            result = s.connect_ex((target_ip, port))

            if result == 0:  # 0 indicates a successful connection (port is open)
                print(f"[+] Port {port:<5} OPEN")
                open_ports.append(port)

            s.close() # Always close the socket

        except socket.gaierror:
            print("[!] Hostname could not be resolved. Exiting.")
            sys.exit()
        except socket.error:
            print("[!] Could not connect to server. Check IP or network. Exiting.")
            sys.exit()
        except KeyboardInterrupt:
            print("\n[!] Scan interrupted by user.")
            sys.exit()
        except Exception as e:
            print(f"[!] An unexpected error occurred: {e}")
            # Do not exit on general exception to allow scan to continue for other ports
            # sys.exit() # Removed to allow scan to continue on other ports

    print("-" * 40)
    if open_ports:
        print(f"[*] Scan complete. Open ports found: {open_ports}")
    else:
        print("[*] Scan complete. No open ports found in the specified range.")

# Entry point for the script when run directly
if __name__ == "__main__":
    # Ensure correct number of command-line arguments
    if len(sys.argv) != 4:
        print("Usage: python3 simple_scanner.py <target_ip> <start_port> <end_port>")
        print("Example: python3 simple_scanner.py 127.0.0.1 1 100")
        sys.exit(1)

    # Parse arguments
    target = sys.argv[1]
    try:
        start = int(sys.argv[2])
        end = int(sys.argv[3])
    except ValueError:
        print("[!] Port numbers must be integers.")
        sys.exit(1)

    # Validate port range
    if not (1 <= start <= 65535) or not (1 <= end <= 65535) or (start > end):
        print("[!] Invalid port range. Ports must be between 1 and 65535, and start_port <= end_port.")
        sys.exit(1)

    # Execute the scanner
    simple_port_scanner(target, start, end)

```

### Sample Output

<img width="418" height="163" alt="image" src="https://github.com/user-attachments/assets/ed2a7d1c-e185-486f-b381-aa74ee5ecdaa" />

