# Raw Code
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

        # Formula
        term1 = (self.f0 + self.f1) / 2
        term2 = (u - 0.5) * self.delta_f0
        term3 = (u * (u - 1) / 2) * ((self.delta2_f_1 + self.delta2_f0) / 2)

        fx = term1 + term2 + term3
        return round(fx, 2)

    def error_relative(self, actual, estimated):
        et = abs((actual - estimated) / actual) * 100
        return round(et, 2)


# Data obtained from table
h = 3
x0 = 15
x_target = 16
actual_value = 897104

f0 = 634575      # f(15)
f1 = 1673874     # f(18)
delta_f0 = 1039299   # Δf(15)
delta2_f_1 = 296784  # Δ²f(12)
delta2_f0 = 589680   # Δ²f(15)

# For inisialising and run the process of interpolation
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
## Formula used:
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
## Breakdown on each functions in the code:
---
### 1. **BesselInterpolator class**

```python
class BesselInterpolator:
    def __init__(self, h, f0, f1, delta_f0, delta2_f_1, delta2_f0):
        ...
```

* Used to store needed parameters.
* Parameters:

  * `h`: Difference between each x (in our given question, 3)
  * `f0`: Function values in `x0 = 15`
  * `f1`: Function values in `x1 = 18`
  * `delta_f0`: First difference of values in `x = 15`
  * `delta2_f_1`: Second difference in values of `x = 12`
  * `delta2_f0`: Second difference of values in `x = 15`

---

### 2. **`interpolate() function`**

```python
def interpolate(self, x_target, x0):
    u = (x_target - x0) / self.h
```

* To count parameter `u` based on `x0`.
* Because `x_target = 16` and `x0 = 15`, with `h = 3`, ergo:

  $$
  u = \frac{16 - 15}{3} = \frac{1}{3} \approx 0.333
  $$

Next, calculate 3 functions in Bessel:

```python
term1 = (self.f0 + self.f1) / 2
```

* First one: Average value of `f0` and `f1`.

```python
term2 = (u - 0.5) * self.delta_f0
```

* Second: First difference (Δf) multiplied by `u`.

```python
term3 = (u * (u - 1) / 2) * ((self.delta2_f_1 + self.delta2_f0) / 2)
```

* Third: Value of (Δ²f) averaged, then multiplied with quadratic form of `u`.

```python
fx = term1 + term2 + term3
return round(fx, 2)
```

* End result is the addition of the previously calculated values, then round it to two digits behind comma.

---

### 3. **`error_relative() function`**

```python
def error_relative(self, actual, estimated):
    et = abs((actual - estimated) / actual) * 100
    return round(et, 2)
```

* This function computes **Et** in percentage:

$$
E_t = \left|\frac{y_{\text{aktual}} - y_{\text{estimasi}}}{y_{\text{aktual}}}\right| \times 100\%
$$

---

### 4. **Data**

```python
h = 3
x0 = 15
x_target = 16
actual_value = 897104
```

Values from given table:

* `f0 = 634575` (y di x = 15)
* `f1 = 1673874` (y di x = 18)
* `Δf(15) = 1039299`
* `Δ²f(12) = 296784`
* `Δ²f(15) = 589680`

---

### 5. **Output**

```python
print(f"Hasil interpolasi Bessel di x = {x_target}: {estimated}")
print(f"Nilai sebenarnya: {actual_value}")
print(f"Error Term/Suku Kesalahan (Et): {error}%")
```
---
## Result:
---
Hasil interpolasi ≈ 931760.0
Nilai aktual = 897104
Galat relatif = 3.86%
---
## Screenshot:
---
(https://i.imgur.com/a/1diWYIa.jpeg)
