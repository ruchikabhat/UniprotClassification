# UniprotClassification
# Convert Uniprot IDs to protein classification
import sys
import os
import requests
print(f"Python version being used: {sys.version}")

import streamlit as st
import pandas as pd
import numpy as np
import plotly.express as px

# Download function
def download_h5ad_file():
    url = "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE240nnn/GSE240016/suppl/GSE240016_CD45neg_thymic_stroma_d0147+annotation.h5ad"
    local_filename = "thymic_stroma.h5ad"
    
    if not os.path.exists(local_filename):
        st.info("Downloading data file...")
        response = requests.get(url, stream=True)
        response.raise_for_status()
        with open(local_filename, 'wb') as f:
            for chunk in response.iter_content(chunk_size=8192):
                f.write(chunk)
        st.success("Download complete!")
    return local_filename

# Set page configuration
st.set_page_config(page_title="scRNA-seq Analysis Tool", layout="wide")

# Title and introduction
st.title("Single-cell RNA Sequencing Analysis Tool")
st.write("Upload your scRNA-seq data for interactive analysis and visualization")


# Create some sample data for testing
np.random.seed(42)
n_cells = 100
n_genes = 50

data = pd.DataFrame(
    np.random.normal(0, 1, size=(n_cells, n_genes)),
    columns=[f"Gene_{i}" for i in range(n_genes)]
)

# Show data preview
st.write("Sample data preview:")
st.dataframe(data.head())

# Create a simple visualization
fig = px.scatter(
    data.iloc[:, :2],
    x=data.columns[0],
    y=data.columns[1],
    title="Sample Gene Expression Visualization"
)
st.plotly_chart(fig)
