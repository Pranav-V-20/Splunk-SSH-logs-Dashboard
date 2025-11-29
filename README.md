# 🛡️ Splunk SSH Authentication Monitoring Dashboard

A step-by-step project to build an interactive Splunk dashboard for monitoring SSH authentication activity, detecting failed login spikes, and visualizing brute-force attacks with geo-location.
---
## 📌 Objective

This project demonstrates how to create a Splunk dashboard that provides insights into SSH authentication logs.
You will build multiple panels to track:
* Total SSH events
* Successful vs failed logins
* Invalid user attempts
* Failed logins by username
* Possible brute-force IPs
* Geo-location-based brute force visualization
---

## 🧪 Lab Setup

Before building the dashboard:

In Splunk Dashboard Studio, click Add Input.

1. Choose Time → click the pencil icon.
2. Set:
  * Label: Time Range
  * Token: time_range

3. Click Add Input again → choose Submit.

Note: For all panels created later, always set the time to Shared Time Picker: time_range to maintain consistency.
---
## 📝 Tasks
### ✅ Task 0: Setting Up Time Range

This step ensures all dashboard panels use a unified time filter.

Steps:

* Add Time Input
* Label → Time Range
* Token → time_range
* Add Submit Input

### ✅ Task 1: Authentication Overview Panels

#### Goal: Provide a quick summary of SSH activity.

1. Total SSH Events

* Add Panel → New → Single Value
* Set Title: Total SSH Events
* Search Query:
```
source="ssh_logs.json" host="LinuxServer" sourcetype="_json"
| stats count AS "Total SSH Events"
```
2. Successful Logins

* Add Panel → Single Value
* Title: Successful Logins
* Search Query:
```
source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Successful SSH Login"
| stats count AS "Successful Logins"
```
3. Failed Logins

* Add Panel → Single Value
* Title: Failed Logins
* Search Query:
```
source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Failed SSH Login"
| stats count AS "Failed Login"
```
4. Connection Without Authentication (Invalid Users)

* Add Panel → Single Value
* Title: Invalid User Attempts
* Search Query:
```
source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Connection Without Authentication"
| stats count AS "Connection Without Authentication"

```
---

### ✅ Task 2: Login Activity Trends

#### Goal: Visualize login behavior and detect potential brute-force attempts.

1. Failed Logins by Username

* Add Panel → Bar Chart
* Title: Failed Logins by Username
* Search Query:
```
source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Failed SSH Login"
| top username
```
2. Possible Brute Force (Multiple Failed Attempts)

* Add Panel → Statistics Table
* Title: Possible Brute Force by IP Address
* Search Query:
```
source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Multiple Failed Authentication Attempts"
| top id.orig_h
```
---
### ✅ Task 3: Visualizing Brute Force Attack in Geo-Location

#### Goal: Display the source of brute-force attempts on a world map.

Brute Force Attack Geo-Location Map

* Add Panel → Choropleth Map
* Title: Brute Force Attack with Geo-Location
* Search Query:
```
source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Multiple Failed Authentication Attempts"
| table id.orig_h
| iplocation id.orig_h
| stats count by Country
| geom geo_countries featureIdField="Country"
```
---
## 📊 Final Output

By completing all tasks, you will have a fully functional Splunk dashboard that:

<img width="1920" height="1133" alt="Final" src="https://github.com/user-attachments/assets/4127e156-8f19-4fc0-a651-a97077cefc7e" />

✔ Tracks SSH login activity
✔ Shows authentication trends
✔ Identifies brute-force sources
✔ Maps attacker IP origins globall
