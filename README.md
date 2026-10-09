# Dataset Field Guide: Amazon Last-Mile Delivery (`DLA7` Station)

## Project Overview

* **Project Title:** Data-Driven Last-Mile Fleet Allocation: Learning Operational Item-to-Vehicle Assignment Rules[cite: 1]
* **Problem Statement:** Standard Vehicle Routing Problem (VRP) algorithms optimize purely for theoretical mathematical metrics like shortest physical distance or lowest travel time[cite: 1]. However, real-world human dispatchers assign packages based on unwritten operational preferences—such as driver neighborhood familiarity, road network complexities, and vehicle volume constraints[cite: 1].
* **Objective:** Solve the **Vehicle Allocation Problem (VAP)** by predicting which package belongs to which specific delivery vehicle on a given day[cite: 1, 2]. By treating this as a Machine Learning or Agentic AI classification/clustering problem, models can capture real-world dispatcher preferences without needing to solve complex Traveling Salesperson Problem (TSP) stop sequences[cite: 1, 2].

---

## Column Descriptions

### 1. Package Identification & Physical Dimensions

* `package_id`: A unique string identifier assigned to each individual parcel[cite: 1]. Serves as the primary key for item-level prediction[cite: 1].
* `planned_service_time_sec`: The estimated driver drop-off/dwell time in seconds at the delivery address[cite: 1]. Represents the physical service workload required at the door (e.g., parking, scanning, walking to the entrance)[cite: 1, 2].
* `depth_cm`: The physical depth measurement of the parcel in centimeters ($\text{cm}$)[cite: 1].
* `height_cm`: The physical height measurement of the parcel in centimeters ($\text{cm}$)[cite: 1].
* `width_cm`: The physical width measurement of the parcel in centimeters ($\text{cm}$)[cite: 1].
* `volume_cm3`: The total physical space consumed by the individual package in cubic centimeters ($\text{cm}^3$), computed as $\text{depth} \times \text{height} \times \text{width}$[cite: 1]. Essential for evaluating vehicle capacity boundaries[cite: 1, 2].

### 2. Delivery Location & Spatial Attributes

* `stop_id`: An anonymized code representing a specific physical delivery address[cite: 1]. Multiple packages bound for the same house or apartment building share the same `stop_id`[cite: 1].
* `lat`: The latitude coordinate of the delivery location in decimal degrees[cite: 1].
* `lng`: The longitude coordinate of the delivery location in decimal degrees[cite: 1].
* `city`: The city or municipality name where the delivery drop-off is located[cite: 1].
* `zone_id`: An anonymized micro-neighborhood/zone cluster identifier (e.g., `P-12.3C`)[cite: 1]. Captures geographic groupings used by dispatchers to assign adjacent neighborhoods to the same vehicle[cite: 1].
* `type`: Operational point type (e.g., `Dropoff` for customer locations or `Station` for the central depot)[cite: 1].
* `avg_travel_time_sec`: The average travel time in seconds from this stop to all other stops on the same route under typical traffic conditions[cite: 1]. Provides a measure of relative spatial isolation or proximity[cite: 1].

### 3. Depot & Operational Context

* `station_code`: Unique identifier for the distribution depot handling the dispatch[cite: 1]. For this dataset, all rows are filtered to station **`DLA7`**[cite: 1].
* `date`: The calendar date (`YYYY-MM-DD`) of the dispatch[cite: 1]. Represents both the departure date from the depot and the scheduled arrival date for the customer[cite: 1].
* `departure_time`: Timestamp (in UTC) showing when the vehicle departed the depot[cite: 1].
* `executor_capacity_cm3`: The total available interior volume capacity of the assigned vehicle in cubic centimeters ($\text{cm}^3$)[cite: 1]. Represents the hard payload constraint for van loading[cite: 1, 2].
* `route_score`: Historical qualitative efficiency score assigned to the route execution[cite: 1].
* `num_stops`: Total count of unique stop locations assigned to the vehicle executing the route[cite: 1].

### 4. Fleet Allocation & Machine Learning Targets

* `available_vehicles_count`: The total number of active delivery vehicles dispatched from depot `DLA7` on that specific date[cite: 2]. Sets the total candidate clusters/classes ($K$) available for assignment[cite: 1, 2].
* `assigned_vehicle_id`: **[MACHINE LEARNING TARGET Variable ($y$)]** A zero-indexed integer ($0, 1, 2, \dots, K-1$) indicating which specific vehicle was assigned to deliver this package on that date[cite: 1]. This is the ground-truth target that models aim to predict[cite: 1].

---

## Getting Started

You can load the dataset directly into Pandas using Python:

```python
import pandas as pd

# Load dataset directly from raw URL
url = "[https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO_NAME/main/dla7_packages_dataset.csv](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO_NAME/main/dla7_packages_dataset.csv)"
df = pd.read_csv(url)

df.head()
