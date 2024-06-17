# Import relevant libraries
import pandas as pd
import re
import streamlit as st
import plotly.express as px
import plotly.graph_objects as go

# -------------------------------------------------------------------------------------------------------------------------------------------
# Open csv file
file_path = "vpn.csv"
df = pd.read_csv(file_path)
# -------------------------------------------------------------------------------------------------------------------------------------------
# Clean the data
df[['Country', 'location_city']] = df['location_selected'].str.split('-', n=1, expand=True)
df['location_city'] = df['location_city'].str.extract(r"([a-zA-Z]+)")
df.drop(['location_selected', 'location_city', 'user_country'], axis=1, inplace= True) 

# Modify the data types
# Date
df['date'] = pd.to_datetime(df["date"], format="%Y-%m-%d").dt.date

# Drop rows with NaN values
df = df.dropna()
# -------------------------------------------------------------------------------------------------------------------------------------------
# Simple streamlit application
st.image("streamlit.png")

st.write("""
# Simple VPN Data Visualisation App
         
Displayed are the VPN characteristics across various countries and protocols.
         
         """)

st.write("Dataset of the App")
st.dataframe(df)  # Same as st.write(df)
# -------------------------------------------------------------------------------------------------------------------------------------------
# user axis selection
st.subheader('VPN Dataset')
x_axis = st.selectbox('Choose a Variable for the x-axis:', ['protocol', 'Country', 'date'], index=0)
y_axis = st.selectbox('Choose a Variable for the y-axis:', ['time_to_connect', 'download_speed', 'latency'], index = 1)
avg_df = df.groupby(x_axis)[y_axis].median().reset_index()

if x_axis != 'date':
    fig = px.bar(avg_df, x = x_axis, y = y_axis)
    st.plotly_chart(fig)     

else:
    fig = px.line(avg_df, x = x_axis, y = y_axis)
    st.plotly_chart(fig)

# -------------------------------------------------------------------------------------------------------------------------------------------
# Scatter Matrix
scat_matrix = px.scatter_matrix(df, dimensions=["time_to_connect", "download_speed", "latency"], color="Country")
scat_matrix.update_layout(title= 'Scatter Matrix of VPN Features by Countries')
st.plotly_chart(scat_matrix)
# -------------------------------------------------------------------------------------------------------------------------------------------
# Heatmap
selected_columns = ["time_to_connect", "download_speed", "latency"]
corr = df[selected_columns].corr()
corr_matrix = go.Figure(data=go.Heatmap(
    z=corr.values,
    x=corr.columns,
    y=corr.columns,
    colorscale='Viridis'))

corr_matrix.update_layout(title='Heatmap of VPN Features Correlation')
st.plotly_chart(corr_matrix)
# -------------------------------------------------------------------------------------------------------------------------------------------
