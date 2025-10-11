# toronto-equity-dashboard
Social equity indices of city of Toronto

Illustrative File Structure:

toronto-equity-dashboard/

├── 📁 data/

│   ├── raw/                  # Original exports from Fabric (CSV, GeoJSON, etc.)

│   ├── processed/            # Cleaned and transformed datasets

│   └── metadata/             # Data dictionary, schema notes, sources

├── 📁 notebooks/

│   ├── analysis.ipynb        # Exploratory data analysis

│   ├── geospatial.ipynb      # GeoPandas/Folium mapping

│   └── dashboard.ipynb       # Final dashboard logic

├── 📁 dashboard/

│   ├── app.py                # Streamlit or Panel app

│   ├── components/           # Custom widgets or layout modules

│   └── assets/               # Logos, icons, images

├── 📁 docs/

│   ├── README.md             # Project overview

│   ├── methodology.md        # Analysis and modeling approach

│   └── changelog.md          # Version history

├── 📁 .github/

│   └── workflows/            # GitHub Actions for automation

├── requirements.txt          # Python dependencies

└── LICENSE                   # Optional open-source license
