# Protocol_Validation

## 🔗 PCIe 6.0 Architecture Overview
The model implements the standard PCIe stack:

- Software Layer  
- Transaction Layer (TLP)  
- Data Link Layer (DLP)  
- Physical Layer (Flit Mode)

From the document:

> “A Flit is 256 bytes in length: 236 bytes of TLP traffic, 6 bytes of DLP traffic, 8 bytes of Flit CRC, 6 bytes of Flit ECCs.”

---

## 🧩 System Topology (3 Masters × 3 Endpoints)
The VisualSim model connects:

- **3 Traffic Generators (Masters)**  
- **3 DRAM Endpoints (Slaves)**  
- **PCIe 6.0 Switch** performing routing, arbitration, fragmentation, and flit assembly

As stated:

> “The Master is Normal TG generating transactions of Read or Write. The slaves are randomly selected.”

---

## 📦 Flit Structure (PCIe 6.0)
Each flit is **256 bytes**, composed of:

| Component | Size |
|----------|------|
| TLP Payload | 236B |
| DLP | 6B |
| CRC | 8B |
| ECC | 6B |

> “One or more TLP can be packed within a Flit… some TLPs can span across multiple Flits.”

---

## 🧪 Validation Flow

### 1. Transaction Generation
Each master sends:
- Random **Read/Write** commands  
- Random endpoint selection  
- Fixed payload size (1024B)

> “The Master1 is randomly sending transactions to Endpoint1, Endpoint2 and Endpoint3.”

---

### 2. TLP Formation
The TG generates **236‑byte TLPs**, visible in logs:

> “A_Bytes = 236… PCIe_Bytes = 236.”

---

### 3. Flit Assembly
Flits are created only when 236B payload is available:

> “Until it is filled it won’t generate a new flit. Hence we see FLIT[1] until it gets filled.”

---

### 4. PCIe Switch Routing
The switch performs:
- Fragmentation  
- Arbitration  
- Port‑based routing  
- Buffer management  

Example:

> “Destination Port = 7, TLPs = 2, TLPs total size = 128.”

---

### 5. ACK/NAK Replay Validation
The model validates PCIe 6.0 replay logic:

> “RX acknowledges and sends ACK back… Prev_Flit_Ukey = 2.”

---

### 6. Endpoint Buffer Validation
Overflow detection works correctly:

> “Packet getting dropped at EP_2. Current buffer usage = 4096.0 and required space = 64.”

---

## 📊 Key Observations
- ✔ Correct flit packing (multiple TLPs per flit)  
- ✔ Correct routing through PCIe switch  
- ✔ ACK/NAK replay mechanism validated  
- ✔ Endpoint buffer overflow detection  
- ✔ MRRS validation warning for oversized requests  

Example:

> “Received input with request size (1024) > Max_Read_Req_Size_Bytes (64).”

---

## 📈 Performance Metrics
Captured from logs:

- Transaction latency  
- Throughput arrays  
- Flit sequence numbers  
- TLP segmentation  
- Buffer usage per port  

Example:

> “Task_Latency = 3.2E‑11, Throughput_Array = (3.2E13).”

---

## ▶ How to Run the Model
1. Open VisualSim Architect  
2. Load: `PCIe6_Device_Interface3x3_model.xml`  
3. Set simulation time to **1 µs**  
4. Enable:
   - Debug Display  
   - Text Displays  
   - PCIe Bus Debug  
5. Run simulation  
6. Inspect:
   - Flit assembly logs  
   - TLP segmentation  
   - Endpoint buffer usage  
   - ACK/NAK behavior  

---

## 📚 References
- PCIe 6.0 Specification  
- AsicLab VLSI Academy (reference diagrams)  
- VisualSim Architect Documentation  

---

## 📬 Contact
For improvements, analysis, or collaboration, feel free to reach out or open an issue.




