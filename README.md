# Amazing Grace Cleaning Services – Local Streamlit Dashboard

A local Python dashboard for showcasing Amazing Grace Cleaning Services' business overview, service performance, cost and quotation analysis, payment and collection analysis, and three-month sales/collections forecasting.

## Important

- This application runs locally. It does not deploy to Azure, Render, Streamlit Community Cloud, or any other cloud host.
- No Power BI licence is required because the charts are recreated in Streamlit/Plotly rather than embedding Power BI.
- The sales forecast uses completed-job quoted amount as a proxy because a separate final invoiced amount is not available in the included workbook.

## Project structure

```text
Amazing_Grace_Local_Streamlit_App/
├── app.py
├── requirements.txt
├── README.md
├── run_local.bat
├── run_local.sh
├── .streamlit/
│   └── config.toml
└── data/
    └── Amazing_Grace_Cleaning_Dataset_Refined.xlsx
```

## Run locally on Windows

1. Install Python 3.10 or later.
2. Extract this project folder.
3. Open Command Prompt or the VS Code terminal in the project folder.
4. Create and activate a virtual environment:

```powershell
py -m venv .venv
.venv\Scripts\activate
```

5. Install dependencies:

```powershell
python -m pip install -r requirements.txt
```

6. Start the app:

```powershell
python -m streamlit run app.py
```

The app normally opens at `http://localhost:8501`.

You can also double-click `run_local.bat` after installing dependencies.

## Run locally on macOS/Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
streamlit run app.py
```

Or execute `bash run_local.sh` after installing dependencies.

## Use another workbook

The bundled workbook loads automatically. To use a revised workbook, select **Upload an updated Excel workbook** in the app sidebar. It must contain these sheets:

- `Jobs`
- `Labour_Payments`
- `Payments`
- `Job_Collections`
- `Materials_Usage`
- `Staff_Lunch_and_Commute`

The app removes fully identical duplicate job rows and excludes job IDs that have conflicting duplicate records. It combines agreed labour, estimated materials, and staff lunch/commute expenses to calculate estimated direct job costs.

## Forecasting approach

The forecasting page forecasts two monthly series independently:

1. Completed-job quoted value, using completion date (or job date if completion date is missing) as a sales proxy.
2. Cash collections, using payment transaction date and amount received.

It compares Naive, Seasonal Naive, and Exponential Smoothing (ETS) models using expanding-window rolling-origin validation with a three-month horizon. Models are selected by WAPE, with MAE and bias also displayed. Forecast bounds are approximate validation-error ranges.

## Data and cost limitations

The included workbook does not contain a complete allocation of general overhead, owner remuneration, taxes, fuel and equipment costs. The contribution measure is therefore an estimated contribution after the direct cost components included in the workbook; it is not net profit.

## Local access limitation

`localhost` means the application is accessible from the computer running it. Other devices cannot access it automatically. This package is designed for a local presentation/showcase, not remote owner access.
