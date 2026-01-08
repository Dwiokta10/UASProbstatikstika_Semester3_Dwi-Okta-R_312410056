# UAS PROBABILITAS DAN STATISTIKA

|                |                    |
| ------------------ | ------------------ |
|      _Nama_    | Dwi Okta Ramadhani |
|      _NIM_     |      312410056     |
|     _Kelas_    |      TI.24.A3      |
|  _Mata Kuliah_ | Probabilitas Dan Statistika|

# Analisis Data Zomato Menggunakan Python

Analisis ini dilakukan menggunakan dataset **`zomato-data-.csv`** yang berisi **148 data restoran** dengan **7 variabel utama**, yaitu `name`, `online_order`, `book_table`, `rate`, `votes`, `approx_cost(for two people)`, dan `listed_in(type)`.

Tujuan dari analisis ini adalah untuk memahami **preferensi pelanggan**, **tren jenis restoran**, serta **pengaruh layanan online terhadap popularitas dan rating restoran** berdasarkan data nyata dari platform Zomato.

---

## Pembacaan dan Struktur Dataset

Langkah pertama dalam analisis adalah membaca dataset dan meninjau struktur data untuk memastikan data siap digunakan.

**Short Code:**

```python
dataframe.head()
dataframe.info()
```

Berdasarkan hasil pembacaan data, dataset terdiri dari **148 baris data** dan seluruh kolom memiliki nilai lengkap tanpa data kosong (null). Kolom `rate` telah dikonversi menjadi tipe numerik (float), sedangkan `votes` dan `approx_cost(for two people)` bertipe integer. Kondisi ini menunjukkan bahwa dataset sudah **bersih dan layak untuk dianalisis lebih lanjut**.

---

## Pembersihan dan Transformasi Data Rating

Kolom `rate` pada dataset awalnya berbentuk teks seperti `4.1/5`, sehingga tidak dapat langsung digunakan dalam analisis statistik. Oleh karena itu, dilakukan proses pembersihan dan konversi menjadi nilai numerik.

**Short Code:**

```python
def handleRate(value):
    value = str(value).split('/')
    return float(value[0])

dataframe['rate'] = dataframe['rate'].apply(handleRate)
```

Setelah proses ini, nilai rating seperti `4.1/5` berhasil diubah menjadi `4.1`. Transformasi ini memungkinkan analisis statistik seperti distribusi rating, perhitungan median, dan visualisasi boxplot.

---

## Analisis Jenis Restoran

Untuk mengetahui jenis restoran yang paling dominan, dilakukan analisis pada kolom `listed_in(type)`.

**Short Code:**

```python
sns.countplot(x=dataframe['listed_in(type)'])
```

Hasil analisis menunjukkan bahwa jenis restoran yang paling sering muncul adalah **Dining** dan **Cafes**, diikuti oleh **Buffet** dan **Other**. Hal ini mengindikasikan bahwa mayoritas restoran pada dataset berfokus pada layanan makan di tempat dan kafe, sesuai dengan preferensi konsumen.

---

## Total Votes Berdasarkan Jenis Restoran

Selanjutnya dilakukan pengelompokan data berdasarkan jenis restoran untuk melihat total votes pada setiap kategori.

**Short Code:**

```python
dataframe.groupby('listed_in(type)')['votes'].sum()
```

Hasil pengelompokan menunjukkan bahwa kategori **Dining** memiliki total votes paling tinggi dibandingkan kategori lainnya. Ini menandakan bahwa restoran dengan jenis Dining memiliki **tingkat interaksi pelanggan paling besar** di platform Zomato.

---

## Restoran dengan Jumlah Votes Tertinggi

Untuk mengetahui restoran paling populer, dilakukan pencarian nilai maksimum pada kolom `votes`.

**Short Code:**

```python
max_votes = dataframe['votes'].max()
dataframe.loc[dataframe['votes'] == max_votes, 'name']
```

Berdasarkan hasil tersebut, **Empire Restaurant** tercatat sebagai restoran dengan jumlah votes tertinggi, yaitu **4.884 votes**. Hal ini menunjukkan bahwa Empire Restaurant merupakan restoran **paling banyak dipilih dan paling populer** dalam dataset.

---

## Ketersediaan Layanan Pesanan Online

Analisis berikutnya dilakukan untuk melihat jumlah restoran yang menyediakan layanan pesanan online.

**Short Code:**

```python
sns.countplot(x=dataframe['online_order'])
```

Hasil visualisasi menunjukkan bahwa restoran yang menyediakan layanan **online order lebih banyak** dibandingkan yang tidak. Hal ini mengindikasikan bahwa layanan pemesanan online telah menjadi **standar penting** dalam industri kuliner saat ini.

---

## Distribusi Rating Restoran

Distribusi rating dianalisis untuk melihat kecenderungan penilaian pelanggan terhadap restoran.

**Short Code:**

```python
plt.hist(dataframe['rate'], bins=5)
```

Hasil distribusi menunjukkan bahwa mayoritas restoran memiliki rating di kisaran **3.5 hingga 4.2**, dengan sangat sedikit restoran yang memiliki rating ekstrem rendah atau tinggi. Ini menunjukkan bahwa kualitas restoran pada dataset secara umum berada pada kategori **cukup baik dan stabil**.

---

## Kisaran Harga Favorit untuk Dua Orang

Analisis harga dilakukan pada kolom `approx_cost(for two people)` untuk mengetahui kisaran harga yang paling diminati.

**Short Code:**

```python
sns.countplot(x=dataframe['approx_cost(for two people)'])
```

Hasil analisis menunjukkan bahwa kisaran harga yang paling sering muncul berada pada rentang **300–600**. Hal ini menandakan bahwa pasangan cenderung memilih restoran dengan **harga menengah** yang dianggap seimbang antara kualitas dan biaya.

---

## Perbandingan Rating Restoran Online dan Offline

Untuk melihat pengaruh layanan online terhadap rating restoran, dilakukan perbandingan menggunakan boxplot.

**Short Code:**

```python
sns.boxplot(x='online_order', y='rate', data=dataframe)
```

Hasil analisis menunjukkan bahwa restoran yang menyediakan layanan **online order memiliki median rating sedikit lebih tinggi dan variasi rating yang lebih kecil**, sehingga dapat dikatakan lebih konsisten dalam kualitas layanan.

---

## Hubungan Jenis Restoran dan Mode Pesanan

Analisis hubungan antara jenis restoran dan mode pesanan dilakukan menggunakan pivot table.

**Short Code:**

```python
dataframe.pivot_table(
    index='listed_in(type)',
    columns='online_order',
    aggfunc='size',
    fill_value=0
)
```

Hasil analisis menunjukkan bahwa kategori **Dining** dan **Cafes** lebih dominan dalam menyediakan layanan online order, sementara beberapa kategori lainnya masih bergantung pada layanan offline. Hal ini menunjukkan bahwa jenis restoran memengaruhi **strategi layanan pemesanan**.

---

## Kesimpulan

Berdasarkan seluruh hasil analisis dan implementasi coding menggunakan dataset **`zomato-data-.csv`**, dapat disimpulkan bahwa:

* Jenis restoran **Dining** merupakan kategori paling dominan
* **Empire Restaurant** adalah restoran paling populer berdasarkan jumlah votes
* Layanan **online order** berpengaruh positif terhadap konsistensi rating
* Kisaran harga favorit untuk dua orang berada pada rentang **300–600**
* Seluruh pertanyaan analisis awal berhasil dijawab menggunakan pendekatan statistik dan visualisasi data

# Terimakasih
