# RetailIQ Streamlit Frontend

The Streamlit entry point is `app.py` at the repository root. The analytics and ML modules are in the root-level `src/` package.

It preserves the existing backend modules under `src/` and provides:
- premium SaaS dark/light UI
- collapsible-style sidebar navigation
- adaptive KPI cards
- multiple dataset-driven dashboard visualizations
- key insights
- recommended next steps
- Data and Data Quality workspaces
- Breakdown and Relationships
- separate Classification, Regression, Clustering, Forecasting and Anomaly Detection pages
- recommendation hub
- advanced Research Evaluation access
- CSV/XLSX/XLS upload
- session-state persistence

The application and logo are already in their project locations; no file copying is required. A `.streamlit/config.toml` is not present or required for startup.

From the repository root, run:
`python -m streamlit run app.py`
