import streamlit as st
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import altair as alt

st.title('Dollar Exchange rate')

data_load = st.text('Loading data')
df_data = pd.read_excel('data.xlsx')
data_load.text('Data has been loaded')
df_data['Date'].fillna(0)
if st.checkbox('Show row data'):
    st.subheader('data')
    st.write(df_data)

st.write(f"This contains the data from {df_data.values[3][0]} to {df_data['Date'].iloc[-1]}")

chart = alt.Chart(df_data).mark_line().encode(
    x=alt.X('Date:N'),
    y=alt.Y('Price:Q'),
).properties(title="Graph")
st.altair_chart(chart, use_container_width=True)

st.subheader('Convert')

select_box = st.selectbox(
    "Convert to",
    ['Dollars','rupees']
)
rate = df_data['Price'].iloc[-1]
if select_box == 'rupees':
    st.number_input('Enter the amount of dollars to convert', key='value')
    amount = st.session_state.value * rate
    st.write(amount)
if select_box == 'Dollars':
    st.number_input('Enter the amount of rupees to convert', key='value')
    amount = st.session_state.value / rate
    st.write(amount)

