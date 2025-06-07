# Code
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
