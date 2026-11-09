# RetailIQ architecture

```mermaid
flowchart LR
    user["User"]

    subgraph interactive["Interactive Streamlit application"]
        ui["Streamlit interface and orchestration<br/>app.py"]
        upload["Upload CSV, XLS, or XLSX"]
        reader["Read upload with Pandas<br/>app.py"]
        raw["Raw Pandas DataFrame"]
        profile["Profile columns, types, missing values,<br/>duplicates, and date fields<br/>src/analyzer.py"]
        clean["Build cleaning plan and clean data<br/>src/cleaner.py"]
        cleaned["Cleaned Pandas DataFrame"]
        clean_profile["Profile cleaned data<br/>src/analyzer.py"]
        state[("Streamlit session state<br/>raw_df, cleaned_df, profiles, and results<br/>app.py")]

        upload --> reader --> raw
        raw --> profile
        raw --> clean --> cleaned --> clean_profile
        raw --> state
        cleaned --> state
        profile --> state
        clean_profile --> state

        subgraph analytics["Interactive analytics"]
            eda["EDA and statistics<br/>app.py"]
            features["Feature engineering<br/>src/dav_support.py"]
            supervised["Classification and regression<br/>src/ml_analyzer.py<br/>src/advanced_ml.py"]
            comparison["Model comparison and hybrid modeling<br/>src/dav_support.py"]
            explain["SHAP and LIME explainability<br/>src/dav_support.py"]
            clustering["K-Means clustering<br/>src/ml_analyzer.py"]
            anomaly["Isolation Forest anomaly detection<br/>src/ml_analyzer.py"]
            forecast["Lag-based forecasting<br/>src/advanced_ml.py"]
            insights["Recommendations and data story<br/>src/recommender.py<br/>src/dav_support.py<br/>src/advanced_ml.py"]
        end

        state --> eda
        state --> features
        state --> supervised
        state --> comparison
        state --> explain
        state --> clustering
        state --> anomaly
        state --> forecast
        state --> insights

        plots["Plotly visualizations<br/>app.py"]
        export["Download filtered data as CSV<br/>app.py"]
        eda --> plots
        features --> plots
        supervised --> plots
        comparison --> plots
        explain --> plots
        clustering --> plots
        anomaly --> plots
        forecast --> plots
        insights --> plots
        state --> export
        plots --> ui
        export --> ui
    end

    subgraph research["Separate research evaluation flow"]
        research_ui["Research Evaluation action<br/>app.py"]
        cli["Optional command-line entry point<br/>scripts/run_research_suite.py"]
        suite["Benchmark and evaluation runner<br/>src/research_suite.py"]
        datasets["Research benchmark inputs<br/>research_datasets/"]
        eval["Classification, regression, clustering,<br/>anomaly, and forecasting evaluations"]
        outputs[("Research CSV artifacts<br/>results/research_suite/")]
        summary["Research summary and saved artifacts<br/>shown in app.py"]

        research_ui --> suite
        cli --> suite
        datasets --> suite
        suite --> eval --> outputs
        eval --> summary
        outputs --> summary
        summary --> research_ui
    end

    user --> ui
    ui --> upload
    ui --> research_ui
```

Uploaded working data, profiles, filters, and interactive analysis results are held in Streamlit session state; the application does not persist uploads to a database. Interactive visualizations are rendered in the app, and filtered data can be downloaded by the user as CSV. Research evaluation is a separate benchmark workflow: it reads from `research_datasets/`, writes evaluation CSV files under `results/research_suite/`, and displays its summary and saved artifacts in the app.
