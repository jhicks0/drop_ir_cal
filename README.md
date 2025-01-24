# drop_ir_cal
This project is an analysis/characterization of the Melexis MLX90614 (versions DCH, BCF, DCI) IR sensors for integration into the NCAR NRD41 dropsonde for measuring Sea Surface Temperature (SST). Of primary interest is to understand how well the sensors compensate for thermal gradients.

## Versions
|PART NUMBER           |Temperature Code|Voltage                |Thermopiles             |FOV                     |
|----------------------|----------------|-----------------------|------------------------|------------------------|
|MLX90614ESF-DCH-000-SP|E (-40°C...80°C)|D (3V medical accuracy)|C (gradient compensated)|H (12°, refractive lens)|
|MLX90614ESF-BCF-000-SP|E (-40°C...80°C)|B (3V)                 |C (gradient compensated)|F (10°)                 |
|MLX90614ESF-DCI-000-SP|E (-40°C...80°C)|D (3V medical accuracy)|C (gradient compensated)|I (5°)                  |

## Methods

### Initial testing
In the NCAR/EOL Calibration Lab, the IR sensors are set up in a custom 'jig' designed and built by DFS for the custom blackbody insert. The blackbody is installed in the Fluke 7040 oil bath and set to 25°C to limit the influence of the ambient room temperature on the test. Three data streams are setup: from the IR sensor itself (controlled and captured by the Melexis evaluation board), from the four thermistors attached to the blackbody (controlled by the Wisard board and captured by the lab computer, and from the Fluke 1594A Super-thermometer/5699 SPRT probe (captured by the lab computer).

Once the bath temperature has stabilized, data is collected for ~1 minute. Then, the temperature of the IR sensor body is raised (need to experiment with integrated heater coil versus heat gun) for a moment (max 5 seconds). Once finished, the system is allowed to stabilize (~30 seconds). Next, three puffs of air are blown past the sensor lens (spaced approximately 5 seconds apart). After this, data is collected for another ~1 minute, before shutting down.

IR data is saved to PN_SN_IR_YYYYMMDDTHHMM at 8Hz. Thermistor data is saved to PN_SN_TH_YYYYMMDDTHHMM at 1 sample/2 to 3 seconds. Thermistor data is recorded in raw A to D counts and is subsequently converted to Kelvin and then to Celsius. The IR data either needs to be downsampled or the sample rate decreased. The thermistor data either needs to be upsampled or the sample rate increased. SPRT data is saved to PN_SN_ST_YYYYMMDDTHHMM at 1Hz, different than the default of 0.5Hz. This is done from the 1594A by navigating to Main Menu>Measure Menu>Timing Settings>Sample Period and changing 2 to 1. All data is merged into one data file organized by date-timestamp.

### Data Analysis

|Sensor|Variable|Units|Notes                     |
|------|--------|-----|--------------------------|
|IR    |Ta      |C    |"ambient" (package temp)  |
|      |To      |C    |"object" (IR)             |
|ST    |T       |C    |reference                 |
|TH    |T1      |C    |need to identify position |
|      |T2      |C    |need to identify position |
|      |T3      |C    |need to identify position |
|      |T4      |C    |need to identify position |