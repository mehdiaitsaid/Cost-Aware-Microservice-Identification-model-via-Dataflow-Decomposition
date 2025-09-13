# Communication-Cost Aware Microservice Decomposition – Cargo Tracking System  

This repository contains the dataset and resources used to validate our **communication-cost–aware dataflow-driven approach to microservice identification**.  
The dataset is based on the **Cargo Tracking System** case study widely used in microservices research.  

---

## 📂 Repository Contents  
- `processes_datastores.yaml` – YAML mapping of all processes, subprocesses, and datastores.  
- `sentences.yaml` – YAML mapping of all dataflows (sentences) between processes and datastores.  
- `results/` – Results of applying the proposed model and comparison with DDD-based decomposition.  
- `scripts/` – Python scripts for simulation and visualization of the decomposition.  

---

## 📝 Dataset Description  

### Processes  
Each process represents a system function, decomposed into sub-processes at **DFD Level-1**.  

```yaml
processes:
  P1: "View Tracking"
  P11: "Fetch Cargo by TrackingId"
  P12: "Fetch Delivery status"
  P13: "Fetch HandlingEvent records"
  P14: "Fetch RouteSpecification and Itinerary"
  P15: "Fetch Voyage details"

  P2: "View Cargos"
  P21: "Fetch Cargo list"
  P22: "Fetch RouteSpecification"
  P23: "Fetch Delivery info"
  P24: "Fetch Itinerary"
  P25: "Fetch Voyage planning"

  P3: "Book Cargo"
  P31: "Generate Cargo TrackingId"
  P32: "Persist Cargo entity"
  P33: "Link Cargo to Location"

  P4: "Change Cargo Destination"
  P41: "Update RouteSpecification"
  P42: "Update Delivery status"
  P43: "Update Destination info"

  P5: "Route Cargo"
  P51: "Assign Itinerary"
  P52: "Add Legs and CarrierMovements"
  P53: "Persist Route and Itinerary"

  P6: "Create Location"
  P61: "Add Location to datastore"

  P7: "Create Voyage"
  P71: "Add Voyage to datastore"

  P8: "Add CarrierMovement"
  P81: "Add CarrierMovement to Voyage"
  P82: "Persist CarrierMovement details"

  P9: "Handle Cargo Event"
  P91: "Register Cargo handling event"
  P92: "Update Delivery after event"
  P93: "Update HandlingEvent datastore"
  P94: "Update Delivery status (misdirected/unloaded)"
```
### Datastores
Datastores represent persistent repositories used by processes.

```yaml
datastores:
  S1: "Cargo – Stores unique identifiers of cargos (TrackingId)"
  S2: "RouteSpecification – Stores itinerary info (Origin, Destination, Deadline)"
  S3: "Itinerary – Stores cargo route identifiers"
  S4: "Leg – Stores each transport leg (load/unload location & time)"
  S5: "Location – Stores location details (unLocode, name)"
  S6: "HandlingEvent – Stores event history (upload, completion, registration time)"
  S7: "Delivery – Stores delivery status (transport status, isUnloadedAtDestination, ETA, routingStatus)"
  S8: "Voyage – Stores voyage identifiers (voyageNumber)"
  S9: "CarrierMovement – Stores voyage movements (departure/arrival location & time)"

```

### Sentences (Dataflows)
Sentences describe dataflow interactions between processes and datastores.

| Start | → | End | Meaning                                                  |
| ----- | - | --- | -------------------------------------------------------- |
| S1    | → | P11 | Cargo datastore provides cargo details by TrackingId     |
| S6    | → | P12 | HandlingEvent datastore provides delivery-related events |
| S8    | → | P13 | Voyage datastore provides voyage details                 |
| S2    | → | P14 | RouteSpecification datastore provides routing info       |
| S7    | → | P14 | Delivery datastore provides delivery status              |
| S1    | → | P21 | Cargo datastore provides cargo list                      |
| S2    | → | P22 | RouteSpecification provides route info for cargos        |
| S2    | → | P23 | RouteSpecification provides delivery route status        |
| S3    | → | P24 | Itinerary provides planned routes                        |
| S7    | → | P25 | Delivery datastore provides voyage planning info         |
| S5    | → | P31 | Location datastore provides location data for booking    |
| P32   | → | S1  | Persist cargo entity into Cargo datastore                |
| P33   | → | S2  | Persist route specification into RouteSpecification      |
| P42   | → | S2  | Update RouteSpecification after destination change       |
| P43   | → | S7  | Update Delivery status after destination change          |
| S5    | → | P51 | Location datastore provides location data for routing    |
| S8    | → | P52 | Voyage datastore provides voyage data for routing        |
| S9    | → | P52 | CarrierMovement provides movement info for routing       |
| P53   | → | S3  | Persist itinerary into Itinerary datastore               |
| P53   | → | S4  | Persist legs into Leg datastore                          |
| P61   | → | S5  | Add new Location to datastore                            |
| P71   | → | S8  | Add new Voyage to datastore                              |
| S8    | → | P81 | Voyage datastore provides voyage details for movement    |
| P82   | → | S9  | Add CarrierMovement details into CarrierMovement         |
| P92   | → | S6  | Update HandlingEvent datastore with cargo event          |
| S8    | → | P92 | Voyage datastore provides voyage context for event       |
| S6    | → | P93 | HandlingEvent datastore provides event history           |
| S7    | → | P93 | Delivery datastore provides delivery status for update   |
| P94   | → | S7  | Update Delivery datastore after cargo event              |


## 📊 Usage  

The dataset can be used to:  
- Reproduce the decomposition experiment.  
- Compare our approach with other microservice identification methods.  
- Simulate and visualize process–datastore graphs.  

---

## 🔗 Reference  

This dataset is derived from the **Cargo Tracking System** case study used in prior microservices identification research.  
A complete reference will be made available soon.  


