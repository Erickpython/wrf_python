To create a professional meteorological suite of plots for Kenya, you need a mix of surface, atmospheric, and diagnostic variables. 

Below is a comprehensive summary table for WRF variables. These are categorized by their physical impact on East African weather, specifically targeting the unique dynamics of the **Kenya Highlands**, the **Lake Victoria Basin**, and the **Indian Ocean coastline**.

### WRF Meteorological Plotting Reference Table

| Category | Parameter | WRF Variable(s) | Units | Recommended Colormap | Professional "Popping" Choice |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Moisture** | **Total Precipitation** | `RAINC + RAINNC` | mm | `YlGnBu`, `Blues` | `ocean_r` |
| | **Precipitable Water** | `PW` (diagnostic) | $kg/m^2$ | `GnBu`, `YlGnBu` | `viridis` |
| | **Relative Humidity** | `rh` (calculated) | % | `BrBG`, `Greens` | `YlGn` |
| | **Cloud Fraction** | `CLDFRA` | 0 - 1 | `Greys_r` | `bone` |
| **Thermal** | **2m Temperature** | `T2` | °C (Subtract 273.15) | `RdYlBu_r`, `Spectral_r` | `magma` |
| | **Surface Skin Temp** | `TSK` | °C (Subtract 273.15) | `hot`, `inferno` | `plasma` |
| | **Heat Index** | Calculated from T2 & RH | °C | `YlOrRd` | `afmhot` |
| **Dynamics** | **Sea Level Pressure** | `slp` (diagnostic) | hPa | `coolwarm` | `RdBu_r` |
| | **10m Wind Speed** | `sqrt(U10^2 + V10^2)` | Knots or m/s | `Blues`, `Speed` | `YlGnBu` |
| | **Vert. Velocity** | `W` | m/s | `seismic`, `bwr` | `PuOr` |
| | **Vorticity** | Calculated from U, V | $s^{-1}$ | `PiYG`, `PRGn` | `curl` |
| **Energy** | **CAPE** | `MCAPE` or `CAPE` | J/kg | `YlOrBr` | `gist_heat` |
| | **Surface Flux** | `HFX` (Sensible Heat) | $W/m^2$ | `RdGy_r` | `autumn` |

---

### Pro-Tips for Plotting These Variables in Kenya

#### 1. The Precipitation "Accumulation" Catch
When plotting `RAINC` and `RAINNC`, remember that WRF provides **accumulated** rainfall from the start of the simulation. To show hourly rainfall for your animation, you must subtract the previous time step from the current one:
> `hourly_precip = rain_total.isel(Time=t) - rain_total.isel(Time=t-1)`

#### 2. Pressure and Altitude
Kenya has extreme elevation changes. When plotting **Sea Level Pressure (SLP)**, ensure you use the diagnostic `slp` provided by libraries like `wrf-python`. Standard pressure (`P` or `PB`) will mostly just show the shape of the mountains rather than the actual weather systems.

#### 3. CAPE for Thunderstorms
If you want to forecast lightning or heavy storms over Nairobi or the Lake region, plot **CAPE (Convective Available Potential Energy)**. Use a colormap like `YlOrBr` and set your levels to start at **500 J/kg**, as anything below that is usually non-convective.

#### 4. Relative Humidity (RH)
RH is not usually a direct output in the WRF `wrfout` file; it must be calculated using temperature, pressure, and mixing ratio (`QVAPOR`). Most users use the `wrf.getvar(ds, "rh")` function to handle this automatically.

