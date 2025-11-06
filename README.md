# FPL Squad Selection

This project builds a **data-driven Fantasy Premier League (FPL) squad optimizer**, using real performance data from the **first 10 matchweeks** to select:
1. An **optimal 15-player squad** under official FPL budget & squad constraints.
2. The **best possible starting XI** for **Matchweek 11**, maximizing expected short-term return.
3. **Captain and Vice-Captain** assignments based on forecasted performance.

This is not a subjective “eye test” team. The entire squad and lineup were generated through **statistical projection** and **integer linear programming optimization**.

---

## 🎯 Objective

To remove bias and guesswork from FPL team selection by:
- Using real performance + context data to estimate **expected points** for the upcoming matchweek.
- Optimizing the squad and starting XI under actual FPL rules.
- Prioritizing **value, form, and favourable fixture conditions**.

---

## 📊 Data Source

- Player statistics and metadata: **FPL Official API** (`bootstrap-static`)
- Fixture difficulty / upcoming opponent: **FPL Fixtures API**
- Data window: **Matchweeks 1 to 10**
- Output target: **Matchweek 11**

No third-party scrapers or unofficial datasets were used.

---

## 🧠 Approach

### 1. Data Cleaning & Filtering
- Removed players with extremely low playing time to avoid distorted points-per-90 values.
- Merged team, fixture, availability, and player form data.

### 2. Expected Score Calculation
A custom short-term projection model:

expected_score = points_per_90 * fixture_factor * form_factor * availability_factor

- **Fixture factor:** Based on opponent difficulty rating.
- **Form factor:** Recent form normalized relative to player pool mean.
- **Availability factor:** Accounts for injuries / doubtful status.

### 3. Squad Optimization (15 players)
Solved using Integer Linear Programming with the real constraints in FPL:
- £100M budget
- **2 GK / 5 DEF / 5 MID / 3 FWD**
- Max **3 players per club**

### 4. Starting XI Optimization
Second ILP model selects the optimal formation that maximize total expected return under the FPL constraints:
- 1 GK
- ≥ 3 DEF
- ≥ 2 MID
- ≥ 1 FWD

### 5. Captain Choice
- Captain = highest expected points player in starting XI
- Vice = second highest

---

## Final Squad (15 Players)

| Player | Team | Position | Price (£m) | Expected Score |
|---|---|---|---:|---:|
| Raya | Arsenal | GK | 5.8 | 5.69 |
| Petrović | Bournemouth | GK | 4.5 | 3.80 |
| Gabriel | Arsenal | DEF | 6.6 | **16.06** |
| James | Chelsea | DEF | 5.5 | 12.37 |
| Van de Ven | Spurs | DEF | 4.8 | 8.74 |
| Guéhi | Crystal Palace | DEF | 5.0 | 7.01 |
| Chalobah | Chelsea | DEF | 5.1 | 6.73 |
| Casemiro | Man Utd | MID | 5.5 | **15.16** |
| Mbeumo | Man Utd | MID | 8.4 | 11.68 |
| Rice | Arsenal | MID | 6.8 | 9.92 |
| Neto | Chelsea | MID | 7.1 | 9.63 |
| Sessegnon | Fulham | MID | 5.4 | 8.46 |
| Haaland | Man City | FWD | 14.8 | 11.69 |
| Welbeck | Brighton | FWD | 6.5 | 9.38 |
| Mateta | Crystal Palace | FWD | 7.9 | 8.44 |

**Total Squad Cost:** £99.7M (within the £100M limit)

---

## Starting XI for Matchweek 11

| Player | Position | Expected Score |
|---|---|---:|
| Raya | GK | 5.69 |
| Gabriel | DEF | **16.06** |
| James | DEF | 12.37 |
| Van de Ven | DEF | 8.74 |
| Casemiro | MID | **15.16** |
| Mbeumo | MID | 11.68 |
| Rice | MID | 9.92 |
| Neto | MID | 9.63 |
| Sessegnon | MID | 8.46 |
| Haaland | FWD | 11.69 |
| Welbeck | FWD | 9.38 |

### Captaincy
- **Captain:** **Gabriel**
- **Vice-Captain:** **Casemiro**

---

## 🖼️ Formation

<img width="784" height="567" alt="image" src="https://github.com/user-attachments/assets/80e1e98e-9de7-4d7f-8ca7-cb17eebce244" />

## ⚠️ Limitations & Considerations

| Limitation | Explanation |
|---|---|
| **Short data window** | Using only MW1–10 limits reliability for teams/players that change sharply afterward. |
| **Fixture factor is simplified** | Using general FDR values is less precise than expected goals-based defensive metrics. |
| **Rotation risk** | Model does not predict manager decisions, tactical rotation, or late fitness updates. |
| **Captaincy selection does not include risk modeling** | Captain chosen purely by expected mean, not upside/downside variance. |
| **Injuries can break projections** | Player availability is treated numerically; late injury news must be checked before finalizing transfers. |

---

## Future Enhancements

- Integrate xG/xGA-driven opponent difficulty modeling.
- Use bookmaker win/goal probabilities to refine expected score.
- Add minute-probability model to reduce rotation risk from high-variance players.
- Run Monte Carlo simulations to evaluate **confidence intervals** of expected returns.

---

## How to Run / Reproduce

1. **Clone the repository**
   ```bash
   git clone [https://github.com/yourusername/your-repo-name.git](https://github.com/SamAbr/FPL-Squad-Selection/)
   cd FPL-Squad-Selection
  
2. **Create & activate a virtual environment**
  ```bash
  python -m venv venv
  source venv/bin/activate   # Windows: venv\Scripts\activate
  ```
3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Run the project**

  Jupyter Notebook: FPL.ipynb

## License & Contact

This project is released under the **MIT License**, which allows anyone to use, modify, and distribute the code, provided proper credit is given.

**Author:** *Samuel Ab. Gebremariam*  
**Portfolio:** https://samabr.github.io/Sam  
**GitHub:** https://github.com/samabr  
**Email:** *abrhsamuel@gmail.com)*  




