# SIEM Intelligence Processors: UEBA, AI/ML, and Analytics Engines

## Table of Contents
1. [Introduction: The Intelligence Layer Question](#introduction-the-intelligence-layer-question)
2. [The Architectural Debate: Integrated vs. Standalone](#the-architectural-debate-integrated-vs-standalone)
3. [UEBA - User and Entity Behavior Analytics](#ueba---user-and-entity-behavior-analytics)
4. [cyDNA and Advanced Threat Detection](#cydna-and-advanced-threat-detection)
5. [AI/ML Analytics Engines](#aiml-analytics-engines)
6. [The Intelligence Spectrum: From Simple to Sophisticated](#the-intelligence-spectrum-from-simple-to-sophisticated)
7. [Are These Part of SIEM or Separate?](#are-these-part-of-siem-or-separate)
8. [Modern SIEM Architecture: The Intelligence Stack](#modern-siem-architecture-the-intelligence-stack)
9. [Vendor Approaches Comparison](#vendor-approaches-comparison)
10. [Choosing Your Architecture](#choosing-your-architecture)
11. [Conclusion](#conclusion)

---

## Introduction: The Intelligence Layer Question

You've identified a **critical nuance** in SIEM architecture! If SIEM is the "brain" and correlation engine, then where do these advanced intelligence processors fit?

- **UEBA** (User and Entity Behavior Analytics)
- **cyDNA** (Cyber Defense Neural Analytics / Genetic-based threat detection)
- **AI/ML engines** (Machine learning models)
- **Advanced analytics** (Statistical models, anomaly detection)
- **Threat intelligence platforms**

### The Core Question

**Are all these intelligence processors part of the SIEM umbrella, or are they separate systems?**

**The answer**: **It depends** - and this is where the industry is actively evolving!

There are **three architectural approaches**:

```
Approach 1: Integrated (All-in-One SIEM)
┌─────────────────────────────────────────┐
│           SIEM Platform                 │
│  ┌────────────────────────────────┐    │
│  │ UEBA Module (built-in)         │    │
│  │ AI/ML Engine (built-in)        │    │
│  │ Correlation Rules              │    │
│  │ Threat Intel (built-in)        │    │
│  └────────────────────────────────┘    │
└─────────────────────────────────────────┘
Examples: Microsoft Sentinel, Splunk ES (with UEBA),
          Exabeam Fusion

Approach 2: Modular (SIEM + Plugins)
┌─────────────────────────────────────────┐
│           SIEM Core                     │
│  ┌────────────────────────────────┐    │
│  │ Correlation Rules              │    │
│  │ Basic Analytics                │    │
│  └────────────────────────────────┘    │
└──────────┬──────────────────────────────┘
           │
     ┌─────┴─────┬──────────┬────────┐
     ▼           ▼          ▼        ▼
┌─────────┐ ┌─────────┐ ┌──────┐ ┌──────┐
│  UEBA   │ │ AI/ML   │ │ TIP  │ │cyDNA │
│ Add-on  │ │ Add-on  │ │Add-on│ │Add-on│
└─────────┘ └─────────┘ └──────┘ └──────┘
Examples: Splunk ES + Splunk UEBA (separate purchase),
          IBM QRadar + Watson AI modules

Approach 3: Best-of-Breed (Separate Systems)
┌──────────────────────────────────────────┐
│        SIEM (Correlation)                │
└──────────┬───────────────────────────────┘
           │
     ┌─────┴─────┬──────────┬────────┐
     ▼           ▼          ▼        ▼
┌─────────┐ ┌─────────┐ ┌──────┐ ┌──────┐
│ Exabeam │ │Darktrace│ │Anomali│ │Custom│
│  UEBA   │ │ AI/ML   │ │  TIP  │ │ ML   │
│(Separate│ │(Separate│ │(Sep.) │ │Engine│
│Platform)│ │Platform)│ │       │ │      │
└─────────┘ └─────────┘ └──────┘ └──────┘
Examples: Splunk Core + Exabeam UEBA,
          QRadar + Darktrace AI,
          ELK + Custom ML models
```

**The trend**: Moving from Approach 3 → Approach 1 (consolidation)

---

## The Architectural Debate: Integrated vs. Standalone

### Historical Context

**2000-2010: SIEM = Simple Correlation**
```
SIEM Intelligence = Basic rule-based correlation
IF (5 failed logins) THEN alert
```

**2010-2015: SIEM + Separate UEBA**
```
SIEM: Rule-based detection
UEBA: Separate product for behavioral analytics
Problem: Two consoles, two teams, integration challenges
```

**2015-2020: SIEM Vendors Acquire UEBA**
```
Splunk acquires Caspida (UEBA) → Splunk UEBA
Exabeam starts as pure UEBA, adds SIEM features
```

**2020-Present: Native AI/ML in SIEM**
```
Microsoft Sentinel: Built-in ML, UEBA native
Splunk ES: Integrated UEBA option
QRadar: Watson AI embedded
```

### The Debate Today

| Perspective | Argument |
|-------------|----------|
| **"Everything in SIEM"** | "SIEM should be a unified analytics platform with all intelligence built-in. Separate tools create silos and integration headaches." |
| **"Best-of-Breed"** | "SIEM does correlation; specialized tools do ML/UEBA better. Don't lock into one vendor's AI algorithms." |
| **"It Depends on Maturity"** | "Small orgs need integrated; large enterprises can manage multiple specialized tools." |

---

## UEBA - User and Entity Behavior Analytics

### What is UEBA?

**UEBA** uses **machine learning** to establish behavioral baselines and detect anomalies for:
- **Users**: Employees, contractors, service accounts
- **Entities**: Hosts, applications, network devices, cloud resources

### UEBA vs Traditional SIEM Correlation

| Traditional SIEM Rules | UEBA Machine Learning |
|------------------------|----------------------|
| **Static rules**: "IF login from 2 locations in < 1 hour THEN alert" | **Dynamic baselines**: "User normally logs in from NYC; today from Russia = anomaly" |
| **Threshold-based**: "IF > 10 failed logins" | **Peer comparison**: "Downloads 100x more data than peers" |
| **Signature detection**: Known bad patterns | **Anomaly detection**: Unknown suspicious patterns |
| **False positives**: High (rigid rules) | **False positives**: Lower (context-aware) |
| **Detects**: Known threats | **Detects**: Unknown/insider threats |

### Example: Insider Threat Detection

**Scenario**: Employee preparing to leave company and steal data

**Traditional SIEM Correlation**:
```
Rule 1: Alert if > 1000 files downloaded
Result: No alert (employee downloads 500 files/day normally)

Rule 2: Alert if USB device connected
Result: Alert (but employee uses USB daily - false positive)

Outcome: Threat missed OR buried in false positives
```

**UEBA Approach**:
```
Baseline Learning (60-90 days):
- User "john.doe" normally downloads 500 files/day
- Typical file types: Excel, Word (finance dept)
- Normal working hours: 9 AM - 5 PM
- Peer group: Other finance employees

Anomalous Behavior Detected:
Day 1: Downloaded 800 files (60% above baseline) - Risk Score: +10
Day 2: Downloaded files include source code (never accessed before) - Risk Score: +30
Day 3: Working at 11 PM (unusual) - Risk Score: +5
Day 4: Downloaded files to USB (peer group rarely does this) - Risk Score: +15
Day 5: Accessing HR systems (not part of normal role) - Risk Score: +20

Cumulative Risk Score: 80/100
UEBA Alert: "High-confidence insider threat - Data theft preparation"
Context: Employee submitted resignation 2 weeks ago (from HR system)

Outcome: High-fidelity alert with full context
```

### Is UEBA Part of SIEM?

**The Answer: It's Converging**

#### Historical Separation (2010-2015)
```
SIEM: Splunk, QRadar, ArcSight
  ↓ (sends alerts to)
UEBA: Exabeam, Securonix, Gurucul (standalone)
```

#### Modern Integration (2020+)

**Option A: Built into SIEM**
- **Microsoft Sentinel**: Native UEBA (no separate product)
- **Exabeam Fusion**: Started as UEBA, now full SIEM with UEBA native
- **Splunk Enterprise Security**: Can enable UEBA module

**Option B: SIEM Add-on Module**
- **Splunk UEBA**: Separate license, integrates with Splunk ES
- **IBM QRadar User Behavior Analytics**: Add-on module
- **LogRhythm UEBA**: Integrated module

**Option C: Separate Best-of-Breed**
- **Exabeam** (UEBA) + **Splunk** (SIEM)
- **Securonix** (UEBA) + **QRadar** (SIEM)
- Allows specialized UEBA with any SIEM

### UEBA Architecture Within SIEM Ecosystem

```
┌────────────────────────────────────────────────────┐
│               SIEM Platform                        │
│                                                    │
│  ┌──────────────────────────────────────────┐    │
│  │    Correlation Engine Layer              │    │
│  │                                          │    │
│  │  ┌────────────────────────────────────┐ │    │
│  │  │ 1. Rule-Based Correlation         │ │    │
│  │  │    (Traditional IF-THEN logic)    │ │    │
│  │  └────────────────────────────────────┘ │    │
│  │                                          │    │
│  │  ┌────────────────────────────────────┐ │    │
│  │  │ 2. Statistical Analysis           │ │    │
│  │  │    (Thresholds, baselines)        │ │    │
│  │  └────────────────────────────────────┘ │    │
│  │                                          │    │
│  │  ┌────────────────────────────────────┐ │    │
│  │  │ 3. UEBA Engine                    │ │    │
│  │  │    (ML-based behavior analytics)  │ │    │
│  │  │    ← This could be built-in or    │ │    │
│  │  │      separate module               │ │    │
│  │  └────────────────────────────────────┘ │    │
│  │                                          │    │
│  │  ┌────────────────────────────────────┐ │    │
│  │  │ 4. Threat Intelligence Matching   │ │    │
│  │  │    (IOC correlation)              │ │    │
│  │  └────────────────────────────────────┘ │    │
│  │                                          │    │
│  │  ┌────────────────────────────────────┐ │    │
│  │  │ 5. AI/ML Custom Models            │ │    │
│  │  │    (Proprietary algorithms)       │ │    │
│  │  └────────────────────────────────────┘ │    │
│  └──────────────────────────────────────────┘    │
└────────────────────────────────────────────────────┘

All these processors work together in modern SIEM!
```

---

## cyDNA and Advanced Threat Detection

### What is cyDNA?

**cyDNA** (Cyber Defense Neural Analytics) is a newer term for **genetic/DNA-inspired threat detection algorithms**.

**Concept**: Model threats like biological DNA - identifying "genetic markers" of attacks

### cyDNA vs Traditional Detection

**Traditional Signature Detection**:
```
IF (MD5 hash = known_malware_hash) THEN block
Problem: Slight file modification = new hash = missed detection
```

**cyDNA Approach**:
```
Extract "genetic markers" of malware family:
- Behavioral patterns (like genes)
- Code structure patterns
- Communication patterns
- Execution sequences

Even if malware is modified, genetic markers remain
Result: Detect malware variants without exact signatures
```

### Is cyDNA Part of SIEM?

**Short Answer**: **Usually NOT directly in SIEM** - it's typically in EDR/sandbox/specialized engines that **feed results TO SIEM**

**Architecture**:
```
┌─────────────────────────────────┐
│   Specialized cyDNA Engine      │
│                                 │
│   • Malware analysis            │
│   • Genetic pattern matching    │
│   • Variant detection           │
│                                 │
│   Examples:                     │
│   - Intezer (DNA-based)         │
│   - CrowdStrike (behavioral DNA)│
│   - Cylance (AI prevention)     │
└────────────┬────────────────────┘
             │
             │ Sends detections/alerts
             ↓
┌────────────────────────────────┐
│          SIEM                  │
│                                │
│  • Receives cyDNA alerts       │
│  • Correlates with other data  │
│  • Adds context                │
│  • Generates incidents         │
└────────────────────────────────┘
```

### Real-World Example: CrowdStrike Falcon's Approach

**CrowdStrike Falcon** uses "Indicators of Attack" (IOAs) - similar concept to cyDNA:

```
Traditional AV: Looks for malware file hash
CrowdStrike IOA: Looks for attack behavior patterns

Example Attack Pattern (IOA):
1. Process: powershell.exe spawned by WINWORD.EXE
2. Behavior: Download file from internet
3. Behavior: Execute downloaded file
4. Behavior: Create scheduled task
5. Behavior: Establish outbound connection

Even if exact malware is new, the attack pattern matches
known ransomware/backdoor "behavioral DNA"

CrowdStrike detects → Sends alert to SIEM
SIEM correlates: This user also accessed sensitive data today
Combined Alert: "Ransomware + Data Exfiltration Risk"
```

**Conclusion**: cyDNA/behavioral engines are typically **outside SIEM** in specialized tools (EDR, sandbox, malware analysis platforms) but **send enriched alerts TO SIEM** for correlation.

---

## AI/ML Analytics Engines

### The AI/ML Spectrum in Security

AI/ML is a **broad umbrella** - let's break it down:

```
Intelligence Level: Simple → Sophisticated

Level 1: Rule-Based (Not ML)
├─ Static correlation rules
├─ Threshold-based alerts
└─ Example: IF failed_logins > 5 THEN alert

Level 2: Statistical Analysis (Basic ML)
├─ Standard deviation calculations
├─ Moving averages
├─ Outlier detection
└─ Example: Alert if traffic > 3 std deviations from mean

Level 3: Classical Machine Learning
├─ Supervised learning (trained models)
├─ Unsupervised learning (clustering, anomaly detection)
├─ Decision trees, random forests
└─ Example: UEBA behavioral baselines

Level 4: Deep Learning / Neural Networks
├─ Complex pattern recognition
├─ NLP for log analysis
├─ Recurrent neural networks for sequences
└─ Example: Detect malicious code in PowerShell

Level 5: Advanced AI (Proprietary)
├─ Vendor-specific algorithms
├─ Ensemble models (multiple ML techniques)
├─ Continuous learning systems
└─ Example: Darktrace's "Enterprise Immune System"
```

### Are All These Part of SIEM?

**Modern SIEM platforms include multiple levels**:

#### Tier 1 SIEM (Traditional)
```
✓ Level 1: Rule-based correlation
✓ Level 2: Basic statistics
✗ Level 3-5: Not included (rely on external tools)

Example: Legacy ArcSight, older QRadar
```

#### Tier 2 SIEM (Modern with ML)
```
✓ Level 1: Rule-based correlation
✓ Level 2: Statistical analysis
✓ Level 3: Classical ML (built-in UEBA, anomaly detection)
~ Level 4-5: Partial (some deep learning features)

Example: Splunk ES with ML Toolkit, IBM QRadar with Watson
```

#### Tier 3 SIEM (AI-Native)
```
✓ Level 1: Rule-based correlation
✓ Level 2: Statistical analysis
✓ Level 3: Classical ML
✓ Level 4: Deep learning
✓ Level 5: Proprietary AI

Example: Microsoft Sentinel (Azure ML integrated),
         Exabeam (AI-native), Securonix (ML-first design)
```

### Where Does Each Processor Live?

| Processor Type | Typically Part of SIEM? | Common Placement |
|----------------|------------------------|------------------|
| **Basic correlation rules** | ✓ Yes - Core SIEM | SIEM engine |
| **Statistical thresholds** | ✓ Yes - Core SIEM | SIEM engine |
| **UEBA** | ⚡ Converging - Often yes now | SIEM module or integrated |
| **Threat Intelligence** | ⚡ Hybrid | SIEM queries external TIP |
| **ML Anomaly Detection** | ⚡ Converging - Often yes | SIEM ML module |
| **cyDNA / Behavioral** | ✗ Usually separate | EDR, Sandbox, specialized tools |
| **Deep Learning NLP** | ~ Partial | SIEM vendors adding this |
| **Proprietary AI** | ~ Depends on vendor | Some in SIEM, some separate |

---

## The Intelligence Spectrum: From Simple to Sophisticated

Let's map the **complete intelligence processing spectrum** and where each typically lives:

### The Intelligence Stack

```
┌─────────────────────────────────────────────────────────────┐
│  LEVEL 5: Advanced Proprietary AI                           │
│  ┌────────────────────────────────────────────────────┐    │
│  │ Vendor-Specific Algorithms                         │    │
│  │ Examples:                                          │    │
│  │ • Darktrace "Enterprise Immune System"            │    │
│  │ • Vectra AI "Cognito" platform                    │    │
│  │ • Securonix "Autonomous Threat Sweeper"           │    │
│  │                                                    │    │
│  │ Typically: Separate specialized platforms         │    │
│  │ Integration: Send alerts TO SIEM                  │    │
│  └────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  LEVEL 4: Deep Learning & Neural Networks                  │
│  ┌────────────────────────────────────────────────────┐    │
│  │ • Natural Language Processing (log analysis)      │    │
│  │ • Image recognition (phishing detection)          │    │
│  │ • Sequence prediction (attack progression)        │    │
│  │ • Generative models (synthetic threat scenarios)  │    │
│  │                                                    │    │
│  │ Examples:                                          │    │
│  │ • Microsoft Sentinel (Azure ML backend)           │    │
│  │ • Splunk Deep Learning Toolkit                    │    │
│  │ • Custom TensorFlow/PyTorch models                │    │
│  │                                                    │    │
│  │ Typically: Hybrid (some in SIEM, some external)   │    │
│  └────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  LEVEL 3: Classical Machine Learning (UEBA)                │
│  ┌────────────────────────────────────────────────────┐    │
│  │ • Supervised Learning (trained classifiers)       │    │
│  │ • Unsupervised Learning (clustering, anomalies)   │    │
│  │ • Random Forests, Decision Trees                  │    │
│  │ • K-means clustering, PCA                         │    │
│  │                                                    │    │
│  │ Examples:                                          │    │
│  │ • Splunk UEBA (built-in or add-on)               │    │
│  │ • Exabeam Advanced Analytics                      │    │
│  │ • QRadar User Behavior Analytics                  │    │
│  │ • Securonix UEBA                                  │    │
│  │                                                    │    │
│  │ Typically: Integrated into modern SIEM or add-on  │    │
│  └────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  LEVEL 2: Statistical Analysis                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │ • Standard deviation, mean, median                │    │
│  │ • Time-series analysis                            │    │
│  │ • Outlier detection (Z-score, IQR)                │    │
│  │ • Frequency analysis                              │    │
│  │ • Trending and forecasting                        │    │
│  │                                                    │    │
│  │ Examples:                                          │    │
│  │ • Splunk's stats, timechart, outlier commands     │    │
│  │ • QRadar's statistical baselines                  │    │
│  │ • All modern SIEMs have this built-in             │    │
│  │                                                    │    │
│  │ Typically: Core SIEM functionality                │    │
│  └────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  LEVEL 1: Rule-Based Correlation (Traditional)             │
│  ┌────────────────────────────────────────────────────┐    │
│  │ • Boolean logic (IF-THEN-ELSE)                    │    │
│  │ • Pattern matching (regex)                        │    │
│  │ • Threshold-based rules                           │    │
│  │ • Time-windowed correlation                       │    │
│  │ • Static signatures                               │    │
│  │                                                    │    │
│  │ Examples:                                          │    │
│  │ • All SIEM correlation rules                      │    │
│  │ • Traditional IDS/IPS signatures                  │    │
│  │ • Firewall policy rules                           │    │
│  │                                                    │    │
│  │ Typically: Universal - every SIEM has this        │    │
│  └────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### The "SIEM Umbrella" Question

**Your Question**: Do all these intelligence processors fall under the SIEM umbrella?

**The Answer**: **Yes, but with nuances**

```
Traditional View (2000-2015):
┌─────────────────┐
│  SIEM           │
│  = Level 1 & 2  │
│  (Rules + Stats)│
└─────────────────┘

Everything else (UEBA, AI/ML) was OUTSIDE the SIEM

Modern View (2020+):
┌────────────────────────────────────────┐
│  Modern SIEM Platform                  │
│  ┌──────────────────────────────────┐ │
│  │ Level 1: Rule Engine (core)     │ │
│  ├──────────────────────────────────┤ │
│  │ Level 2: Stats Engine (core)    │ │
│  ├──────────────────────────────────┤ │
│  │ Level 3: ML/UEBA (integrated)   │ │
│  ├──────────────────────────────────┤ │
│  │ Level 4: Deep Learning (partial) │ │
│  └──────────────────────────────────┘ │
│                                        │
│  Level 5: Still external (for now)    │
└────────────────────────────────────────┘

The SIEM umbrella is EXPANDING!
```

---

## Are These Part of SIEM or Separate?

### The Definitive Answer

**It depends on your architecture choice and vendor!**

Let me break this down clearly:

### Scenario 1: All-in-One SIEM (Integrated Intelligence)

**Philosophy**: "SIEM is a unified security analytics platform with all intelligence built-in"

**Architecture**:
```
┌──────────────────────────────────────────────────┐
│         Unified SIEM Platform                    │
│                                                  │
│  ┌────────────────────────────────────────────┐ │
│  │  Data Ingestion Layer                     │ │
│  └────────────────────────────────────────────┘ │
│                     ↓                            │
│  ┌────────────────────────────────────────────┐ │
│  │  Normalization & Enrichment               │ │
│  └────────────────────────────────────────────┘ │
│                     ↓                            │
│  ┌────────────────────────────────────────────┐ │
│  │  Intelligence Processing Layer            │ │
│  │                                            │ │
│  │  ┌──────────────────────────────────────┐ │ │
│  │  │ Rule-Based Correlation Engine       │ │ │
│  │  └──────────────────────────────────────┘ │ │
│  │  ┌──────────────────────────────────────┐ │ │
│  │  │ Statistical Analytics Engine        │ │ │
│  │  └──────────────────────────────────────┘ │ │
│  │  ┌──────────────────────────────────────┐ │ │
│  │  │ UEBA/ML Engine (INTEGRATED)         │ │ │
│  │  └──────────────────────────────────────┘ │ │
│  │  ┌──────────────────────────────────────┐ │ │
│  │  │ Threat Intelligence (INTEGRATED)    │ │ │
│  │  └──────────────────────────────────────┘ │ │
│  │  ┌──────────────────────────────────────┐ │ │
│  │  │ AI/ML Models (INTEGRATED)           │ │ │
│  │  └──────────────────────────────────────┘ │ │
│  │                                            │ │
│  │  All intelligence under one platform!     │ │
│  └────────────────────────────────────────────┘ │
│                     ↓                            │
│  ┌────────────────────────────────────────────┐ │
│  │  Alert Generation & Response              │ │
│  └────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────┘

Examples:
• Microsoft Sentinel (UEBA, ML, TI all built-in)
• Exabeam Fusion (started as UEBA, now full SIEM)
• Securonix (ML-native SIEM platform)
```

**Pros**:
- ✓ Single pane of glass
- ✓ No integration complexity
- ✓ Unified licensing
- ✓ Consistent data model

**Cons**:
- ✗ Vendor lock-in
- ✗ May not be "best-of-breed" for each component
- ✗ All-or-nothing approach

---

### Scenario 2: Modular SIEM (SIEM Core + Intelligent Modules)

**Philosophy**: "SIEM core handles data, optional intelligence modules for advanced analytics"

**Architecture**:
```
┌──────────────────────────────────────────────────┐
│         SIEM Core Platform                       │
│  ┌────────────────────────────────────────────┐ │
│  │  Data Ingestion & Storage                 │ │
│  │  Normalization & Basic Correlation        │ │
│  └────────────────────────────────────────────┘ │
└─────────────┬────────────────────────────────────┘
              │
              │ API / Integration Layer
              │
    ┌─────────┴──────────┬───────────┬──────────┐
    ▼                    ▼           ▼          ▼
┌─────────┐      ┌──────────┐  ┌─────────┐  ┌─────────┐
│  UEBA   │      │  AI/ML   │  │  TIP    │  │ Custom  │
│ Module  │      │  Toolkit │  │ Module  │  │ Scripts │
│         │      │          │  │         │  │         │
│ Add-on  │      │  Add-on  │  │ Add-on  │  │ Add-on  │
│Purchase │      │  Free/   │  │Purchase │  │  DIY    │
│         │      │  Paid    │  │         │  │         │
└─────────┘      └──────────┘  └─────────┘  └─────────┘

Examples:
• Splunk ES (core) + Splunk UEBA (add-on) + ML Toolkit
• IBM QRadar (core) + QRadar UBA (add-on) + Watson
• Elastic SIEM + ML modules + custom models
```

**Pros**:
- ✓ Flexible - add modules as needed
- ✓ Can mix vendor modules with custom code
- ✓ Pay for what you use

**Cons**:
- ~ Additional cost per module
- ~ Moderate integration complexity
- ~ May require separate training

---

### Scenario 3: Best-of-Breed (SIEM + Separate Intelligence Platforms)

**Philosophy**: "Use specialized tools for each intelligence function, SIEM orchestrates"

**Architecture**:
```
                    ┌──────────────┐
                    │   SOC Team   │
                    └──────┬───────┘
                           │
                    ┌──────▼───────┐
                    │     SIEM     │
                    │  (Hub/Brain) │
                    └──────┬───────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
    ┌─────▼─────┐    ┌────▼────┐    ┌─────▼─────┐
    │  Exabeam  │    │Darktrace│    │  Anomali  │
    │   UEBA    │    │ AI/ML   │    │    TIP    │
    │ (Separate │    │(Separate│    │ (Separate │
    │ Platform) │    │Platform)│    │ Platform) │
    └───────────┘    └─────────┘    └───────────┘
         ↓                ↓                ↓
    Sends alerts    Sends alerts    Sends IOCs
         ↓                ↓                ↓
         └────────────────┴────────────────┘
                          │
                    ┌─────▼──────┐
                    │    SIEM    │
                    │ Correlates │
                    │    All     │
                    └────────────┘

Examples:
• Splunk Core + Exabeam UEBA + Darktrace AI + Anomali TIP
• ELK Stack + Securonix UEBA + Custom ML + MISP
• QRadar + Gurucul UEBA + Vectra NDR
```

**Pros**:
- ✓ Best-in-class for each function
- ✓ Flexibility to swap vendors
- ✓ Can leverage existing investments

**Cons**:
- ✗ High integration complexity
- ✗ Multiple consoles/interfaces
- ✗ Higher total cost
- ✗ Requires skilled integration team

---

## Modern SIEM Architecture: The Intelligence Stack

### The Complete Modern Stack

Here's how **all intelligence processors** fit together in a modern security operations architecture:

```
┌─────────────────────────────────────────────────────────────────┐
│                    HUMAN LAYER (SOC)                            │
│  Tier 1 Analysts | Tier 2 Analysts | Tier 3 IR | Threat Hunters│
└────────────────────────────┬────────────────────────────────────┘
                             │
                             │ Uses / Investigates
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│              ORCHESTRATION & RESPONSE LAYER                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │    SOAR      │  │  Ticketing   │  │   Playbooks  │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             │ Receives Alerts
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│           SIEM PLATFORM (CENTRAL ANALYTICS HUB)                 │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │         INTELLIGENCE PROCESSING LAYER                     │ │
│  │  (This is where all the "smart" stuff happens!)          │ │
│  │                                                           │ │
│  │  ┌─────────────────────────────────────────────────────┐ │ │
│  │  │ 1. Rule-Based Correlation Engine                   │ │ │
│  │  │    • Boolean logic, pattern matching               │ │ │
│  │  │    • Time-windowed correlation                     │ │ │
│  │  │    • Multi-event chaining                          │ │ │
│  │  │    Status: ✓ Always in SIEM core                   │ │ │
│  │  └─────────────────────────────────────────────────────┘ │ │
│  │                                                           │ │
│  │  ┌─────────────────────────────────────────────────────┐ │ │
│  │  │ 2. Statistical Analytics Engine                    │ │ │
│  │  │    • Baselines, thresholds, deviations             │ │ │
│  │  │    • Time-series analysis                          │ │ │
│  │  │    • Frequency analysis, trending                  │ │ │
│  │  │    Status: ✓ Always in SIEM core                   │ │ │
│  │  └─────────────────────────────────────────────────────┘ │ │
│  │                                                           │ │
│  │  ┌─────────────────────────────────────────────────────┐ │ │
│  │  │ 3. UEBA / Behavioral Analytics Engine             │ │ │
│  │  │    • User baseline modeling                        │ │ │
│  │  │    • Peer group analysis                           │ │ │
│  │  │    • Anomaly detection (ML-based)                  │ │ │
│  │  │    • Risk scoring                                  │ │ │
│  │  │    Status: ⚡ Integrated (modern) OR Separate      │ │ │
│  │  │           (legacy/best-of-breed)                   │ │ │
│  │  └─────────────────────────────────────────────────────┘ │ │
│  │                                                           │ │
│  │  ┌─────────────────────────────────────────────────────┐ │ │
│  │  │ 4. Threat Intelligence Engine                      │ │ │
│  │  │    • IOC matching (IPs, domains, hashes)           │ │ │
│  │  │    • STIX/TAXII feed integration                   │ │ │
│  │  │    • Reputation scoring                            │ │ │
│  │  │    Status: ⚡ Hybrid - SIEM queries external TIP   │ │ │
│  │  │           OR built-in TI module                    │ │ │
│  │  └─────────────────────────────────────────────────────┘ │ │
│  │                                                           │ │
│  │  ┌─────────────────────────────────────────────────────┐ │ │
│  │  │ 5. Machine Learning Models                         │ │ │
│  │  │    • Supervised classification                     │ │ │
│  │  │    • Unsupervised clustering                       │ │ │
│  │  │    • Ensemble methods                              │ │ │
│  │  │    Status: ⚡ Integrated (modern SIEMs) OR         │ │ │
│  │  │           Custom external models                   │ │ │
│  │  └─────────────────────────────────────────────────────┘ │ │
│  │                                                           │ │
│  │  ┌─────────────────────────────────────────────────────┐ │ │
│  │  │ 6. Deep Learning / NLP Engines                     │ │ │
│  │  │    • Log parsing with NLP                          │ │ │
│  │  │    • Sequence prediction                           │ │ │
│  │  │    • Image analysis (phishing)                     │ │ │
│  │  │    Status: ~ Emerging - Some vendors have this     │ │ │
│  │  └─────────────────────────────────────────────────────┘ │ │
│  │                                                           │ │
│  │  ┌─────────────────────────────────────────────────────┐ │ │
│  │  │ 7. Proprietary AI Algorithms                       │ │ │
│  │  │    • Vendor-specific secret sauce                  │ │ │
│  │  │    • Advanced behavioral modeling                  │ │ │
│  │  │    Status: ~ Vendor-dependent                      │ │ │
│  │  └─────────────────────────────────────────────────────┘ │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │         DATA LAYER                                        │ │
│  │  Normalized security data from all sources               │ │
│  └───────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                             ↑
                             │ Data flows IN
                             │
┌─────────────────────────────────────────────────────────────────┐
│            SPECIALIZED PROCESSORS (Outside SIEM)                │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │   cyDNA      │  │   Sandboxes  │  │  Specialized │         │
│  │   Engines    │  │   (Malware   │  │  AI Engines  │         │
│  │              │  │   Analysis)  │  │ (Darktrace)  │         │
│  │ (EDR-based)  │  │              │  │              │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
│         ↓                  ↓                  ↓                 │
│         └──────────────────┴──────────────────┘                │
│                            │                                    │
│                  Detections sent to SIEM                        │
└─────────────────────────────────────────────────────────────────┘
```

### The Key Insight

**Answer to your question**:

**Modern SIEM platforms are expanding to include Levels 1-5 intelligence under the "SIEM umbrella"**, but...

**The degree of integration varies**:

| Intelligence Type | 2015 Status | 2025 Status |
|------------------|-------------|-------------|
| **Rule-based correlation** | ✓ Always in SIEM | ✓ Always in SIEM |
| **Statistical analysis** | ✓ Always in SIEM | ✓ Always in SIEM |
| **UEBA** | ✗ Separate product | ⚡ Usually integrated now |
| **Threat Intelligence** | ~ Queried externally | ⚡ Hybrid (built-in + external) |
| **ML models** | ✗ External/custom | ⚡ Built-in for modern SIEMs |
| **Deep Learning** | ✗ Not available | ~ Some vendors have it |
| **cyDNA** | ✗ Not in SIEM space | ✗ Still in EDR/specialized tools |
| **Proprietary AI** | ✗ Separate platforms | ~ Some vendors integrating |

**The "SIEM umbrella" is getting BIGGER**, absorbing more intelligence functions!

---

## Vendor Approaches Comparison

Let me show you how major vendors handle these intelligence processors:

### Microsoft Sentinel (Cloud-Native, AI-First)

```
┌────────────────────────────────────────────────┐
│        Microsoft Sentinel                      │
│                                                │
│  Built-in Intelligence (No Extra Cost):        │
│  ✓ Rule-based correlation                      │
│  ✓ Statistical analytics                       │
│  ✓ UEBA (native - Fusion ML)                   │
│  ✓ ML models (Azure ML backend)                │
│  ✓ Threat Intelligence (native)                │
│  ✓ Anomaly detection (built-in)                │
│                                                │
│  Philosophy: "Everything under one umbrella"   │
└────────────────────────────────────────────────┘

Approach: Integrated - All intelligence IS the SIEM
```

### Splunk Enterprise Security (Modular)

```
┌────────────────────────────────────────────────┐
│    Splunk Enterprise Security (Core SIEM)      │
│                                                │
│  Included:                                     │
│  ✓ Rule-based correlation (correlation searches│
│  ✓ Statistical analytics (SPL stats commands)  │
│  ✓ Basic ML (ML Toolkit - free)                │
│                                                │
│  Add-ons (Extra Purchase):                     │
│  $ Splunk UEBA (separate product)              │
│  $ Splunk Deep Learning Toolkit                │
│  $ Splunk Threat Intelligence Framework        │
│                                                │
│  Philosophy: "Core + Optional Modules"         │
└────────────────────────────────────────────────┘

Approach: Modular - Intelligence spread across products
```

### IBM QRadar (Traditional with AI Add-ons)

```
┌────────────────────────────────────────────────┐
│         IBM QRadar SIEM (Core)                 │
│                                                │
│  Included:                                     │
│  ✓ Rule-based correlation (extensive)          │
│  ✓ Statistical analytics                       │
│  ✓ Network flow analysis                       │
│                                                │
│  Add-ons:                                      │
│  $ QRadar User Behavior Analytics (UEBA)       │
│  $ Watson for Cyber Security (AI)              │
│  $ QRadar Threat Intelligence (Xforce)         │
│                                                │
│  Philosophy: "Robust core + AI enhancements"   │
└────────────────────────────────────────────────┘

Approach: Core SIEM + Optional AI modules
```

### Exabeam Fusion (UEBA-First, Now Full SIEM)

```
┌────────────────────────────────────────────────┐
│          Exabeam Fusion XDR                    │
│                                                │
│  All Integrated (UEBA is the foundation):      │
│  ✓ UEBA (core DNA of product)                  │
│  ✓ Behavioral analytics (ML-native)            │
│  ✓ Timeline-based investigation                │
│  ✓ Rule-based correlation (added later)        │
│  ✓ Threat Intelligence (built-in)              │
│                                                │
│  Philosophy: "ML-first, SIEM second"           │
│  Started as pure UEBA, evolved to full SIEM    │
└────────────────────────────────────────────────┘

Approach: Fully integrated, ML-native
```

### Securonix (ML-Native SIEM)

```
┌────────────────────────────────────────────────┐
│      Securonix Next-Gen SIEM                   │
│                                                │
│  All Integrated (ML is core):                  │
│  ✓ Big data architecture (Hadoop-based)        │
│  ✓ Advanced UEBA (native)                      │
│  ✓ Machine learning models (extensive)         │
│  ✓ Autonomous threat sweeper (AI)              │
│  ✓ Rule-based correlation                      │
│  ✓ Threat Intelligence (built-in)              │
│                                                │
│  Philosophy: "ML-first SIEM from the ground up"│
└────────────────────────────────────────────────┘

Approach: Fully integrated, AI-native architecture
```

### Elastic Security (Open Source Flexibility)

```
┌────────────────────────────────────────────────┐
│         Elastic Security (SIEM)                │
│                                                │
│  Included (Basic):                             │
│  ✓ Rule-based detection                        │
│  ✓ Statistical queries                         │
│  ✓ Machine Learning (basic - paid tier)        │
│                                                │
│  DIY Extensible:                               │
│  ○ Add your own ML models (Python)             │
│  ○ Integrate external UEBA (APIs)              │
│  ○ Custom threat intel feeds                   │
│  ○ Open source flexibility                     │
│                                                │
│  Philosophy: "Flexible platform, you build it" │
└────────────────────────────────────────────────┘

Approach: Core platform + DIY intelligence
```

---

## Choosing Your Architecture

### Decision Framework

**Question 1: What's your organization size?**

```
Small (< 1,000 employees)
→ Recommendation: Integrated SIEM (Microsoft Sentinel, Exabeam)
→ Rationale: Limited team, need simplicity

Mid-Market (1,000 - 10,000)
→ Recommendation: Modular SIEM (Splunk ES + UEBA module)
→ Rationale: Balance of capability and complexity

Enterprise (10,000+)
→ Recommendation: Best-of-breed OR fully integrated enterprise platform
→ Rationale: Can manage complexity, need best capabilities
```

**Question 2: What's your team's skill level?**

```
Basic SOC Team
→ Recommendation: Integrated SIEM with native ML/UEBA
→ Rationale: Don't need to manage multiple platforms

Advanced SOC Team
→ Recommendation: Best-of-breed approach
→ Rationale: Can integrate and optimize multiple tools

Data Science Team Available
→ Recommendation: Flexible platform (Elastic) + custom ML
→ Rationale: Build custom intelligence
```

**Question 3: What's your budget?**

```
Limited Budget
→ Recommendation: Elastic Security + open source UEBA
→ Rationale: Lower licensing costs

Moderate Budget
→ Recommendation: Cloud-native SIEM (Sentinel) with pay-as-you-go
→ Rationale: Predictable costs, everything included

Large Budget
→ Recommendation: Best-of-breed stack
→ Rationale: Get best tools for each function
```

---

## Conclusion

### Final Answer to Your Questions

**Q1: "SIEM is the brain and correlation engine - what about UEBA, cyDNA, and other intelligent processors?"**

**A1**: Modern SIEM has **evolved from simple correlation to a comprehensive analytics platform**. The "brain" now includes:
- ✓ Traditional correlation (always included)
- ✓ Statistical analytics (always included)
- ⚡ UEBA (converging - usually included in modern SIEMs)
- ⚡ ML/AI models (converging - increasingly included)
- ~ Deep learning (emerging - some vendors have it)
- ✗ cyDNA (typically in EDR/specialized tools, sends alerts TO SIEM)

**Q2: "Do all these intelligent processors (from dump filters to sophisticated AI/ML proprietary algorithms) fall under the SIEM umbrella?"**

**A2**: **YES - but with important nuances**:

### The Modern Answer (2025):

```
┌──────────────────────────────────────────────┐
│       "SIEM Umbrella" (Expanding)            │
│                                              │
│  Core SIEM Platform Now Includes:            │
│  ✓ Rule-based correlation (Level 1)          │
│  ✓ Statistical analysis (Level 2)            │
│  ✓ Classical ML / UEBA (Level 3) ← NEW!      │
│  ✓ Some deep learning (Level 4) ← EMERGING!  │
│  ~ Proprietary AI (Level 5) ← VENDOR-SPECIFIC│
│                                              │
│  Still Outside SIEM:                         │
│  ✗ cyDNA engines (in EDR)                    │
│  ✗ Specialized AI platforms (Darktrace, etc.)│
│  ✗ Sandboxes / malware analysis              │
│                                              │
│  These send their intelligence TO SIEM       │
└──────────────────────────────────────────────┘
```

### Three Valid Architectural Approaches:

1. **All Under SIEM Umbrella** (Integrated)
   - Microsoft Sentinel, Exabeam, Securonix approach
   - All intelligence processors built into SIEM
   - Simplest, most unified

2. **Modular SIEM Umbrella** (Core + Add-ons)
   - Splunk, QRadar approach
   - Core SIEM + optional intelligence modules
   - Flexible, pay for what you need

3. **SIEM as Hub** (Best-of-Breed)
   - SIEM receives alerts from specialized AI platforms
   - Specialized processors remain separate
   - Most complex, potentially best-of-breed

### The Trend

**From dump filters → sophisticated AI: All converging under the SIEM umbrella**

```
2000-2010: SIEM = Dump filters + simple rules
2010-2015: SIEM = Rules + Some intelligence external
2015-2020: SIEM = Rules + UEBA modules
2020-2025: SIEM = Full analytics platform (AI-native)
Future:    SIEM = Autonomous security platform
```

**Your instinct is correct**: Modern SIEMs are becoming **unified intelligence platforms** that include everything from basic filters to sophisticated AI/ML under one umbrella!

The days of "SIEM is just correlation rules" are over. **SIEM is now the unified security analytics and intelligence platform**.

