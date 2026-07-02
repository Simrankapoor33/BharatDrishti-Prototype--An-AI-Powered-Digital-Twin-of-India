#  BharatDrishti — AI-Powered Digital Twin of India
### Bharatiya Antariksh Hackathon (BAH) 2026 | ISRO

<div align="center">

![India](https://img.shields.io/badge/🇮🇳-Digital%20Twin%20of%20India-orange?style=for-the-badge)
![ISRO](https://img.shields.io/badge/ISRO-BAH%202026-blue?style=for-the-badge)
![AI](https://img.shields.io/badge/AI-Powered-green?style=for-the-badge)
![Team](https://img.shields.io/badge/Team-NeuralBharat-purple?style=for-the-badge)
![Live](https://img.shields.io/badge/Live-Deployed-brightgreen?style=for-the-badge)

**A dynamic, high-fidelity AI climate intelligence platform — modeling India's atmosphere, ocean, and land surface using national satellite data.**

###  [Live Demo → bharat-drishti-prototype-an-ai-powe.vercel.app](https://bharat-drishti-prototype-an-ai-powe.vercel.app/)

</div>

---


- Severe Cyclone Alert
- Heatwave Alert
- Heavy Rainfall Warning (>5mm/hr)

---

## What is BharatDrishti?

BharatDrishti is an **AI-Powered Digital Twin of India's Climate Systems** — a real-time spatiotemporal model that replicates India's atmospheric, oceanic, and land surface processes at high resolution.

> *"A virtual India that breathes, rains, and heats — just like the real one."*

###  Problem Statement
India faces increasing climate extremes — monsoon variability, heatwaves, droughts, and cyclones — all of which demand accurate, high-resolution, and timely prediction systems. Conventional NWP (Numerical Weather Prediction) models lack the spatial granularity and AI-driven adaptability needed for modern climate monitoring.

###  Our Solution
BharatDrishti leverages **Deep Learning + Indian National Satellite Data** to build a climate digital twin that:
- Predicts temperature and rainfall at high spatial resolution
- Forecasts 7 days ahead with daily granularity
- Detects extreme events like heatwaves and droughts
- Runs as an interactive web dashboard for real-time visualization

---

##  Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   BharatDrishti Platform                 │
├──────────────┬──────────────────┬───────────────────────┤
│   Data Layer │   AI Model Layer │   Visualization Layer  │
├──────────────┼──────────────────┼───────────────────────┤
│ INSAT-3D     │ Model 1:         │ Interactive Web        │
│ Oceansat     │ Rainfall LSTM    │ Dashboard (Mapbox GL)  │
│ IMD Grid     │                  │                        │
│ MOSDAC       │ Model 2:         │ State-wise Climate     │
│ ERA5         │ Temp ConvLSTM    │ Analytics              │
│ Hydrological │                  │                        │
│ Datasets     │ Model 3: (WIP)   │ Heatwave & Drought     │
│              │ Drought Index    │ Alert System           │
└──────────────┴──────────────────┴───────────────────────┘
```

---

##  AI Models

###  Model 1: Rainfall LSTM
| Parameter | Value |
|-----------|-------|
| Architecture | LSTM (3 layers) |
| Input | Multi-source rainfall data |
| Resolution | 0.25° (129×135 grid) |
| Lookback | 30 days |
| Horizon | 7 days |
| Target RMSE | < 3 mm/day |
| Status |  Complete |

---

###  Model 2: Temperature ConvLSTM
| Parameter | Value |
|-----------|-------|
| Architecture | ConvLSTM2D (3 encoder + decoder) |
| Input | IMD Max & Min Temp Binary Grid (.GRD) |
| Resolution | 1° (31×31 grid) |
| Lookback | 30 days |
| Horizon | 7 days |
| Normalize by | 60°C |
| Parameters | ~2.5M |
| Training platform | Kaggle GPU T4 x2 |
| Status |  Complete |

####  Model 2 Results
| Metric | T_max | T_min | Target |
|--------|-------|-------|--------|
| **RMSE** | **1.420°C** | **1.143°C** | < 1.5°C  |
| MAE | 0.754°C | 0.599°C | — |
| Bias | -0.207°C | -0.324°C | — |
| **Correlation** | **0.9468** | **0.9713** | — |
| Heatwave F1 | 0.451 | — | — |

####  Day-wise RMSE (Model 2)
| Day | T_max RMSE | T_min RMSE |
|-----|-----------|-----------|
| Day+1 | 1.424°C | 1.156°C |
| Day+2 | 1.423°C | 1.148°C |
| Day+3 | 1.422°C | 1.143°C |
| Day+4 | 1.421°C | 1.140°C |
| Day+5 | 1.420°C | 1.139°C |
| Day+6 | 1.418°C | 1.138°C |
| Day+7 | 1.414°C | 1.137°C |

>  **Key insight:** RMSE remains stable across all 7 forecast days — model generalizes well!

---

##  Project Structure

```
BharatDrishti-Prototype/
│
├──  index.html                            # Main web dashboard (Mapbox GL JS)
├──  index.css                             # Dashboard styling
├──  app.js                                # Interactive map logic
│
├──   Notebooks/
│   ├──  rainfall-model.ipynb              # Model 1: Rainfall LSTM
│   └──  Model2_Temperature_ConvLSTM.ipynb # Model 2: Temp ConvLSTM
│
└──  README.md
```

---

##  Data Sources

| Source | Type | Coverage | Years |
|--------|------|----------|-------|
| **INSAT-3D/3DS/3DR** | Satellite imagery | All India | 2013–2025 |
| **Oceansat-2/3** | Ocean surface data | Indian Ocean | 2009–2025 |
| **IMD Pune** | Max/Min Temp Grid (.GRD) | All India | 2000–2025 |
| **IMD Pune** | Rainfall Grid (.bin) | All India | 2000–2025 |
| **MOSDAC** | Multi-parameter data | All India | 2003–2025 |
| **ERA5** | Reanalysis products | Global | 1940–2025 |
| **Hydrological** | River & groundwater | All India | 2000–2025 |

---

##  Web Dashboard Features

| Feature | Description |
|---------|-------------|
|  **3D Mapbox GL map** | Interactive globe with India overlay |
|  **State-wise temperature** | Hover any state → climate data popup |
|  **Rainfall layer** | Spatial rainfall visualization |
|  **Heatwave alerts** | T_max > 40°C zones highlighted |
|  **7-day forecast** | Day-by-day temperature & rain |
|  **Day/Night toggle** | Switch between day and night data |
|  **INSAT data layers** | LST, SST, IMC real-time layers |
|  **Live alert system** | Cyclone, heatwave, rainfall warnings |
|  **Temporal analysis** | Historical (2000–2023) + Live + AI Forecast |

---

##  Getting Started

### Prerequisites
```bash
Python >= 3.11
TensorFlow >= 2.15
CUDA 11.8+ (for GPU training)
```

### Installation
```bash
# Clone the repository
git clone https://github.com/vikashkr96/BharatDrishti-Prototype--An-AI-Powered-Digital-Twin-of-India.git
cd BharatDrishti-Prototype--An-AI-Powered-Digital-Twin-of-India

# Install Python dependencies
pip install tensorflow numpy pandas matplotlib seaborn scikit-learn tqdm scipy

# Open dashboard
# Just open index.html in browser, or use Live Server in VS Code
```

### Run Model 2 (Temperature ConvLSTM)
```bash
# Place IMD .GRD files in:
#   Maxtemp_MaxT_2025/Maxtemp_MaxT_YYYY.GRD
#   Mintemp_MinT_2025/Mintemp_MinT_YYYY.GRD

# Open Notebooks/Model2_Temperature_ConvLSTM.ipynb
# Run all cells in order
# GPU recommended — trained on Kaggle T4 x2
```

---

##  Training Details (Model 2)

```
Platform     : Kaggle (GPU T4 x2)
Epochs       : 22 (Early stopping, best at epoch 12)
Batch size   : 16
Optimizer    : Adam (lr=1e-3)
Loss         : MSE
Best val_loss: 0.000453
Training time: ~2 hours (GPU)
Data years   : 2000–2025 (IMD .GRD binary format)
```

---

##  Why BharatDrishti?

| Feature | Conventional Models | BharatDrishti |
|---------|-------------------|---------------|
| Approach | Rule-based NWP | Deep Learning (ConvLSTM/LSTM) |
| Data sources | 1–3 sources | 7+ Indian national sources |
| Forecast accuracy | ~2–3°C RMSE | **1.14–1.42°C RMSE ** |
| Heatwave detection | Threshold-based | **AI-powered (F1=0.45)** |
| Forecast horizon | 3–5 days | **7 days (stable)** |
| India-specific data | Partial | **100% Indian data** |
| Dashboard | None | **Live 3D web dashboard** |
| Correlation | ~0.85 | **0.97** |

---

##  Hackathon Context

- **Event:** Bharatiya Antariksh Hackathon (BAH) 2026
- **Organized by:** ISRO (Indian Space Research Organisation)
- **Problem Domain:** Climate Monitoring & Disaster Preparedness
- **Track:** AI/ML for Space Applications
- **Live URL:** [bharat-drishti-prototype-an-ai-powe.vercel.app](https://bharat-drishti-prototype-an-ai-powe.vercel.app/)

---

##  Team NeuralBharat 🇮🇳

> *"Where Neural Networks meet Bharat's Climate"*

| # | Member | Role | GitHub |
|---|--------|------|--------|
| 1 | **Simran Kapoor** | Team Lead & DL Engineer | [@Simrankapoor33](https://github.com/Simrankapoor33) |
| 2 | **Vikash Kumar** | Frontend & Integration | [@vikashkr96](https://github.com/vikashkr96) |
| 3 | **Satyam** | Team Member | [@satyamforwork1987-create](https://github.com/satyamforwork1987-create) |
| 4 | **Prince Singh** | Team Member | — |

**College:** Maharishi Markandeshwar University (MM(DU)), Mullana

---

##  Future Work

- [ ] Model 3: Drought Index (High Temp + Low Rain)
- [ ] Model 4: Cyclone Track Prediction
- [ ] Real-time IMD API integration
- [ ] Mobile app (Flutter)
- [ ] Improved heatwave detection (class-weighted loss)
- [ ] 0.25° spatial resolution upgrade
- [ ] Multi-language dashboard (Hindi + English)

---

##  License

This project is licensed under the **Apache 2.0 License** — see [LICENSE](LICENSE) for details.

---

##  Acknowledgements

- **ISRO** for organizing BAH 2026
- **IMD Pune** for providing historical climate grid data
- **MOSDAC** for satellite data access
- **Kaggle** for GPU compute resources (T4 x2)
- **Mapbox** for the 3D mapping platform

---

<div align="center">

###  Team NeuralBharat 🇮🇳

**Made  for India**

*BharatDrishti — Seeing India Through AI Eyes* 

 **[Live Demo → bharat-drishti-prototype-an-ai-powe.vercel.app](https://bharat-drishti-prototype-an-ai-powe.vercel.app/)**

</div>
