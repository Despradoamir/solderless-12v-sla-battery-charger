# Minimalist Solderless 12V SLA Battery Charger (13.8V Float-Locked)

![Project Badges](badges.svg)



**Designed & Engineered by:** Amir Husseinzadeh  
**Contact:** [t.me/AmirHusseinzadeh](https://t.me)

---

## 📌 Overview
This open-hardware repository presents an ultra-reliable, minimalist, and **100% solderless 12V Sealed Lead-Acid (SLA) battery charger**. By utilizing natural transformer core impedance matching and selecting a precise **10V AC** secondary winding, the open-circuit DC voltage locks naturally at **13.8V DC**—the exact safe float-charge threshold for 12V SLA batteries—eliminating the need for active voltage regulators (`LM317`), PCBs, or cutoff relays.

### ✨ Key Engineering Highlights
* **Zero-Regulator Float Lock:** $V_{DC} \approx (10\text{V} \times \sqrt{2}) - 1.4\text{V} \approx 13.8\text{V}$ under no-load conditions, preventing battery overcharging and electrolyte dry-out.
* **Zero Solder Joints:** 100% mechanical crimp connections eliminate thermal solder-joint failure under continuous operation.
* **Passive Lead Heatsinking:** Full-length factory leads on `1N4007` diodes act simultaneously as passive heatsinks and secondary fusible links.
* **Primary Thermal Protection:** A `110°C` thermal fuse mounted flush against the transformer core cuts off 220V AC mains in case of prolonged short circuits or core overheating.

---

## 🔌 Circuit Schematic & Architecture

![Circuit Schematic](schematic.svg)

### Interactive Wiring Flow (Auto-Rendered by GitHub)

```mermaid
graph LR
    AC[220V AC Mains] --> TF[Thermal Fuse 110°C<br/>Mounted on Core]
    TF --> TR[Step-Down Transformer<br/>EI 57x26 Core<br/>220V -> 10V AC]
    TR -->|10V AC| BR[Bridge Rectifier<br/>4x 1N4007 Full-Length Leads<br/>Crimp Connections]
    BR -->|+ / -| CAP[Filter Capacitor<br/>1000uF / 25V]
    CAP -->|+13.8V DC| POS[Red Alligator Clip +]
    CAP -->|GND| NEG[Black Alligator Clip -]
```

---

## 🛠️ Bill of Materials (BOM)

| Qty | Component | Technical Specification | Role in Circuit |
| :-: | :--- | :--- | :--- |
| **1** | **Step-Down Transformer** | `EI 57×26` Core, 220V AC Primary, **10V and 20V AC** Secondary | Voltage step-down & natural impedance current limiting |
| **4** | **Rectifier Diode** | `1N4007` (Keep full factory lead length) | Full-wave bridge rectification & passive heat dissipation |
| **1** | **Electrolytic Capacitor** | `1000µF or 470µF / 25V` | Ripple filtering & stabilizing open-circuit float voltage |
| **1** | **Thermal Fuse** | `105°C – 115°C` (2A, 250V AC) | Primary AC mains disconnect upon core overheating |
| **-** | **Hardware & Insulation** | Metal crimp ferrules, fiberglass sleeving, Red/Black clips | Solderless mechanical joints & high-voltage insulation |

---

## 🔧 Step-by-Step Assembly Guidelines

1. **Primary Thermal Protection (220V AC Side):**
   * Secure the thermal fuse physically against the transformer winding or metal core using a high-temperature zip tie or heat-resistant tape.
   * Crimp the 220V AC line through the fuse to the transformer primary lead. **Do not solder** the thermal fuse (soldering iron heat will trip it).
   * Insulate all 220V joints with fiberglass sleeving.
2. **Solderless Bridge Rectifier (10V AC Side):**
   * **Do not trim** the leads of the four `1N4007` diodes; their full length is required for passive cooling.
   * Twist and crimp them into a standard bridge configuration using metal ferrules.
3. **Capacitor & Output Leads:**
   * Crimp the `470µF or 1000µF / 25V` capacitor across the `+` and `-` bridge junctions (observing polarity), then crimp the output alligator clip wires.
4. **Pre-Use Verification:**
   * Before connecting a battery, measure the open-circuit output voltage across the clips. It should read between **13.7V and 14.0V DC**.

---

## ❓ Frequently Asked Questions (FAQ)

### Can I use a standard 12V AC transformer instead of 10V AC?
**No.** A standard 12V AC transformer typically outputs 13V–14V AC under light load, which rectifies to **17V–18.5V DC** across the filter capacitor. Without an active regulator, this will overcharge and damage a 12V SLA battery. Using an **10V AC** tap (or unwinding a few turns from a 12V secondary) is critical to lock the output at **13.8V DC**.

### Can this charger be left connected permanently (float/trickle mode)?
**Yes.** Because the open-circuit voltage is locked at **13.8V DC** (within the standard 13.5V–13.8V/13.9V standby float range for 12V VRLA/SLA batteries), current naturally tapers off to a few milliamps once the battery reaches full charge.

### Can I charge a large car battery (e.g., 55Ah or 60Ah) with this circuit?
This specific BOM (`EI 57×26` core and `1N4007` 1A diodes) is optimized for **4.5Ah to 12Ah** SLA batteries (UPS, alarm panels, emergency lights, motorcycles). While it can maintain a larger battery, charging a deeply discharged car battery from empty would overload the 1A diodes and small core.

### Why are there no solder joints in this design?
Under continuous high-current charging, low-cost rectifier diodes run hot, which frequently causes standard tin-lead solder joints to crack or melt over time. Mechanical crimp ferrules combined with full-length diode leads eliminate this failure mode entirely.

---

## ⚠️ Safety Disclaimer
This circuit connects directly to **220V AC mains**. Ensure all primary connections are properly insulated with fiberglass sleeving and housed inside a non-conductive enclosure with ventilation slots before powering on.

---

## 📄 License & Attribution
This hardware design is released under the **MIT License** (see the `LICENSE` file for details).  
If you build, modify, or reference this design, please credit the original designer:  
**Amir Husseinzadeh** — [t.me/AmirHusseinzadeh](https://t.me)