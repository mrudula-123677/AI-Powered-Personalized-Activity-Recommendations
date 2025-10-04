## Capstone Project: AI-Powered Personalized Activity Recommendations

**Project Goal:** Developing a highly personalized recommendation system for Strava users that goes beyond simple static routes and training plans.

| Aspect | Details |
| --- | --- |
| **Status** | Data Collection is complete and the master dataset is compiled. |
| **Current Focus** | Moving from data wrangling to serious **Exploratory Data Analysis (EDA)**. |
| **Tech Stack** | Python (Pandas, GeoPandas, Matplotlib/Plotly), Strava API, Custom Data Generator. |
| **Next Big Step** | Validate the distribution of the 50 user profiles and identify key features for modeling. |

---

## Phase 1: Data Collection & Cleaning (Done!)

The last few weeks were essentially a two-part mission: securing the initial real data and then generating the necessary volume for the Collaborative Filtering model. The master dataset is now compiled, containing **2,500 activities** across **50 users** and **100 unique routes**, covering exactly **one year** (from 2024-10-04 to 2025-10-04).

### 1. Connecting to the Strava API (Real Data & Initial Structure)

We managed to get the **OAuth flow** stable and pulled the required activity history for my primary profile (let's assume my ID is one of the 50, e.g., hermandavid).

- **Key Achievement:** My real activities established the foundational structure and defined the 100 key route_id's that the synthetic users interact with. I made sure all key fields, including the all-important **rating** and performance metrics (`average\_pace\_min\_per\_km`, `distance\_km\_user`), were correctly mapped and cleaned.
- **Initial Cleanup:** Converted all units to kilometers, minutes/km, and handled initial data type conversions. The `distance\_km\_user` and `distance\_km\_route` fields now give me a built-in metric to analyze route consistency.

### 2. Generating Synthetic User Profiles (Scaling the Data)

This was the biggest technical lift in this phase. The model needs a robust **User-Route Interaction Matrix**, and my single profile wasn't enough.

- **Methodology:** We successfully generated **49 additional synthetic profiles** (like brenda53, brittany42, etc.) based on patterns observed in my own activity profile. This was not simple random noise; each of the 50 users in the dataset has a distinct "personality."
- **Validation Point:** The total count of 2,500 activities and 50 users is now locked in. This ensures I have the scale required for the **Matrix Factorization** component of the Collaborative Filtering engine.
- **Route Unification:** A critical outcome is that all 2,500 activities map to a set of 100 canonical route_id's. This route-level aggregation is the key feature that will allow the recommender to function.

### 3. Data Integration Summary

The compiled CSV (`synthetic_strava_data.csv`) is now the single source of truth, containing everything I need:

- **User/Activity Data:** The actual pace, distance, and rating given by a user for an activity.
- **Route Data:** Static attributes like `distance\_km\_route`, elevation_meters_route, surface_type_route, and a pre-calculated `difficulty\_score`. This last feature is crucial for the **Content-Based Filtering** part of the project.

---

## Next Steps: Phase 2 - Exploratory Data Analysis (EDA)

The data is clean and structured. The next few days are dedicated to validating the synthetic profiles and extracting high-level insights before feature engineering begins.

### The Big Questions I Need to Answer:

1. **Synthetic Profile Validation:** We need to confirm that the 50 users genuinely exhibit different behaviors. I'll perform comparative EDA on distributions (e.g., distance, pace) to make sure the "Beginner Runner" doesn't look exactly like the "Marathoner."
2. **Route Popularity & Ratings:** We need to analyze the 100 routes. Which routes are the most popular (highest interaction count)? Which routes receive the highest average rating? This helps identify the baseline preferences across the simulated community.
3. **My Personal Fingerprint (Hermandavid):** We need to isolate my real profile's data and plot my specific pace, time-of-day, and distance habits. This defines the **content** against which the Content-Based Recommender will be built.
    1. **Correlation Check:** We need to see if the difficulty_score correlates well with lower  or slower average paces. This relationship is a critical assumption in my model design.
