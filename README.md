# water-quality-portfolio
rajab-sani-water-quality/
│── README.md
│── requirements.txt
│── .gitignore
│
├── data/
│   └── water_quality.csv
│
├── scripts/
│   ├── analysis.py
│   └── preprocessing.py
│
├── dashboard/
│   └── app.py
│
├── results/
│   ├── cod_trend.png
│   ├── bod_trend.png
│   ├── correlation_matrix.png
│   └── summary.txt
│
└── assets/
    └── preview.png
    Location,Date,pH,COD,BOD,TSS
A1,2024-01-01,7.1,120,60,80
A1,2024-02-01,7.0,135,68,88
A1,2024-03-01,6.9,150,75,95
A2,2024-01-01,7.5,90,45,60
A2,2024-02-01,7.4,105,52,68
A2,2024-03-01,7.3,115,58,75
A3,2024-01-01,6.8,160,85,105
A3,2024-02-01,6.7,175,92,115
A3,2024-03-01,6.6,190,100,130
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv('../data/water_quality.csv')
df['Date'] = pd.to_datetime(df['Date'])

# ======================
# Trend COD
# ======================
for loc in df['Location'].unique():
    subset = df[df['Location'] == loc]
    plt.plot(subset['Date'], subset['COD'], marker='o', label=loc)

plt.title('COD Trend')
plt.xlabel('Date')
plt.ylabel('COD (mg/L)')
plt.legend()
plt.savefig('../results/cod_trend.png')
plt.clf()

# ======================
# Trend BOD
# ======================
for loc in df['Location'].unique():
    subset = df[df['Location'] == loc]
    plt.plot(subset['Date'], subset['BOD'], marker='o', label=loc)

plt.title('BOD Trend')
plt.xlabel('Date')
plt.ylabel('BOD (mg/L)')
plt.legend()
plt.savefig('../results/bod_trend.png')
plt.clf()

# ======================
# Correlation
# ======================
corr = df[['pH','COD','BOD','TSS']].corr()

plt.imshow(corr)
plt.xticks(range(len(corr.columns)), corr.columns)
plt.yticks(range(len(corr.columns)), corr.columns)
plt.colorbar()
plt.title('Correlation Matrix')
plt.savefig('../results/correlation_matrix.png')

# ======================
# Summary Insight
# ======================
summary = f"""
Average COD: {df['COD'].mean()}
Average BOD: {df['BOD'].mean()}
Highest Pollution Location: {df.groupby('Location')['COD'].mean().idxmax()}
"""

with open('../results/summary.txt', 'w') as f:
    f.write(summary)
