[README (2).md](https://github.com/user-attachments/files/35128919/README.2.md)
# Smart Home IoT: Sensor Data Analytics for Energy Management

Datasets for the FT3 case study in **Data Science for IoT (21CSE429T)**.

**Author:** Aditya Chebrolu (RA2311056010116)

## About

This case study looks at how IoT sensors in a smart home (temperature, humidity, light, motion and smart plugs) can be used to monitor electricity use and turn raw readings into energy-saving decisions.

> **Note:** The data is **simulated**, not recorded from a real household. It is modelled on the structure of the UCI [Appliances Energy Prediction](https://archive.ics.uci.edu/) dataset, with realistic faults (missing values, duplicates, sensor spikes) added on purpose to demonstrate the data-cleaning workflow.

## Files

| File | Description |
|---|---|
| `smart_home_energy_raw.csv` | Raw sensor readings: 4,345 rows, 10 columns, including faults |
| `smart_home_energy_cleaned.csv` | Cleaned data: 4,320 unique rows, no missing values |

## Dataset description

- **Period:** 1–30 October 2026 (30 days)
- **Interval:** one reading every 10 minutes

| Column | Description | Unit |
|---|---|---|
| `timestamp` | Date and time of reading | - |
| `indoor_temp_c` | Living-room temperature | °C |
| `indoor_humidity_pct` | Living-room relative humidity | % |
| `outdoor_temp_c` | Outdoor temperature | °C |
| `light_lux` | Ambient light level | lux |
| `occupancy` | Presence from PIR sensor (1 = occupied) | 0 / 1 |
| `hvac_power_w` | Air-conditioner power draw | W |
| `appliance_power_w` | Other appliances power draw | W |
| `lighting_power_w` | Lighting power draw | W |
| `energy_wh` | Total energy used in the 10-minute interval | Wh |

## Data cleaning applied

| Issue | Found in raw data | Treatment |
|---|---|---|
| Missing values | 340 blank cells | Time-based linear interpolation |
| Duplicate rows | 25 | Removed |
| Sensor-fault outliers | 15 readings (impossible temperatures and humidity, power spikes above 9,000 W) | Set to missing, then interpolated |

Genuine high-usage events (cooking, AC, heavy appliances) were kept, since they are real behaviour and not faults.

## Key findings

- Energy use peaks at **18:00**, when the family is home and AC, cooking and entertainment overlap.
- Weekend consumption is about **63% higher** than weekday consumption.
- Load split: appliances about 50%, HVAC about 45%, lighting about 5%.
- Energy per interval is much higher when the home is occupied (about 145 Wh) than when empty (about 18 Wh).
- Abnormal night-time usage appears on **7, 18 and 26 October**, consistent with an appliance left on overnight.

## Quick start

```python
import pandas as pd

df = pd.read_csv("smart_home_energy_cleaned.csv", parse_dates=["timestamp"])
print(df.describe())

# Average energy by hour of day
print(df.groupby(df["timestamp"].dt.hour)["energy_wh"].mean())
```

## Related deliverables

The accompanying case study report (Word) and presentation (PowerPoint) cover the full analysis: preprocessing, descriptive statistics, visualisations, correlation analysis and anomaly detection.
