# 🏎️ Merkle Tree Based Integrity Verification for F1 Telemetry Data

A cryptographic integrity verification system that uses **Merkle Trees** and **SHA-256** to protect Formula 1 telemetry data against **data tampering** and **Man-in-the-Middle (MitM) attacks**.

This project demonstrates how real F1 telemetry can be transformed into a tamper-evident structure where any unauthorized modification immediately becomes detectable.

---

## 📌 Overview

Modern Formula 1 cars continuously transmit telemetry data from hundreds of sensors to the pit wall during race sessions. This data directly influences strategic decisions, making data integrity extremely important.

This project proposes a lightweight cryptographic solution using a **Merkle Tree**, where every telemetry data point is hashed using SHA-256 and combined into a single **Merkle Root** representing the integrity of the entire dataset.

A single modified value will propagate through the tree and generate a completely different root hash.

---

## 🎯 Objectives

* Protect F1 telemetry data against unauthorized modification.
* Detect data tampering efficiently.
* Simulate Man-in-the-Middle (MitM) attacks.
* Demonstrate efficient verification using Merkle Proofs.
* Evaluate computational feasibility for high-frequency telemetry environments.

---

## 📊 Dataset

The implementation uses real Formula 1 telemetry data obtained through the FastF1 Python library.

**Dataset Information**

| Parameter     | Value                       |
| ------------- | --------------------------- |
| Driver        | Lewis Hamilton (HAM)        |
| Session       | Silverstone 2023 Qualifying |
| Source        | FastF1                      |
| Data Points   | 1,004                       |
| Sampling Rate | ~10-20 Hz                   |
| Hash Function | SHA-256                     |

Telemetry channels used:

* Time
* Speed
* Throttle
* Brake
* RPM
* Gear
* DRS

---

## 🏗️ System Architecture

The system follows the pipeline below:

```text
Telemetry Data
       ↓
Data Serialization
       ↓
SHA-256 Hashing
       ↓
Leaf Nodes
       ↓
Merkle Tree Construction
       ↓
Merkle Root Generation
       ↓
Integrity Verification
```

---

## 🌳 Merkle Tree Concept

Each telemetry row is converted into a deterministic JSON string before hashing.

```text
Telemetry Row
      ↓
SHA-256
      ↓
Leaf Hash
```

The leaf hashes are recursively combined until a single Merkle Root is generated.

```text
           Root
         /      \
     HashAB    HashCD
      /  \      /  \
     A    B    C    D
```

Any modification to any telemetry value changes the corresponding leaf hash and ultimately changes the Merkle Root.

---

## ⚔️ Threat Model

### 1. Data Tampering

An attacker modifies telemetry values stored in the system.

Example:

```text
Speed: 298 km/h → 999 km/h
```

Result:

* Leaf hash changes
* Parent hashes change
* Merkle Root changes

---

### 2. Man-in-the-Middle (MitM)

An attacker intercepts telemetry data during wireless transmission.

```text
Car → Attacker → Pit Wall
```

The receiver verifies incoming data against the trusted Merkle Root.

Any modification will cause verification failure.

---

## 🧪 Experimental Results

### Tampering Detection

| State            | Result         |
| ---------------- | -------------- |
| Original Dataset | Valid Root     |
| Modified Dataset | Different Root |
| Detection        | Successful     |

### Merkle Proof Efficiency

Traditional verification:

```text
O(n)
```

Merkle Proof verification:

```text
O(log n)
```

For this dataset:

```text
1004 data points
10 verification hashes per leaf
```

---

## 📂 Notebook Contents

`datatelemetry.ipynb` includes:

* F1 telemetry extraction using FastF1
* Data preprocessing
* Serialization
* SHA-256 hashing
* Merkle Tree construction
* Tampering simulation
* Merkle Proof generation
* Tree visualization
* Hash performance analysis

---

## 🛠️ Technologies Used

* Python
* FastF1
* Pandas
* hashlib
* Matplotlib
* JSON

---

## 🚀 Installation

Clone the repository:

```bash
git clone https://github.com/GbrlJennie/merkle-tree-F1.git

cd merkle-tree-F1
```

Install dependencies:

```bash
pip install fastf1 pandas matplotlib numpy
```

Run the notebook:

```bash
jupyter notebook datatelemetry.ipynb
```

---

## 📄 Research Paper

**Merkle Tree Based Integrity Verification for F1 Telemetry Data Against Tampering and Man-in-the-Middle Attacks**

This project was developed as part of the II4021 Cryptography course at Institut Teknologi Bandung.

---

## 🎥 Video Demonstration

https://youtu.be/GKRUUT38Ssg

---

## 👩‍💻 Author

Gabriela Jennifer Sandy

18223092

Information Systems and Technology

Institut Teknologi Bandung

---

## 📚 References

* Merkle, R. C. (1988). A Digital Signature Based on a Conventional Encryption Function.
* Nakamoto, S. (2008). Bitcoin: A Peer-to-Peer Electronic Cash System.
* FastF1 Python Library.
