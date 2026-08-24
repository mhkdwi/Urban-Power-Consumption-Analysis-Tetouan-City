# data-analysis-electric-power-consumption  

Saya menganalisis data dari kaggle yaitu data konsumsi power listrik di kota Tetouan, Maroko pada tahun 2018. (https://www.kaggle.com/datasets/fedesoriano/electric-power-consumption)  

Tetouan merupaka kota bersejarah di utara Maroko yang dijuluki sebagai "Kota Putih" karena dominasi warna putih pada bangunan-bangunannya.  
  
Kota ini memiliki 4 musim:  
1. Musim Panas (Juni – September)&emsp;&emsp;&emsp;: suhu udara rata-rata bisa mencapai 27°-30° celcius  
2. Musim Gugur (Oktober – November)&emsp;: suhu udara rata-rata 20° celcius  
3. Musim Dingin (Desember – Maret)&emsp;&emsp;: suhu udara rata-rata berkisar 9°-16° celcius  
4. Musim Semi (April – Mei)&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;: suhu uara rata-rata berkisar 15°-22° celcius  

![Teks Alternatif](Tetouan.jpeg)
<p align="center"><i>Kota Tetouan</i></p>  

Data ini berisi pencatatan setiap 10 menit sekali sepanjang tahun 2018, dengan total 52.416 baris data.  
Saya menyederhanakan data ini menggunakan python dan pandas untuk melakukan beberapa proses:  
1. Resampling data dari per 10 menit menjadi rata-rata per jam (*hourly*).
2. Menghitung total penggunaan daya dari gabungan Zone 1, 2, dan 3. 
3. Serta menambah kolom pendukung seperti jam, nama hari, dan penanda *weekday/weekend*.

```python
import pandas as pd

# Load dataset utama
df = pd.read_csv('powerconsumption.csv')

# Format tanggal & hitung total konsumsi
df['Datetime'] = pd.to_datetime(df['Datetime'])
df['Total_Power_Consumption'] = (
    df['PowerConsumption_Zone1'] + 
    df['PowerConsumption_Zone2'] + 
    df['PowerConsumption_Zone3']
)

# Resampling data per 10 menit menjadi Rata-Rata per Jam
df_hourly = df.resample('1h', on='Datetime').mean().reset_index()

# Menambahkan kolom pendukung untuk analisis
df_hourly['Hour'] = df_hourly['Datetime'].dt.hour
df_hourly['Month'] = df_hourly['Datetime'].dt.month_name()
df_hourly['Day_Name'] = df_hourly['Datetime'].dt.day_name()
df_hourly['Is_Weekend'] = df_hourly['Datetime'].dt.dayofweek.isin([5, 6]).map({True: 'Weekend', False: 'Weekday'})

# Simpan data bersih
df_hourly.to_csv('power_consumption_cleaned.csv', index=False)


