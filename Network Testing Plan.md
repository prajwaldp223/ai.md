# *Network Testing Plan*
-----

#### *Objective:*
Test the network's file-sharing performance and speed/load capabilities using various tools ( ssh, netcat, python webserver), and evaluate bandwidth utilization, latency, and file transfer efficiency.

---

### *1. Environment Setup*
- *Test Machines*: At least two devices (Source and Destination) on the same network or across different networks.
- *Files for Testing*: Different file sizes for testing transfer times:
  - Small files: 10MB
  - Medium files: 500MB
  - Large files: 5GB+
- *Network Conditions*: Run tests under different network load conditions (idle, moderate traffic, heavy traffic).

---

### *2. File Sharing Test*

#### *2.1 ssh (Remote Execution)*
- *Purpose*: Test network latency and reliability for remote commands and file transfers.
- *Command*: ssh user@remote "command"
- *Test Plan*:
  - Use SSH to execute remote commands, e.g., ls or cat largefile, and measure responsiveness.
  - Measure time delay for executing commands over the network.
    bash
    time ssh user@remote "ls -l /path/to/directory"
    

#### *2.2 netcat (Raw Data Transfer)*
- *Purpose*: Measure file transfer performance using a lightweight utility for raw data transfer.
- *Command*: 
       ```
       nc -l PORT > file on receiver and cat file | nc host PORT on sender.
       ```
- *Test Plan*:
  - Set up netcat listener on the destination machine.
  - Measure speed by transferring files of different sizes.
    ```
    bash
    time cat sourcefile | nc destination_IP 12345
    ```
#### *2.3 Python HTTP Server (Wireless File Transfer)*
- *Purpose*: Transfer files wirelessly between devices on the same network using a Python HTTP server.
- *Python Server Setup*:
  - Run the Python server on the source machine:
    ```
   bash:
    python3 -m http.server 8000
    ```
  - The server will start, and files in the current directory will be accessible via http://<source-IP>:8000/.

- *Transfer Process*:
  - Access the URL http://<source-IP>:8000/ from the destination machine's browser or use wget/curl to download files:
    bash
    wget http://<source-IP>:8000/filename
    

- *Test Plan*:
  - Measure the time taken to transfer files of different sizes over the wireless network.
  - Test under various network conditions (idle, moderate traffic, heavy traffic).
    bash
    time wget http://<source-IP>:8000/filename
    
---

### *3. Speed Test*

#### *3.1 Bandwidth Utilization*
- *Objective*: Measure how much bandwidth is utilized during file transfers.
- *Tools*: iperf or iperf3
- *Test Plan*:
  - Run a bandwidth test between two machines.
  - Measure upload and download speeds over different protocols (TCP, UDP).
  ```
    bash
    iperf3 -s  # On server
    iperf3 -c server_IP  # On client
  ```  

#### *3.2 Latency Measurement*
- *Objective*: Measure latency during file transfers to identify delays.
- *Tools*: ping, traceroute
- *Test Plan*:
  - Use ping to measure round-trip time (RTT).
  ```
    bash
    ping destination_IP
  ```  
  - Use traceroute to identify hop delays.
  ```
    bash
    traceroute destination_IP
  ```  

---

### *4. Load Test*

#### *4.1 Concurrent File Transfers*
- *Objective*: Measure performance under load (multiple transfers happening simultaneously).
- *Tools*: netcat with multiple instances
- *Test Plan*:
  - Start multiple file transfers at the same time and monitor bandwidth usage and transfer time.
```    
bash
    scp sourcefile user@remote:/destination &
    scp anotherfile user@remote:/destination &
```    

#### *4.2 Simulate High Network Load*
- *Objective*: Test file sharing and speed performance under heavy network load.
- *Tools*: iperf, custom load generation scripts.
- *Test Plan*:
  - Simulate heavy network traffic using iperf or multiple scp processes to flood the network.
```   
 bash
    iperf3 -c destination_IP -u -b 100M  # Simulate heavy traffic
```    

---

### *5. Performance Monitoring*

- Use tools like htop, iftop, or nload to monitor CPU and network usage during tests.
- Track and log the data transfer speeds, bandwidth, latency, and CPU utilization for each test.

---

### *6. Reporting*
- Prepare a report including:
  - File sizes, transfer times, and network conditions.
  - Comparisons of transfer methods (ssh,netcat etc.).
  - Bandwidth utilization and latency metrics.
  - Performance under different load scenarios.
  - Recommendations for optimizing the network.

---