# Personal Project Writeup: Grid Intelligence Platform

##  Wuhao Xia

Grid Intelligence Platform began as a policy and data question: how can someone understand the behavior of the U.S. electricity grid without paying for expensive proprietary market intelligence tools? The proposal was to build an open-source dashboard that combines public operational data from the Energy Information Administration with weather, interconnection queue, solar resource, and selected market price data. Instead of treating these sources as separate charts, the project asks a more practical question: what would a small retail electricity provider, virtual power plant operator, clean energy developer, or policy analyst need to see in order to understand operational risk and transition opportunity for a balancing authority?

The project evolved from a data visualization exercise into a decision-oriented investigation tool. Early versions focused on loading EIA demand, forecast, generation, and interchange data and turning them into useful charts. As we tested the app, we realized that users needed interpretation, not just access to more plots. That led us to reorganize the app around six connected modules: Executive Briefing, Anomaly Detection, Arbitrage Signals, Transition Scoring, Compliance Reports, and About. We added a shared investigation context using Streamlit session state so a user could move across modules while keeping the same balancing authority, route, or ISO location in focus. We also added peer-median comparisons, "Why this matters" notes, a policy recommendation card, and a transparent contradiction detector that flags tensions such as strong transition potential but weak queue activity.

On the code side, the platform uses a two-stage architecture. The ETL layer in `load_to_bigquery.py` pulls data from external APIs and writes raw and aggregated tables to BigQuery. The dashboard in `app.py` then loads those tables once with Streamlit caching and serves all pages from in-memory DataFrames, which makes page navigation fast after startup. The analytical logic lives mostly in `data_processing.py`: forecast error calculations, BA anomaly status, interchange route scoring, LMP anomaly detection, transition scoring, queue summaries, and compliance-style reporting. We also included `validation.py` for Pandera schemas and tests for important processing behavior. This separation made the project easier to reason about because data fetching, validation, transformation, and presentation each had a defined role.

One of my main takeaways was that public data can be powerful, but only if the surrounding engineering makes it usable. Working with EIA and related grid datasets required careful handling of timestamps, balancing authority codes, missing values, inconsistent source coverage, and the difference between operational data and true market data. I also learned that a useful policy dashboard should be honest about uncertainty. For example, the arbitrage module does not claim to prove a tradable spread when LMP coverage is missing; it presents persistent physical flow patterns as a signal worth investigating. Similarly, the transition score is a transparent composite, not a black-box prediction.

This project also changed how I think about the role of software in policy analysis. A notebook can answer one question, but a dashboard has to support repeated exploration by someone who may not know the data structure in advance. That pushed us to build explanatory interface elements, preserve context across pages, and translate raw metrics into decision language. The final product is still a student project with real limitations, but it demonstrates a full pipeline from public data ingestion to cloud storage, analytical scoring, interactive visualization, and policy-relevant interpretation.

## App Screenshots

<img width="1920" height="879" alt="1777693181182" src="https://github.com/user-attachments/assets/c29f09e3-41ba-40e7-837c-aaf840480461" />

<img width="1920" height="879" alt="1777693207149" src="https://github.com/user-attachments/assets/2b1d10c6-2258-4b0b-834d-797ced1682ac" />

<img width="1920" height="879" alt="1777693236048" src="https://github.com/user-attachments/assets/5258ceb7-657c-413d-8c26-23a723a5ff31" />

<img width="1920" height="879" alt="1777693266319" src="https://github.com/user-attachments/assets/6fb9d11e-8d05-4021-a922-e0c21803223d" />

<img width="1920" height="879" alt="1777693294577" src="https://github.com/user-attachments/assets/256f3142-1545-4571-817f-ba5c70d1c5d1" />

<img width="1920" height="879" alt="1777693316959" src="https://github.com/user-attachments/assets/2bbdff5e-0f15-492c-8e97-e103c29cad45" />

<img width="1920" height="879" alt="1777693340517" src="https://github.com/user-attachments/assets/d3a50ea8-5873-400f-896a-c1382d8e339f" />

<img width="1920" height="879" alt="1777693350319" src="https://github.com/user-attachments/assets/320f1758-110f-48b2-b25a-f218a30e313d" />

<img width="1920" height="879" alt="1777693371131" src="https://github.com/user-attachments/assets/1257b29c-fa36-433d-a67f-7388efa4349d" />

<img width="1920" height="879" alt="1777693407233" src="https://github.com/user-attachments/assets/9ba0291d-2572-44fe-9737-d51c8918cdc2" />

## Project README

- [Original project README file](https://github.com/advanced-computing/silly-penguin/blob/main/README.md)
