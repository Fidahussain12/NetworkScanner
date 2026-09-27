NetScan

A lightweight, multi-threaded Python network scanner that discovers online devices on your local network. Works on both Windows and Linux/macOS.

Features
Fast parallel scanning — uses multi-threading (100 workers) to ping an entire subnet quickly instead of checking hosts one by one
Auto-detects your local network — no need to know your IP/subnet in advance
Cross-platform — works on Windows (ipconfig) and Linux/macOS (ifconfig)
Flexible input — scan by full CIDR (192.168.1.0/24), a short form (192.168.1), or let it auto-detect
Graceful interrupt handling — press Ctrl+C mid-scan and still see results found so far


Requirements:
Python 3.7+
ping command available on your system (comes pre-installed on Windows, Linux, and macOS)
No external dependencies — uses only the Python standard library


Installation: 

bash
git clone https://github.com/<your-username>/netscan.git
cd netscan

No pip install needed — everything used is part of Python's standard library.

Usage
bash
# Auto-detect and scan your current local network
python netscan.py

# Scan a specific network in CIDR notation
python netscan.py 192.168.1.0/24

# Scan using a shorthand network (assumes /24)
python netscan.py 192.168.1

# Show help
python netscan.py -h
Example output
Scanning network: 192.168.0.0/24 ...

Online hosts:
192.168.0.1
192.168.0.100
192.168.0.101
192.168.0.102
192.168.0.105
How it works
get_local_ip_and_mask() — detects your machine's IP and subnet mask via ipconfig (Windows) or ifconfig (Linux/macOS), with a socket-based fallback if neither works.
parse_network() — turns the command-line argument (or nothing, for auto-detect) into an ipaddress.ip_network object.
scan_network() — pings every host in the network concurrently using a thread pool.
ping() — sends a single ICMP ping to one IP and checks the reply for a TTL to determine if it's online.
main() — ties it all together: parses arguments, runs the scan, and prints sorted results.
Notes
On Linux/macOS, some systems may require elevated privileges (sudo) for raw ping operations depending on configuration.
Scan speed depends on subnet size and network conditions — a full /24 (254 hosts) typically completes in a few seconds thanks to parallel threading.
License

MIT License — feel free to use, modify, and share.
