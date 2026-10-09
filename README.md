# Dataset Field Guide: Amazon Last-Mile Delivery (DLA7 Station)

## Project Overview

* **Project Title:** Data-Driven Last-Mile Fleet Allocation: Learning Operational Item-to-Vehicle Assignment Rules.
* **Problem Statement:** Standard Vehicle Routing Problem (VRP) algorithms optimize purely for theoretical mathematical metrics like shortest physical distance or lowest travel time. However, real-world human dispatchers assign packages based on unwritten operational preferences—such as driver neighborhood familiarity, road network complexities, and vehicle volume constraints.
* **Objective:** Solve the Vehicle Allocation Problem (VAP) by predicting which package belongs to which specific delivery vehicle on a given day. By treating this as a Machine learning or Agentic AI problem, models can capture real-world dispatcher preferences without needing to solve complex Traveling Salesperson Problem (TSP) stop sequences.

---

## Column Descriptions

### 1. Package Identification & Physical Dimensions

* `package_id`: A unique string identifier assigned to each individual parcel. Serves as the primary key for item-level prediction.
* `planned_service_time_sec`: The estimated driver drop-off/dwell time in seconds at the delivery address. Represents the physical service workload required at the door (e.g., parking, scanning, walking to the entrance).
* `depth_cm`: The physical depth measurement of the parcel in centimeters (cm).
* `height_cm`: The physical height measurement of the parcel in centimeters (cm).
* `width_cm`: The physical width measurement of the parcel in centimeters (cm).
* `volume_cm3`: The total physical space consumed by the individual package in cubic centimeters (cm^3), computed as depth times height times width. Essential for evaluating vehicle capacity boundaries.

### 2. Delivery Location & Spatial Attributes

* `stop_id`: An anonymized code representing a specific physical delivery address. Multiple packages bound for the same house or apartment building share the same stop_id.
* `lat`: The latitude coordinate of the delivery location in decimal degrees.
* `lng`: The longitude coordinate of the delivery location in decimal degrees.
* `city`: The city or municipality name where the delivery drop-off is located.
* `zone_id`: An anonymized micro-neighborhood/zone cluster identifier (e.g., P-12.3C). Captures geographic groupings used by dispatchers to assign adjacent neighborhoods to the same vehicle.
* `type`: Operational point type (e.g., Dropoff for customer locations or Station for the central depot).
* `avg_travel_time_sec`: The average travel time in seconds from this stop to all other stops on the same route under typical traffic conditions. Provides a measure of relative spatial isolation or proximity.

### 3. Depot & Operational Context

* `station_code`: Unique identifier for the distribution depot handling the dispatch. For this dataset, all rows are filtered to station DLA7.
* `date`: The calendar date (YYYY-MM-DD) of the dispatch. Represents both the departure date from the depot and the scheduled arrival date for the customer.
* `departure_time`: Timestamp (in UTC) showing when the vehicle departed the depot.
* `executor_capacity_cm3`: The total available interior volume capacity of the assigned vehicle in cubic centimeters (cm^3). Represents the hard payload constraint for van loading.
* `route_score`: Historical qualitative efficiency score assigned to the route execution.
* `num_stops`: Total count of unique stop locations assigned to the vehicle executing the route.

### 4. Fleet Allocation & Machine Learning Targets

* `available_vehicles_count`: The total number of active delivery vehicles dispatched from depot DLA7 on that specific date
* `assigned_vehicle_id`: [MACHINE LEARNING TARGET Variable (y)] A zero-indexed integer (0, 1, 2, …, ) indicating which specific vehicle was assigned to deliver this package on that date. This is the ground-truth target that models aim to predict.

---

## Getting Started

You can load the dataset directly into Python using Pandas:

```python
import pandas as pd

# Load dataset directly from raw URL
url = "[https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO_NAME/main/dla7_packages_dataset.csv](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO_NAME/main/dla7_packages_dataset.csv)"
df = pd.read_csv(url)

df.head()
