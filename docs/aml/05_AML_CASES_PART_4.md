# AML Cases
## Part 4 — Behavioral AML (Geo, Login, Device)

---
Version: 1.0

Status: Draft

Depends on

- Graph Model
- Canonical Business Model

---

# Purpose

This document introduces Behavioral AML.

Unlike traditional AML, these cases are generated from customer behavior instead of financial movements only.

Behavioral signals increase or decrease the confidence of an AML investigation.

They do not replace financial evidence.

---

# Behavioral Layer

Current future nodes

```
Login

Session

GeoLocation

Device

IPAddress
```

Behavioral AML always complements financial AML.

---

# Behavioral Risk Categories

```
Location

Device

Velocity

Behavior Drift

Relationship
```

---

# Case 7

## AML_IMPOSSIBLE_TRAVEL

### Objective

Detect impossible travel between consecutive logins.

---

## Business Question

```
Could the customer physically travel between these two logins?
```

---

## Required Data

```
Login Timestamp

Latitude

Longitude

Country

City

Device
```

---

## Detection

```
Login A

↓

Distance

↓

Login B

↓

Travel Time

↓

Required Speed
```

If

```
Required Speed

>

Configured Maximum
```

Generate case.

---

## Features

```
distance_km

minutes_between

required_speed

country_change

device_change
```

---

## Severity

LOW

```
Different cities
```

MEDIUM

```
Different states
```

HIGH

```
Different countries
```

CRITICAL

```
Intercontinental
+
Short interval
```

---

# Graph

```
Client

↓

Login

↓

Geo

↓

Login

↓

Geo
```

---

# Case 8

## AML_NEW_COUNTRY_LOGIN

### Objective

Detect login from a country never observed before.

---

## Detection

```
Current Country

NOT IN

Historical Countries
```

---

## Features

```
new_country

login_count

country_frequency

days_since_last_login
```

---

## Future

Increase severity when

```
High Risk Country

OFAC

FATF

Sanctioned Jurisdiction
```

---

# Graph

```
Client

↓

Login

↓

GeoCountry
```

---

# Case 9

## AML_COUNTRY_DRIFT

### Objective

Detect gradual migration of login behavior.

---

Example

Historical

```
Mexico
```

Recent

```
Turkey

Romania

UAE
```

---

## Features

```
country_entropy

new_country_ratio

travel_velocity

country_count
```

---

# Case 10

## AML_LOGIN_TIME_DRIFT

### Objective

Detect changes in login schedule.

---

Historical

```
08:00

↓

18:00
```

Observed

```
03:00

↓

04:00
```

---

## Features

```
hour_distribution

night_login_ratio

weekend_ratio

variance
```

---

# Case 11

## AML_LOGIN_FREQUENCY_SPIKE

### Objective

Detect sudden increase in login activity.

---

Historical

```
2 per week
```

Observed

```
60 per day
```

---

## Features

```
historical_rate

current_rate

ratio

burst_score
```

---

# Case 12

## AML_NEW_DEVICE_NEW_GEO

### Objective

Detect simultaneous appearance of

```
New Device

+

New Geography
```

---

Business Question

```
Did the customer suddenly appear from a new location using a new device?
```

---

Graph

```
Client

↓

Login

↓

Device

↓

Geo
```

---

## Features

```
new_device

new_country

new_city

device_age

geo_age
```

---

## Severity

HIGH

Default.

CRITICAL

If combined with

```
Withdrawal

New External Account

Round Trip
```

---

# Case 13

## AML_SHARED_DEVICE

### Objective

Detect multiple customers using the same device.

---

Graph

```
Client A

↓

Device

↑

Client B

↑

Client C
```

---

## Features

```
client_count

login_count

country_count

device_age
```

---

## Severity

LOW

2 clients

MEDIUM

3

HIGH

>=4

---

# Case 14

## AML_SHARED_GEO_CLUSTER

### Objective

Detect unusual concentration of customers from the same location.

---

Future Inputs

```
Latitude

Longitude

ASN

ISP
```

---

Graph

```
Geo

↓

Login

↓

Client

↓

Contract
```

---

# Behavioral Signals

Behavioral cases never close AML cases.

Instead they contribute signals.

```
Financial Case

+

Behavior Case

↓

Composite Risk
```

---

# Composite Risk Examples

```
Cash In Out

+

Impossible Travel

↓

HIGH
```

```
Repeated Round Trip

+

New Device

↓

HIGH
```

```
Shared External Account

+

Shared Device

↓

CRITICAL
```

---

# Explanation Packet

Behavioral section

```
Behavior Summary

Observed Changes

Historical Baseline

Risk Signals

Missing Data
```

---

# Design Rules

Rule 1

Behavior never replaces money flow.

---

Rule 2

Behavior increases confidence.

---

Rule 3

Behavior always compares against historical baseline.

---

Rule 4

Behavior is temporal.

---

Rule 5

Behavior is customer-specific.

---

# ADR

ADR-012

Behavioral AML

Decision

Behavioral signals complement financial investigations.

Reason

Behavioral anomalies improve prioritization while reducing false positives.

---

# End of Part 4

Next

```
05_AML_CASES_PART_5.md
```

Topics

- Beneficial Owner
- Corporate Structures
- Watchlists
- PEP
- Adverse Media
- Relationship AML
- Community Detection
- Composite Risk Engine
