# Fighting COVID-19: Learning from South Korea

End-to-end analytical case study on **how South Korea managed COVID-19** and what **policy lessons** can be transferred to other countries (focus: Lithuania).

The project combines **epidemiological data, infection case data, time-series testing data and government policy logs** to answer practical questions for public-health decision makers.

---

## 1. Project Overview

**Goal:**  
Use real COVID-19 data from South Korea (Jan–Jun 2020) to extract **actionable insights** and design **data-driven recommendations** for managing the next pandemic wave in Lithuania.

**Main questions**

1. **Who is the “typical” COVID-19 patient?**  
   – Age, sex, infection source, outcomes.

2. **How and where does the virus spread?**  
   – Local vs nationwide outbreaks, super-spreader events, spill-over between cities.

3. **How effective were government policies over time?**  
   – Relation between alert levels, social-distancing measures, testing strategy and case dynamics.

4. **What concrete measures should another country adopt?**  
   – Capacity planning, testing strategy, mobility and gathering rules.

---

## 2. Data & Sources

The analysis is based on the **South Korea COVID-19 dataset** (Kaggle – `kimjihoo/coronavirusdataset`):

- **PatientInfo.csv** – line-list of COVID-19 patients  
- **Case.csv** – local infection clusters and group outbreaks  
- **Time.csv** – national time series (tests, confirmed, released, deceased)  
- **TimeProvince.csv** – provincial time series  
- **Region.csv** – regional demographics and coordinates  
- **Policy.csv** – national COVID-19 policy log

Time period: **2020-01-20 to 2020-06-30**.

---

## 3. Tech Stack

- **Python**: `pandas`, `numpy`
- **Visualization**: `matplotlib`, `seaborn`, `plotly`
- **Data handling & helpers**: custom `covid_analysis_helpers` module  
  – `data_loader`, `custom_plots`, `custom_maps`

---

## 4. Analysis Structure

### 4.1. Patient-Level Epidemiology

**Objectives**

- Build a profile of the “average” COVID-19 patient.  
- Quantify outcomes (isolation, recovery, death) by age and sex.  
- Understand infection sources and contact patterns.

**Key steps**

- Data cleaning (duplicate patient IDs, invalid cities, missing infection cases).  
- Feature engineering:
  - `age_category` (young: 0–29, middle: 30–59, old: 60+)  
  - `symptom_to_confirmed` (days from symptom onset to confirmation)  
  - `confirmed_to_released` (days from confirmation to recovery)  
  - `confirmed_to_deceased` (days from confirmation to death)

**Main insights**

- **Demographics**
  - 5,164 unique patients.
  - Higher share of **female** patients; largest group in their **20s**.
- **Sources of infection**
  - ~⅓ of cases: **unknown source**.  
  - ~⅓: **direct contact with a known patient**.  
  - Remaining: travel, workplaces, churches, clubs, sports facilities, hospitals, care homes.
- **Outcomes**
  - Overall mortality: **1.5%**.  
  - No deaths under 30. Mortality **jumps after age 70** (≥9% in 70s, ≥14% in 80+).  
  - More **men die** than women, despite more women being diagnosed.  
  - Median hospital stay:
    - Young / middle age: **22 days**  
    - Older patients: **26 days**
- **Operational implications**
  - Long isolation times → **bed and isolation capacity** must be planned for weeks, not days.  
  - Age-stratified mortality → **targeted protection** for 70+ is critical.

---

### 4.2. Infection Cases & Outbreak Mapping

**Objectives**

- Identify **super-spreader clusters** and spill-over patterns.  
- Compare local group outbreaks vs imported cases.

**Key steps**

- Merge `Case.csv` with `Region.csv` to fill missing coordinates.  
- Build interactive **Mapbox maps** for:
  - Local group infections  
  - Overseas inflow cases  
  - Patient-contact clusters  
  - Major outbreaks spilling into multiple cities

**Main insights**

- Largest local clusters around **Daegu** and **Seoul**.  
- **Shincheonji Daegu Church** and **Itaewon clubs** stand out as **super-spreader events** with spill-overs to many regions.  
- Overseas inflow cases are concentrated in **Seoul and Gyeonggi-do**, the main international gateways.

**Policy implications**

- Tight control on **mass gatherings** (churches, clubs, sports halls).  
- Strict & early **border testing/quarantine** in hubs like capital regions.  
- Ability to **trace and cut infection chains** from a small number of super-spreader events.

---

### 4.3. Time-Series & Government Policy Alignment

**Objectives**

- Link **test volumes, confirmed cases and deaths** to **government actions** over time.  
- Evaluate the effectiveness and timing of specific policies.

**Data**

- `Time.csv` and `TimeProvince.csv` for daily/accumulated figures.  
- `Policy.csv` for official measures such as:
  - Infectious Disease Alert Levels  
  - Drive-through testing centers  
  - Strong / weak social distancing campaigns  
  - School closures  
  - Club/bar closures

**Key findings**

- **Peak & control**
  - New cases **spike at end of February** following major outbreaks.  
  - Government reacts with **Alert Level 4 (Red)**, **strong social distancing**, **school closures**.  
  - New cases drop to **single digits**, after which distancing is relaxed.
- **Testing strategy**
  - Opening **drive-through testing centers** triggers a strong jump in test volume.  
  - Test numbers show weekly cycles (less reporting Sundays/Mondays).  
  - From April onwards, **positive test ratio is kept below 1%** → indicates wide testing and early detection.
- **Outcomes**
  - Releases lag confirmations by ~2 weeks, consistent with disease course.  
  - Deaths start early and persist; only fall to low single digits by mid-April.

**Operational implications**

- **Early, strong and time-bound** measures (especially after super-spreader events) are effective.  
- **Testing availability + convenience** (drive-through, multiple sites) is key to control.  
- Monitoring **positive test ratio** is a robust KPI for pandemic control.

---

### 4.4. Application to Lithuania

Using demographic similarity (age/sex structure) between South Korea and Lithuania, the project translates insights into concrete recommendations:

- **Hospital & isolation capacity**
  - Plan for **median 22–26 days** of isolation per patient.  
  - Expect resource pressure mainly from **older age groups**.
- **Target groups**
  - Focus protective measures on **70+ age group**.  
  - Engage **young, socially active adults** to reduce spread.
- **Mobility & gatherings**
  - Maintain strict rules for **mass gatherings, worship places, nightlife venues**.  
  - Use **travel restrictions and testing** to prevent inter-regional and international spread.
- **Communication**
  - Emphasize not only death risk but also **spread risk** and **responsibility**.  
  - Promote **early testing** and healthy lifestyle factors.

---

## 5. How to Run the Notebook

1. **Clone the repo**

   ```bash
   git clone https://github.com/your-username/Healthcare-Covid-Operations-Analytics.git
   cd Healthcare-Covid-Operations-Analytics
