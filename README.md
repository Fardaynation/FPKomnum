# - Code
---
``` python
class BesselInterpolator:
    def __init__(self, h, f0, f1, delta_f0, delta2_f_1, delta2_f0):
        self.h = h
        self.f0 = f0
        self.f1 = f1
        self.delta_f0 = delta_f0
        self.delta2_f_1 = delta2_f_1
        self.delta2_f0 = delta2_f0

    def interpolate(self, x_target, x0):
        u = (x_target - x0) / self.h

        # Rumus Bessel
        term1 = (self.f0 + self.f1) / 2
        term2 = (u - 0.5) * self.delta_f0
        term3 = (u * (u - 1) / 2) * ((self.delta2_f_1 + self.delta2_f0) / 2)

        fx = term1 + term2 + term3
        return round(fx, 2)

    def error_relative(self, actual, estimated):
        et = abs((actual - estimated) / actual) * 100
        return round(et, 2)


# Data dari tabel
h = 3
x0 = 15
x_target = 16
actual_value = 897104

f0 = 634575      # f(15)
f1 = 1673874     # f(18)
delta_f0 = 1039299   # Δf(15)
delta2_f_1 = 296784  # Δ²f(12)
delta2_f0 = 589680   # Δ²f(15)

# Inisialisasi dan proses interpolasi
bessel = BesselInterpolator(h, f0, f1, delta_f0, delta2_f_1, delta2_f0)
estimated = bessel.interpolate(x_target, x0)
error = bessel.error_relative(actual_value, estimated)

# Output
print(f"Hasil interpolasi Bessel di x = {x_target}: {estimated}")
print(f"Nilai sebenarnya: {actual_value}")
print(f"Error Term/Suku Kesalahan (Et): {error}%")

```
---
# Explanation
---
* Formula used:
$$
f(x) \approx \frac{f_0 + f_1}{2} + \left(u - \frac{1}{2}\right)\Delta f_0 + \frac{u(u - 1)}{2} \cdot \frac{\Delta^2 f_{-1} + \Delta^2 f_0}{2}
$$

With:

* $f_0$ = Value of starting function (in this case `x=15`)
* $f_1$ = Value of function afterwards (`x=18`)
* $\Delta f_0$ = First difference of values `x=15`
* $\Delta^2 f_0$ = Second difference of values `x=15`
* $\Delta^2 f_{-1}$ = Second difference of values (`x=12`)
* $u = \frac{x - x_0}{h}$, Interpolation parameter
* $h$ = Distance between value (in the given example, $h = 3$)
---
Breakdown on each functions in the code:
---

## 🧩 **Penjelasan Program per Bagian**

### 1. **Kelas BesselInterpolator**

```python
class BesselInterpolator:
    def __init__(self, h, f0, f1, delta_f0, delta2_f_1, delta2_f0):
        ...
```

* Ini adalah class Python untuk menyimpan semua parameter interpolasi.
* Parameter yang dibutuhkan:

  * `h`: selisih antar x (dalam kasus ini, 3)
  * `f0`: nilai fungsi di `x0 = 15`
  * `f1`: nilai fungsi di `x1 = 18`
  * `delta_f0`: selisih pertama di `x = 15`
  * `delta2_f_1`: selisih kedua di titik sebelumnya, `x = 12`
  * `delta2_f0`: selisih kedua di titik utama, `x = 15`

---

### 2. **Fungsi `interpolate()`**

```python
def interpolate(self, x_target, x0):
    u = (x_target - x0) / self.h
```

* Menghitung parameter `u` berdasarkan lokasi target relatif terhadap `x0`.
* Karena `x_target = 16` dan `x0 = 15`, serta `h = 3`, maka:

  $$
  u = \frac{16 - 15}{3} = \frac{1}{3} \approx 0.333
  $$

Selanjutnya menghitung 3 suku dalam rumus Bessel:

```python
term1 = (self.f0 + self.f1) / 2
```

* Ini adalah suku pertama: rata-rata dari nilai fungsi `f0` dan `f1`.

```python
term2 = (u - 0.5) * self.delta_f0
```

* Suku kedua: efek dari selisih pertama (Δf) dikalikan faktor penyesuaian terhadap `u`.

```python
term3 = (u * (u - 1) / 2) * ((self.delta2_f_1 + self.delta2_f0) / 2)
```

* Suku ketiga: pengaruh dari selisih kedua (Δ²f) dirata-ratakan, kemudian dikalikan dengan bentuk kuadratik dari `u`.

```python
fx = term1 + term2 + term3
return round(fx, 2)
```

* Hasil akhir dari interpolasi adalah jumlah dari ketiga suku tersebut, lalu dibulatkan ke dua angka desimal.

---

### 3. **Fungsi `error_relative()`**

```python
def error_relative(self, actual, estimated):
    et = abs((actual - estimated) / actual) * 100
    return round(et, 2)
```

* Fungsi ini menghitung **galat relatif** (relative error) dalam persen:

$$
E_t = \left|\frac{y_{\text{aktual}} - y_{\text{estimasi}}}{y_{\text{aktual}}}\right| \times 100\%
$$

---

### 4. **Pemanggilan dan Output**

```python
# Data dari tabel
h = 3
x0 = 15
x_target = 16
actual_value = 897104
```

Nilai-nilai dari tabel:

* `f0 = 634575` (y di x = 15)
* `f1 = 1673874` (y di x = 18)
* `Δf(15) = 1039299`
* `Δ²f(12) = 296784`
* `Δ²f(15) = 589680`

---

### 5. **Output Program**

```python
print(f"Hasil interpolasi Bessel di x = {x_target}: {estimated}")
print(f"Nilai sebenarnya: {actual_value}")
print(f"Error Term/Suku Kesalahan (Et): {error}%")
```

Misalnya, jika hasil estimasi adalah `931760.0`, maka:

* Hasil interpolasi ≈ 931760.0
* Nilai aktual = 897104
* Galat relatif = 3.86%

---
  
