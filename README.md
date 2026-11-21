Minimalist Solderless 12V SLA & UPS Battery Charger (5Ah–12Ah, 13.8V Float)


![Project Badges](badges.svg)



**Designed & Engineered by:** Amir Husseinzadeh  
**Contact:** [t.me/AmirHusseinzadeh](https://t.me)

---

## 📌 Overview
This open-hardware repository presents an ultra-reliable, minimalist, and **100% solderless 12V Sealed Lead-Acid (SLA) battery charger**. By utilizing natural transformer core impedance matching and selecting a precise **10V AC** secondary winding, the open-circuit DC voltage locks naturally at **13.8V DC**—the exact safe float-charge threshold for 12V SLA batteries—eliminating the need for active voltage regulators (`LM317`), PCBs, or cutoff relays.

### ✨ Key Engineering Highlights
* **Zero-Regulator Float Lock:** $V_{DC} \approx (10\20 text{V} \times \sqrt{2}) - 1.4\text{V} \approx 14.4\text{V}$ under no-load and 13.8 under load conditions, preventing battery overcharging and electrolyte dry-out.
* **Zero Solder Joints:** 100% mechanical crimp connections eliminate thermal solder-joint failure under continuous operation.
* **Passive Lead Heatsinking:** Full-length factory leads on `1N5408` diodes act simultaneously as passive heatsinks and secondary fusible links.
* **Primary Thermal Protection:** A `110°C` thermal fuse mounted flush against the transformer core cuts off 220V AC mains in case of prolonged short circuits or core overheating and 3 or 5amp car fuse on out+ to load

---

## 🔌 Circuit Schematic & Architecture

![Circuit Schematic](schematic.svg)
```mermaid
flowchart TD
    A[220V AC Mains] --> B[110°C Thermal Fuse on Core]
    B --> C["Yaxun 1502DD Transformer<br/>(10V AC Output)<br/>⚠ Do NOT use 20V AC"]
    C --> D["Bridge Rectifier<br/>4x 1N5408 (Long Leads)"]
    D -->|"+"| E["Filter Capacitor 1000µF / 25V"]
    D -->|"-"| F["Negative Rail (-)"]
    E --- F
    E --> G[3A/5A Automotive Fuse]
    G --> H["Alligator Clip (+)"]
    F --> I["Alligator Clip (-)"]
    H --> J["12V UPS Battery<br/>(5Ah to 12Ah)"]
    I --> J
```


```

---

## 🛠️ Bill of Materials (BOM)

| Qty | Component | Technical Specification | Role in Circuit |
| :-: | :--- | :--- | :--- |
| **1** | **Step-Down Transformer** | `EI 57×26` Core, 220V AC Primary, **10V and 20V AC** Secondary | Voltage step-down & natural impedance current limiting |
| **4** | **Rectifier Diode** | `1N5408` (Keep full factory lead length) | Full-wave bridge rectification & passive heat dissipation |
| **1** | **Electrolytic Capacitor** | `1000µF or 470µF / 25V` | Ripple filtering & stabilizing open-circuit float voltage |
| **1** | **Thermal Fuse** | `105°C – 115°C` (2A, 250V AC) | Primary AC mains disconnect upon core overheating 
and for dc out+ to loader car fuse 3 or 5amp fuse for more safety 
|
| **-** | **Hardware & Insulation** | Metal crimp ferrules, fiberglass sleeving, Red/Black clips | Solderless mechanical joints & high-voltage insulation |
1000µF / 25V electrolytic — Output noise suppression & EMI reduction.
Note: this capacitor is NOT for ripple smoothing. Ripple is handled by the
battery itself (a 5–12Ah SLA behaves as hundreds of farads at 100/120Hz).
1000µF is deliberate: enough for noise, small enough to stay minimal.
Sizing it for ripple would require 4700–6800µF with no practical benefit.
---

## 🔧 Step-by-Step Assembly Guidelines

1. **Primary Thermal Protection (220V AC Side):**
   * Secure the thermal fuse physically against the transformer winding or metal core using a high-temperature zip tie or heat-resistant tape.
   * Crimp the 220V AC line through the fuse to the transformer primary lead. **Do not solder** the thermal fuse (soldering iron heat will trip it).
   * Insulate all 220V joints with fiberglass sleeving.
2. **Solderless Bridge Rectifier (10V AC Side):**
   * **Do not trim** the leads of the four `1N5408` diodes; their full length is required for passive cooling.
   * Twist and crimp them into a standard bridge configuration using metal ferrules.
3. **Capacitor & Output Leads:**
   * Crimp the `470µF or 1000µF / 25V` capacitor across the `+` and `-` bridge junctions (observing polarity), then crimp the output alligator clip wires.
4. **Pre-Use Verification:**
   * Before connecting a battery, measure the open-circuit output voltage across the clips. It should read between **13.7V and 14.0V DC**.

---
Design Notes!!!

Capacitor C1 (1000µF) — Noise Reduction, Not Ripple Smoothing

The 1000µF capacitor on the output is not a smoothing capacitor for ripple. Its sole purpose is to suppress high-frequency switching noise from the rectifier diodes and reduce EMI on the output leads.

Ripple smoothing in this topology is handled by the battery itself. A typical 5–12Ah SLA battery presents an effective capacitance of hundreds of farads at 100/120Hz, so terminal ripple under charge is negligible regardless of C1. If ripple smoothing were the goal, C1 would need to be at least 4700–6800µF — which would add bulk, inrush current, and cost without any practical benefit. The 1000µF value is deliberate: enough for noise, small enough to stay minimal.

Transformer — The Critical Component

In this open-loop, regulator-free design, the transformer is not a secondary part — it is part of the circuit. Output voltage is determined directly by:

Vout ≈ Vsecondary(peak) − diode drops − transformer regulation drop − ripple

There is no active regulation to correct for a poor transformer. This is why the design uses a Yaxun 1502DD Plus (EI 57×26 core) as the reference transformer. It offers low regulation drop under load, adequate copper, and stable behavior — and it is available worldwide for approximately $2, which is central to the project's low-cost claim.

Reproducibility note: Any EI 57×26 transformer with good regulation (low DC resistance in both windings, minimal voltage drop at rated load) will produce the same result. Low-quality transformers with undersized copper and 20–30% regulation will not hold 13.8V float — they will either overshoot or sag below the safe float window. If you substitute the transformer, verify its regulation under load before trusting the output voltage.

Recommended check for builders: Measure the secondary voltage unloaded, then again at ~2A load. A well-made transformer will drop only a few volts. A poor one will drop significantly more and should be rejected.

## ❓ Frequently Asked Questions (FAQ)

### Can I use a standard 12V AC transformer instead of 10V AC?
##NO##
 Why does transformer quality matter so much?

In this design there is no voltage regulator. Output voltage is determined
directly by:

    Vout ≈ Vsecondary(peak) − diode drops − transformer regulation drop

A low-quality transformer with undersized copper and 20–30% regulation will
NOT hold 13.8V float — it will either overshoot or sag below the safe float
window. The reference transformer for this project is the **Yaxun 1502DD Plus**
(EI 57×26 core), chosen for its low regulation drop under load. It is
available worldwide for approximately **$2**, which is central to this
project's low-cost claim.

**If you substitute the transformer:** measure the secondary voltage unloaded,
then again at ~2A load. A well-made transformer drops only a few volts.
A poor one drops significantly more and should be rejected before use.

### Can this charger be left connected permanently (float/trickle mode)?
**Yes.** Because the open-circuit voltage is **13.8V DC** under light load , without load 14.4v DC (within the standard 13.5V–13.8V/13.9V standby float range for 12V VRLA/SLA batteries), current naturally tapers off to a few milliamps once the battery reaches full charge.

### Can I charge a large car battery (e.g., 55Ah or 60Ah) with this circuit?
This specific BOM (`EI 57×26` core and `1N5408` 3A diodes) is optimized for **4.5Ah to 12Ah** SLA batteries (UPS, alarm panels, emergency lights, motorcycles). While it can maintain a larger battery, charging a deeply discharged car battery from empty would overload the 3A diodes and small core.

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
