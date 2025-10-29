# ScreenSense — Kids’ Screentime Visualization  

**Project Statement:**  
Analyze kids’ screentime patterns to uncover trends by **age**, **gender**, **location type** (urban/rural), **device type**, **day of week**, and **activity category** using data visualization.  
The goal is to present clear, actionable insights for parents, educators, and policymakers.

---

## 📊 Dataset  
**Source:** [Kaggle — Indian Kids Screentime 2025](https://www.kaggle.com/datasets/ankushpanday2/indian-kids-screentime-2025)  
**Format:** CSV file containing fields like:  
`date`, `age`, `gender`, `location_type`, `device_type`, `activity_category`, and `duration_min`.

---

## 🧠 Project Overview  
This repository documents progress from **Week 1 to Week 6**, covering:
1. Data loading and inspection  
2. Data cleaning and preprocessing  
3. Exploratory (univariate and bivariate) analysis  
4. Device, activity, and weekday/weekend analysis  
5. Cohort and segment insights  
6. Seasonal and habit pattern exploration  

*(Week 7 — Visual Report & Dashboard will be added later.)*

---

## ⚙️ Tech Stack  
- **Python** (pandas, numpy)  
- **Visualization:** matplotlib, seaborn, plotly  
- **Documentation:** Jupyter Notebook, Markdown (README)  

---

## 📅 Week-wise Progress Summary  

### 🗓️ Week 1 — Dataset Setup & Exploration  
**Objectives:**  
- Define project scope and goals.  
- Load and inspect dataset structure.  
- Identify missing values and data quality issues.  

**Key Code:**
```python
import pandas as pd
df = pd.read_csv('data/raw/indian_kids_screentime_2025.csv')
print(df.shape)
print(df.info())
print(df.isnull().sum())
df.head()
```

---

### 🧹 Week 2 — Data Cleaning & Feature Engineering  
**Objectives:**  
- Handle missing or inconsistent data.  
- Create `age_band`, `is_weekend`, and `duration_hours` fields.  
- Save cleaned dataset for later analysis.  

**Key Code:**
```python
import pandas as pd
df = pd.read_csv('data/raw/indian_kids_screentime_2025.csv')

df['gender'] = df['gender'].str.title().fillna('Unknown')
df['location_type'] = df['location_type'].str.title().fillna('Unknown')
df['date'] = pd.to_datetime(df['date'], errors='coerce')
df['weekday'] = df['date'].dt.day_name()
df['is_weekend'] = df['weekday'].isin(['Saturday','Sunday'])

bins = [0,5,8,11,14,17,100]
labels = ['0-5','6-8','9-11','12-14','15-17','18+']
df['age_band'] = pd.cut(df['age'], bins=bins, labels=labels)

df['duration_hours'] = df['duration_min'] / 60
df.to_csv('data/processed/screentime_cleaned.csv', index=False)
print("✅ Cleaned dataset saved!")
```

---

### 📊 Week 3 — Univariate & Bivariate Visuals  
**Objectives:**  
- Visualize distributions (age, duration, gender).  
- Compare screentime patterns across groups.  

**Key Code:**
```python
import pandas as pd
import plotly.express as px

df = pd.read_csv('data/processed/screentime_cleaned.csv')

fig = px.histogram(df, x='duration_hours', nbins=40, title='Distribution of Session Hours')
fig.show()

agg = df.groupby('age_band')['duration_hours'].mean().reset_index()
fig2 = px.bar(agg, x='age_band', y='duration_hours', title='Average Screentime by Age Band')
fig2.show()

fig3 = px.box(df, x='gender', y='duration_hours', title='Screentime by Gender')
fig3.show()
```

---

### 💻 Week 4 — Device & Activity Mix + Weekday/Weekend Trends  
**Objectives:**  
- Study device usage mix (mobile, TV, tablet, laptop).  
- Compare weekday vs weekend behavior.  
- Visualize activity composition across demographics.  

**Key Code:**
```python
import pandas as pd
import plotly.express as px

df = pd.read_csv('data/processed/screentime_cleaned.csv')

dev = df['device_type'].value_counts().reset_index()
dev.columns = ['device','count']
fig = px.pie(dev, names='device', values='count', title='Device Usage Mix')
fig.show()

wk = df.groupby('is_weekend')['duration_hours'].mean().reset_index()
fig2 = px.bar(wk, x='is_weekend', y='duration_hours', title='Average Hours: Weekday vs Weekend')
fig2.show()

act = df.groupby(['location_type','activity_category']).size().reset_index(name='count')
fig3 = px.bar(act, x='location_type', y='count', color='activity_category', barmode='stack', title='Activity Mix by Location')
fig3.show()
```

---

### 🧩 Week 5 — Cohort & Segment Analysis  
**Objectives:**  
- Identify top cohorts (e.g., age × device).  
- Analyze regional or demographic differences.  
- Build heatmaps and grouped visuals.  

**Key Code:**
```python
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go

df = pd.read_csv('data/processed/screentime_cleaned.csv')

pivot = df.groupby(['age_band','location_type'])['duration_hours'].mean().reset_index()
heat = pivot.pivot(index='age_band', columns='location_type', values='duration_hours')

fig = go.Figure(data=go.Heatmap(z=heat.values, x=heat.columns, y=heat.index, colorscale='Greens'))
fig.update_layout(title='Heatmap: Avg Hours by Age Band & Location')
fig.show()

cohort = df.groupby(['age_band','device_type'])['duration_hours'].mean().reset_index()
fig2 = px.bar(cohort, x='age_band', y='duration_hours', color='device_type', barmode='group', title='Avg Hours by Age & Device')
fig2.show()
```

---

### 📆 Week 6 — Seasonal & Habit Patterns  
**Objectives:**  
- Track screentime changes over months.  
- Study weekday patterns by age group.  
- Summarize behavioral trends.  

**Key Code:**
```python
import pandas as pd
import plotly.express as px

df = pd.read_csv('data/processed/screentime_cleaned.csv')
df['month'] = pd.to_datetime(df['date']).dt.to_period('M').astype(str)

month_trend = df.groupby('month')['duration_hours'].mean().reset_index()
fig = px.line(month_trend, x='month', y='duration_hours', title='Monthly Average Screentime (Hours)')
fig.update_layout(xaxis_tickangle=-45)
fig.show()

pattern = df.groupby(['weekday','age_band'])['duration_hours'].mean().reset_index()
days = ['Monday','Tuesday','Wednesday','Thursday','Friday','Saturday','Sunday']
pattern['weekday'] = pd.Categorical(pattern['weekday'], categories=days, ordered=True)
fig2 = px.line(pattern, x='weekday', y='duration_hours', color='age_band', title='Weekday Patterns by Age Band')
fig2.show()
```

---

## 🧾 Deliverables Summary (Weeks 1–6)
| Week | Deliverable | Output |
|------|--------------|--------|
| 1 | Dataset setup | `data/raw/` |
| 2 | Cleaned dataset | `data/processed/screentime_cleaned.csv` |
| 3 | Visual analysis | 8+ charts (univariate/bivariate) |
| 4 | Device & activity insights | 3–4 visuals |
| 5 | Segment analysis | Heatmap, grouped bars |
| 6 | Seasonal patterns | Monthly/weekday visuals |

---

## 💬 Author
**Prepared by:** Shivam  
*(Week 7 – Final Dashboard will be added separately.)*
