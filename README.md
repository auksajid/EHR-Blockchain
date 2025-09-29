# EHR-Blockchain
Blockchain based protection of Patient sensitive data
# BBACM Healthcare Network System - User Manual

## Table of Contents
1. [Introduction](#introduction)
2. [System Overview](#system-overview)
3. [Getting Started](#getting-started)
4. [Core Components](#core-components)
5. [User Roles and Permissions](#user-roles-and-permissions)
6. [Key Operations](#key-operations)
7. [Emergency Response System](#emergency-response-system)
8. [Predictive Analytics and Early Warning System](#predictive-analytics-and-early-warning-system)
9. [Data Visualization](#data-visualization)
10. [Best Practices](#best-practices)
11. [Troubleshooting](#troubleshooting)

---

## 1. Introduction

This manual provides comprehensive guidance for using the Blockchain-Based Access Control Model (BBACM) Healthcare Network System. The system is designed to securely manage healthcare data using blockchain technology, providing robust access control, emergency response capabilities, and predictive health analytics.

### Purpose
- Secure management of Personal Health Information (PHI)
- Real-time monitoring of Patient Physiological Parameters (PPPs)
- Role-based access control for healthcare providers
- Emergency response coordination
- Predictive health risk assessment

### Key Features
- Blockchain-based immutable transaction ledger
- Cryptographic security for participant authentication
- Simulated cloud storage for physiological data
- Rule-based early warning system
- Visual analytics for patient monitoring

---

## 2. System Overview

### Architecture
The system consists of several interconnected components:

```
BBACMNetwork (Core)
├── Participants (Users)
├── Assets (Data)
├── Blockchain (Ledger)
├── Access Control List (Permissions)
├── Cryptography Manager (Security)
├── Simulated Cloud Storage (PPPs Data)
└── Predictive Analytics (Warnings)
```

### Technology Stack
- **Python 3.x**: Core programming language
- **cryptography**: RSA key generation and encryption
- **pandas**: Data manipulation and analysis
- **matplotlib/seaborn**: Data visualization
- **hashlib**: Cryptographic hashing

---

## 3. Getting Started

### Installation Requirements

```python
# Required imports
import hashlib
import json
import time
from datetime import datetime
from cryptography.hazmat.primitives import hashes, serialization
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
import os
import base64
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from typing import Dict, List, Optional, Any
```

### Initial Setup

```python
# Initialize the network
network = BBACMNetwork()

# Create an administrator
admin = Admin("ADMIN001", "System Administrator")
network.add_participant(admin)
```

---

## 4. Core Components

### 4.1 Participant Classes

#### Admin
**Purpose**: System administration and PHI management

```python
# Create an admin
admin = Admin("ADMIN001", "Administrator Name")

# Register participants
admin.register_participant(patient)

# Update PHI records
admin.update_phi(phi_asset, {"allergies": ["Penicillin"]})
```

#### SuperAdmin
**Purpose**: Extended administrative privileges (inherits from Admin)

```python
# Create a super admin
super_admin = SuperAdmin("SADMIN001", "Super Admin Name")
```

#### Patient
**Purpose**: Data ownership and upload permissions

```python
# Create a patient
patient = Patient("PAT001", "Patient Name", "indoor")  # or "outdoor"
network.add_participant(patient)

# Patient types:
# - "indoor": Hospitalized patients
# - "outdoor": Outpatients
```

#### MedicalEntity
**Purpose**: Healthcare professionals with specific access rights

```python
# Create medical entity
doctor = MedicalEntity(
    "DOC001",
    "Dr. Smith",
    "doctor",  # designation: doctor, nurse, specialist
    "Cardiology Department"
)
network.add_participant(doctor)
```

#### EmergencyResponder
**Purpose**: Temporary access during emergencies

```python
# Create emergency responder
paramedic = EmergencyResponder("ER001", "Paramedic Name", "Paramedic")
network.add_participant(paramedic)
```

### 4.2 Asset Classes

#### PHI (Personal Health Information)
**Contains**: Demographic and medical history data

```python
# PHI structure
phi_data = {
    "patientrecordid": "PAT001_PHI",
    "name": "Patient Name",
    "gender": "Male",  # or "Female"
    "dob": "1990-01-01",  # YYYY-MM-DD format
    "address": "Patient Address",
    "contact_no": "XXX-XXX-XXXX",
    "email": "patient@example.com"
}
```

**Medical Conditions**:
- heart_disease
- stroke
- lower_respiratory_infection
- diabetes
- cancer
- alzheimers
- unintentional_injuries
- kidney_disease
- other (text field)

#### PPPs (Patient Physiological Parameters)
**Contains**: Real-time vital signs and measurements

```python
# PPPs data structure
ppps_data = {
    "blood_pressure": "120/80",
    "body_temperature": "98.6°F",
    "pulse": "72 bpm",
    "heart_rate": "75 bpm",
    "eeg": "Normal",  # optional
    "ecg": "Normal rhythm"  # optional
}
```

---

## 5. User Roles and Permissions

### Access Control Matrix

| Role | PHI | PPPs | MEI |
|------|-----|------|-----|
| **Admin** | CREATE, READ, UPDATE, DELETE | CREATE, READ, UPDATE, DELETE | CREATE, READ, UPDATE, DELETE |
| **Patient** | CREATE, READ | CREATE, READ | - |
| **MedicalEntity** | READ | READ, UPDATE | - |
| **EmergencyResponder** | READ (during emergency) | READ (during emergency) | - |

### Permission Checking

```python
# Check if action is permitted
can_access = network.access_control.check_permission(
    "MedicalEntity",  # participant type
    "PHI",           # asset type
    "READ"           # action
)
```

---

## 6. Key Operations

### 6.1 Upload PHI

**Who can perform**: Patients (with Admin/MedicalEntity authorization)

```python
# Step 1: Prepare PHI data
phi_data = {
    "patientrecordid": "PAT001_PHI",
    "name": "John Doe",
    "gender": "Male",
    "dob": "1985-05-15",
    "address": "123 Main St",
    "contact_no": "555-1234",
    "email": "john@example.com"
}

# Step 2: Upload PHI
phi_asset = network.upload_phi(patient, phi_data, doctor)

# Step 3: Add medical conditions (optional)
phi_asset.medical_conditions["heart_disease"] = True
phi_asset.medical_conditions["diabetes"] = True
phi_asset.allergies = ["Penicillin", "Sulfa drugs"]
```

### 6.2 Upload PPPs

**Who can perform**: Patients, MedicalEntities

```python
# Upload physiological parameters
ppps_data = {
    "blood_pressure": "130/85",
    "body_temperature": "99.1°F",
    "pulse": "80 bpm",
    "heart_rate": "82 bpm"
}

network.upload_ppps(patient, ppps_data)
```

**Note**: PPPs data is stored in both blockchain and simulated cloud storage.

### 6.3 Access PHI

**Who can perform**: Authorized MedicalEntities

```python
# Access patient PHI
try:
    phi_asset = network.access_phi(doctor, "PAT001_PHI")
    print(f"Patient: {phi_asset.name}")
    print(f"Conditions: {phi_asset.medical_conditions}")
except PermissionError as e:
    print(f"Access denied: {e}")
```

### 6.4 Update PHI

**Who can perform**: Admins

```python
# Update PHI data
updates = {
    "allergies": ["Penicillin", "Latex"],
    "referredby": "Dr. Johnson",
    "medical_conditions": {
        "heart_disease": True,
        "diabetes": False,
        # ... other conditions
    }
}

network.update_phi(admin, "PAT001_PHI", updates)
```

### 6.5 Transfer Rights

**Who can perform**: MedicalEntities with existing access

```python
# Transfer access rights from doctor to nurse
network.transfer_rights(doctor, nurse, "PAT001_PHI")
```

### 6.6 Mine Block

**Who can perform**: System/Admin

```python
# Mine a block with pending transactions
network.mine_block()
```

### 6.7 View Transaction History

```python
# Get all transactions
all_transactions = network.get_transaction_history()

# Get transactions for specific participant
patient_transactions = network.get_transaction_history("PAT001")

# Display transactions
for tx in all_transactions:
    print(f"Type: {tx['type']}")
    print(f"Participant: {tx['participant']}")
    print(f"Asset: {tx['asset']}")
    print(f"Timestamp: {tx['timestamp']}")
    print(f"Data: {tx['data']}")
    print()
```

---

## 7. Emergency Response System

### 7.1 Triggering an Emergency

**Purpose**: Grant temporary access to emergency responders

```python
# Step 1: Define emergency responders
emergency_responders = [paramedic1, paramedic2]

# Step 2: Trigger emergency
network.trigger_emergency("PAT001", emergency_responders)

# Result: Responders gain temporary access to PHI and PPPs
```

### 7.2 Emergency Responder Access

```python
# During emergency, responders can access data
phi_asset = network.access_phi(paramedic1, "PAT001_PHI")
```

### 7.3 Resolving an Emergency

```python
# Revoke temporary access
network.resolve_emergency("PAT001", emergency_responders)
```

### Emergency Transaction Types
- **EmergencyTriggered**: Logged when emergency is activated
- **EmergencyResolved**: Logged when emergency is deactivated

---

## 8. Predictive Analytics and Early Warning System

### 8.1 How It Works

The system uses a rule-based model to analyze physiological parameters and detect potential health risks.

### 8.2 Warning Thresholds

| Parameter | Low Threshold | High Threshold |
|-----------|---------------|----------------|
| Blood Pressure (Systolic) | < 90 mmHg | > 140 mmHg |
| Blood Pressure (Diastolic) | < 60 mmHg | > 90 mmHg |
| Pulse | < 50 bpm | > 100 bpm |
| Heart Rate | < 50 bpm | > 100 bpm |
| Body Temperature | < 95°F | > 100.4°F |

### 8.3 Performing Analysis

```python
# Analyze patient data
network.perform_predictive_analysis("PAT001")

# Output shows:
# - "No potential health warnings detected" (if normal)
# - List of warnings (if thresholds exceeded)
```

### 8.4 Early Warning Transactions

When warnings are detected:
- **EarlyWarning** transaction is created
- Contains patient ID, warnings, and latest readings
- Added to pending transactions
- Logged in blockchain after mining

### 8.5 Example Scenarios

**Normal Readings**:
```python
ppps_data = {
    "blood_pressure": "120/80",
    "body_temperature": "98.6°F",
    "pulse": "72 bpm"
}
# Result: No warnings
```

**Critical Readings**:
```python
ppps_data = {
    "blood_pressure": "180/110",  # HIGH
    "body_temperature": "101.5°F",  # HIGH
    "pulse": "120 bpm"  # HIGH
}
# Result: Multiple warnings triggered
```

---

## 9. Data Visualization

### 9.1 Visualizing Patient Data

```python
# Generate visualization
visualize_ppps_data(network, "PAT001")
```

### 9.2 Visualization Features

**Includes**:
- Time-series plots for each vital sign
- Red markers for individual data points
- Orange vertical lines indicating warnings
- Warning messages annotated on plots
- Grid lines for easier reading

**Supported Parameters**:
- Blood Pressure
- Body Temperature
- Pulse
- Heart Rate

### 9.3 Interpreting Plots

- **X-axis**: Timestamp of readings
- **Y-axis**: Parameter value
- **Red dots**: Individual measurements
- **Orange dashed lines**: Warning triggered at this time
- **Orange text**: Description of warning

---

## 10. Best Practices

### 10.1 Data Management

1. **Always mine blocks regularly** to commit transactions
   ```python
   network.mine_block()
   ```

2. **Upload PPPs data frequently** for effective monitoring
   - Recommended: Every 15-30 minutes for critical patients
   - Minimum: Every 2-4 hours for stable patients

3. **Keep PHI updated** with current medical information
   ```python
   network.update_phi(admin, phi_asset_id, updates)
   ```

### 10.2 Access Control

1. **Grant minimum necessary permissions**
   - Only authorize required medical entities
   - Transfer rights only when necessary

2. **Review access logs regularly**
   ```python
   transactions = network.get_transaction_history()
   access_logs = [tx for tx in transactions if tx['type'] == 'AccessPHI']
   ```

3. **Revoke access when no longer needed**
   - Use emergency resolution for temporary access
   - Remove from authorized_entities list for permanent revocation

### 10.3 Emergency Response

1. **Define emergency responder teams in advance**
   ```python
   emergency_team = [paramedic1, paramedic2, emt1]
   ```

2. **Always resolve emergencies** to revoke temporary access
   ```python
   network.resolve_emergency(patient_id, emergency_team)
   ```

3. **Document emergency events**
   - Review EmergencyTriggered and EmergencyResolved transactions
   - Analyze response times

### 10.4 Predictive Analytics

1. **Run analysis after each PPPs upload**
   ```python
   network.upload_ppps(patient, ppps_data)
   network.perform_predictive_analysis(patient.participant_id)
   ```

2. **Review EarlyWarning transactions daily**
   ```python
   warnings = [tx for tx in network.get_transaction_history() 
               if tx['type'] == 'EarlyWarning']
   ```

3. **Customize thresholds** for individual patients when needed
   - Modify `check_for_warnings()` function
   - Consider patient-specific baselines

---

## 11. Troubleshooting

### 11.1 Common Errors

#### PermissionError: "Patient does not have permission to create PHI"
**Cause**: Access control check failed  
**Solution**: Ensure proper participant type is used

```python
# Correct:
network.upload_phi(patient, phi_data, doctor)

# Incorrect:
network.upload_phi(doctor, phi_data, patient)  # Wrong order
```

#### ValueError: "PHI asset not found"
**Cause**: Asset ID doesn't exist in network  
**Solution**: Verify asset ID format

```python
# Correct format: "PAT001_PHI"
phi_asset_id = f"{patient.participant_id}_PHI"
```

#### PermissionError: "Medical entity not authorized to access this PHI"
**Cause**: Medical entity not in authorized_entities list  
**Solution**: Grant access first

```python
network.access_control.grant_access(phi_asset, doctor)
# Then access
network.access_phi(doctor, phi_asset.asset_id)
```

### 11.2 Data Issues

#### No PPPs data for visualization
**Cause**: No data uploaded to cloud storage  
**Solution**: Upload PPPs data first

```python
network.upload_ppps(patient, ppps_data)
# Then visualize
visualize_ppps_data(network, patient.participant_id)
```

#### Warnings not triggering
**Cause**: Values within normal thresholds  
**Solution**: Check threshold definitions in `check_for_warnings()`

#### Cannot parse physiological values
**Cause**: Incorrect data format  
**Solution**: Use correct formats

```python
# Correct formats:
"blood_pressure": "120/80"  # Not "120-80" or "120 over 80"
"pulse": "72 bpm"           # Not "72" or "72beats/min"
"body_temperature": "98.6°F" # Not "98.6" or "37C"
```

### 11.3 Performance Issues

#### Slow block mining
**Cause**: Large number of pending transactions  
**Solution**: Mine blocks more frequently

```python
# Mine after every 10-20 transactions
if len(network.pending_transactions) >= 10:
    network.mine_block()
```

#### Memory usage high
**Cause**: Large blockchain or cloud storage  
**Solution**: Implement data archival (not in current implementation)

---

## 12. Complete Example Workflow

### Scenario: New Patient Admission

```python
# 1. Initialize network
network = BBACMNetwork()

# 2. Create participants
admin = Admin("ADMIN001", "System Admin")
patient = Patient("PAT001", "John Doe", "indoor")
doctor = MedicalEntity("DOC001", "Dr. Smith", "doctor", "Cardiology")
nurse = MedicalEntity("NUR001", "Nurse Johnson", "nurse", "Cardiology")

network.add_participant(admin)
network.add_participant(patient)
network.add_participant(doctor)
network.add_participant(nurse)

# 3. Upload PHI
phi_data = {
    "patientrecordid": "PAT001_PHI",
    "name": "John Doe",
    "gender": "Male",
    "dob": "1970-05-15",
    "address": "123 Main St",
    "contact_no": "555-1234",
    "email": "john@example.com"
}
phi_asset = network.upload_phi(patient, phi_data, doctor)
phi_asset.medical_conditions["heart_disease"] = True

# 4. Upload initial vitals
ppps_data = {
    "blood_pressure": "140/90",
    "body_temperature": "98.6°F",
    "pulse": "85 bpm",
    "heart_rate": "88 bpm"
}
network.upload_ppps(patient, ppps_data)

# 5. Perform predictive analysis
network.perform_predictive_analysis(patient.participant_id)

# 6. Doctor accesses PHI
accessed_phi = network.access_phi(doctor, phi_asset.asset_id)

# 7. Transfer rights to nurse
network.transfer_rights(doctor, nurse, phi_asset.asset_id)

# 8. Upload follow-up vitals
ppps_data_followup = {
    "blood_pressure": "135/85",
    "body_temperature": "98.4°F",
    "pulse": "80 bpm",
    "heart_rate": "82 bpm"
}
network.upload_ppps(patient, ppps_data_followup)
network.perform_predictive_analysis(patient.participant_id)

# 9. Mine block
network.mine_block()

# 10. View transaction history
transactions = network.get_transaction_history(patient.participant_id)
for tx in transactions:
    print(f"{tx['type']}: {tx['data']}")

# 11. Visualize patient data
visualize_ppps_data(network, patient.participant_id)
```

---

## Appendix A: Transaction Types Reference

| Transaction Type | Created By | Description |
|-----------------|------------|-------------|
| UploadPHI | Patient | PHI asset created and stored |
| UploadPPPs | Patient/System | Physiological data uploaded |
| AccessPHI | MedicalEntity | PHI accessed by authorized entity |
| UpdatePHI | Admin | PHI data modified |
| TransferRights | MedicalEntity | Access rights transferred |
| EmergencyTriggered | System | Emergency response activated |
| EmergencyResolved | System | Emergency response deactivated |
| EarlyWarning | Predictive System | Health risk detected |

---

## Appendix B: Quick Reference Commands

```python
# Initialization
network = BBACMNetwork()

# Create participants
admin = Admin("ID", "Name")
patient = Patient("ID", "Name", "type")
doctor = MedicalEntity("ID", "Name", "designation", "department")
paramedic = EmergencyResponder("ID", "Name", "specialization")

# Data operations
network.upload_phi(patient, phi_data, doctor)
network.upload_ppps(patient, ppps_data)
network.access_phi(doctor, phi_asset_id)
network.update_phi(admin, phi_asset_id, updates)
network.transfer_rights(doctor, nurse, phi_asset_id)

# Emergency
network.trigger_emergency(patient_id, [responder1, responder2])
network.resolve_emergency(patient_id, [responder1, responder2])

# Analytics
network.perform_predictive_analysis(patient_id)

# Blockchain
network.mine_block()
network.get_transaction_history(participant_id)

# Visualization
visualize_ppps_data(network, patient_id)
```

---

**Document Version**: 1.0  
**Last Updated**: Based on code version from document  
**For Support**: Review transaction logs and system output messages for detailed debugging information
