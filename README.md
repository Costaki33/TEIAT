## TexNet Earthquake Injection Analysis Tool 
By RE. Constantinos Skevofilax and PhD. Alexandros Savvaidis of BEG TexNet

### Introduction 
The Earthquake Injection Correlation Tool is designed to efficiently analyze the relationship between oil and gas fracking operations and the occurrence of induced earthquakes.

The tool plots reported well injection volumes and pressures, and calculates the daily bottomhole pressure experienced underground for the N closest oil and gas wells to a specific earthquake. The goal is to assess the potential influence of these operations on earthquake activity.

Traditionally, in the field of seismology, it is recognized that oil and gas operations can induce earthquakes, as evidenced by the increased volume and frequency of seismic activity in oil and gas fields. However, the underlying mechanisms behind this connection are not yet fully understood. The goal of this tool is to provide an accessible way to visualize trends in these fields, helping to develop a clearer understanding of how human activities contribute to earthquake induction.

## Features
- Visualizes well injection volumes and pressures provided by either TexNet or B3.
- Calculates daily bottomhole pressures for nearby wells.
- Plots injection data trends, including missing and incomplete records.
- Generates reports and visual outputs for deeper analysis.

## Directory Structure

### 1. Storage of Generated Outputs
This directory will store all images and files generated by the TEIAT tool.

### 2. Data Source Directory
This directory will hold critical data resources that the tool will reference for processing calculations.

## How to Use 

1. **Clone the Repository:**
   Clone the repository to your local environment using:
   ```bash
   git clone [<repository-url>](https://github.com/Costaki33/TEIAT.git)

2. **Setup Directories:**
   Create two directories:
   - One to store generated outputs (images and files)
        Modify the following path in the `teiat.py` script:
         ```python
            # Line 24:
            OUTPUT_DIR = '/your/output/directory/path'
         ```
   - Another to hold data sources for processing (IE. Strawn Formation Data File)
     
3. **Gather Required Data:**
   - Gather the Strawn Formation Data file (Latitude, Longitude, and Z Depth):
        Modify the following path in the `teiat.py` script:
        ```python
            # Line 23:
            STRAWN_FORMATION_DATA_FILE_PATH = '/your/earthquake_data/TopStrawn_RD_GCSWGS84.csv'
        ```
   - Gather API Tube OD/ID File:
        Modify the following path in the `friction_loss_calc.py` script:
        ```python
            # Line 14:
            tubing_information = '/your/earthquake_data/cmpl_tubing_data.csv'
        ```
## Code Logic

- **Input Earthquake Data:** The user provides earthquake data from the SeisComP FDSNWS Event - URL Builder in CSV format.

- **Find Closest Wells:** The tool locates the N closest wells (based on API number) to the earthquake within a set range (default: 20 km). It fetches well data from the TexNet Injection Tool API.

- **Data Quality Analysis:**
  - **Complete Data:** Well Injection BBL and Avg Pressure PSIG available.
  - **Incomplete Data:** Well Injection BBL available but Avg Pressure PSIG missing.
  - **Missing Data:** Both Well Injection BBL and Avg Pressure PSIG are missing.

- **Data Validation:** Validates injection data within ±1 year of the earthquake.

- **Calculate Bottomhole Pressure:** Calculates bottomhole pressure using the formula provided by Jim Moore (RRC):
    ```python
    Bottomhole pressure = surface pressure + hydrostatic pressure - deltaP
    ```
    - **Hydrostatic pressure formula:** `0.465 psi/ft X depth (ft)`
    - **Friction loss (DeltaP):** Computed using the Colebrook-White equation for turbulent flow.

- **Well Sorting:** Wells are sorted as deep or shallow based on the Strawn Formation.

- **Visualization:** Generates plots and histograms for injection pressure trends, differentiating between deep and shallow wells.

## Running the Code

Run the following in your code environment:

```bash
python3 inj_pandas_optimized.py 1
```

