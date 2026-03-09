# ARENA-S-M-LAT-ON
## Metro Line Simulation Model (Arena)

### Project Overview

In this project, the modeling concepts of the study are adapted and implemented using Arena simulation software. The metro network is modeled using a simplified representation of the Istanbul metro system, where passenger arrivals, train dwell times, and headway intervals are analyzed to evaluate system performance.

The model captures how sudden changes in passenger demand at one station can affect subsequent stations, allowing the system to be analyzed under different operational scenarios.

---

### Model Structure

A tracking algorithm was developed to match train entities with station resources. The algorithm continuously compares the train position with station numbers, and when the values match, the train is associated with that station. This mechanism produces a transition signal, triggering passenger boarding and alighting events when a train arrives.

The same station logic is reused for all stations in the metro line.

---

### Simulation Flow

The model starts with a **Create block**, which generates trains every **120 seconds**, producing a total of **10 trains**.

Each train receives a unique identifier through an **Assign block**, allowing the model to distinguish between trains within the system.

Stations are modeled using the following Arena blocks:

- **Queue**: represents trains waiting for station availability  
- **Seize**: occupies the station resource when a train arrives  
- **Assign**: identifies the station number of the arriving train  

A **While loop** controls the dwell time at the station. The loop continues while the waiting time is greater than zero. A **Delay block (1 second)** is used to update the system at each second to maintain simulation precision.

---

### Passenger Boarding Logic

A **Branch block** determines how many passengers can board the train:

- If **train capacity > waiting passengers**, all passengers board the train and the station passenger count becomes zero.
- If **waiting passengers > train capacity**, the train reaches full capacity and remaining passengers continue waiting at the station.

Passenger boarding reduces the remaining train capacity accordingly.

After the dwell time ends, the train attempts to move to the next station. If the next station resource is busy, the train waits until it becomes available. This condition increases the realism of the model.

---

### Passenger Alighting

At the second station and subsequent stations, both boarding and alighting processes occur. Passenger alighting is modeled using an **exponential distribution (expo(3,1))**, where passengers leave the train every second and free capacity becomes available.

At the final station, all remaining passengers leave the train, and train capacity is reset before the entity exits the system through a **Dispose block**.

Passenger arrivals are generated separately using **Create–Assign–Count blocks**, where passengers are counted but not treated as full entities within the simulation.

---

### Distribution Analysis

Several probability distributions were tested to model passenger arrivals. The comparison was based on **mean square error between the model curve and the real data histogram**, along with **chi-square goodness-of-fit tests**.

#### Beta Distribution
- Mean square error: **0.00049**
- p-value: **0.0466**

Although the p-value is slightly below 0.05, the error value is low, suggesting a reasonable fit. However, the interpretation should be cautious due to the large dataset.

#### Exponential Distribution
- Mean square error: **0.00653**

This distribution performs significantly worse than the beta distribution. The chi-square test produces a very small p-value, indicating that the exponential distribution does not fit the dataset.

#### Gamma Distribution
- Mean square error: **0.0041**

Better than the exponential distribution but still weaker than the beta distribution. The p-value is below 0.005, indicating insufficient statistical fit.

#### Normal Distribution
- Mean square error: **0.003615**

Although the error value is relatively small, the chi-square test indicates that the distribution does not adequately represent the dataset.

#### Triangular Distribution
- Mean square error: **0.008653**

This distribution shows the worst performance among the tested options and is clearly inconsistent with the data.

#### Uniform Distribution
- Mean square error: **0.000408**

This distribution provides the **best fit** among all tested distributions. The p-value is greater than 0.05, indicating that the distribution cannot be rejected statistically.

Therefore, passenger arrivals are modeled using:

This distribution provides a realistic representation of passenger demand within the system.

---

### Scenario Analysis

Different scenarios were tested to evaluate system performance under varying **train headway intervals**.

#### Train Occupancy

In the base scenario, the occupancy rate at **Station 3** reaches approximately **95%**, indicating high demand. In other scenarios, the occupancy rises to **100%**, suggesting that the system operates close to its capacity limit.

At **Station 2**, the occupancy increases significantly in alternative scenarios, reaching **62.8% and 81.7%**, highlighting increasing congestion at this station.

Station 1 shows a more gradual increase, suggesting that the initial system loading remains within acceptable levels.

In the third scenario, occupancy increases at all stations, indicating a potential **capacity bottleneck**.

---

#### Passenger Waiting Time

Train headways were tested at **120, 300, and 600 seconds**.

The results show that average passenger waiting times remain relatively stable:

- **Station 1:** ~100 seconds in all scenarios  
- **Other stations:** ~20 seconds on average  

The only noticeable change occurs at **Station 2**, where waiting time rises to **32 seconds** under the **120-second headway scenario**.

Overall, the system demonstrates stable performance, and increased headway intervals do not significantly degrade passenger waiting times.

---

### Conclusion

The simulation results show that the metro system operates close to its capacity limit under higher demand scenarios. Increased passenger volumes significantly raise train occupancy levels, particularly at intermediate stations.

The findings suggest that future improvements may require:

- increasing **train frequency**, or  
- expanding **train capacity**

to prevent congestion and maintain service efficiency.

<img width="338" height="395" alt="image" src="https://github.com/user-attachments/assets/7f6724b9-6ff5-450c-9bd8-9d333be3aa7f" />
<img width="589" height="160" alt="image" src="https://github.com/user-attachments/assets/6a9641b4-68ca-4fbc-9a77-a1b84d47f82b" />
<img width="229" height="313" alt="image" src="https://github.com/user-attachments/assets/f08168c8-be87-40e4-a0d9-8c804fadcf12" />
<img width="658" height="180" alt="image" src="https://github.com/user-attachments/assets/4cc05928-5ce0-4c1a-a53d-6f7eba525cc7" />
<img width="581" height="166" alt="image" src="https://github.com/user-attachments/assets/4f26c9a0-377a-4d56-81b5-38c736e57560" />
<img width="228" height="281" alt="image" src="https://github.com/user-attachments/assets/b811ada6-76fc-4478-b8ea-56fa16b40c9d" />
<img width="629" height="167" alt="image" src="https://github.com/user-attachments/assets/65cc6a43-ea64-46fe-be97-74084b9d4854" />
<img width="243" height="309" alt="image" src="https://github.com/user-attachments/assets/fd51038a-c1b9-4974-b8e0-d49c3a7fe9bf" />
<img width="634" height="166" alt="image" src="https://github.com/user-attachments/assets/bbc916a8-2e07-4756-b822-66d07af92722" />
<img width="205" height="264" alt="image" src="https://github.com/user-attachments/assets/3d1ed0ef-be58-43c9-aeec-6131a3e855d7" />
<img width="695" height="169" alt="image" src="https://github.com/user-attachments/assets/34bf513e-b943-4c4d-96d3-6e1442350017" />
<img width="281" height="337" alt="image" src="https://github.com/user-attachments/assets/8b04f157-85a9-4416-923e-a9ed75b11b21" />
<img width="245" height="283" alt="image" src="https://github.com/user-attachments/assets/be880ce0-3464-4ef5-a91c-532845ac6eda" />












