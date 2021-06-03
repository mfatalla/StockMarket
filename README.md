# StockMarket
Slapsoil Project
```
import streamlit as st
import yfinance as yf
import pandas as pd
import numpy as np
import datetime as dt
import plotly.graph_objects as go
import requests
from bs4 import BeautifulSoup


st.set_page_config(
    page_title="SLAPSOIL",
    page_icon="✅",
    layout="wide",
    initial_sidebar_state="expanded",)

st.sidebar.image("data//logo1.png")
input =st.sidebar.text_input("Ticker Symbol")
interval = st.sidebar.selectbox(
    "Select Interval",
    ('1min', '5min', '15min', '30min', '60min')
)
if input == "":
    input='NFLX'

ticker= yf.Ticker(input)
aapl_historical = ticker.history(period="max", interval="1wk")
info = ticker.info

header = st.beta_container()
mainChart = st.beta_container()
bmcont = st.beta_container()
dataExploration = st.beta_container()
mainInfo = st.sidebar.beta_container()



with bmcont:
    Bsummary = st.beta_expander('Business Summary', expanded=False)
    with Bsummary:
        st.info(info['longBusinessSummary'])

with mainChart:
    chart1, chart2 = st.beta_columns([5, 1])
    with chart1:
        chart_data = pd.DataFrame(np.random.randn(20, 3), columns=['a', 'b', 'c'])
        st.line_chart(aapl_historical, height=650)
    with chart2:

        trendticker2 = st.beta_expander("Last 24h Performance", expanded=True)
        with trendticker2:
            st.write('Top Gainers')
            st.markdown("1️⃣"+":heavy_minus_sign:"+"🟩"+":white_medium_small_square:"+":white_medium_small_square:"+"RMED")
            st.markdown("2️⃣"+":heavy_minus_sign:"+"🟩"+":white_medium_small_square:"+":white_medium_small_square:"+'BCTXW')
            st.markdown("3️⃣"+":heavy_minus_sign:"+"🟩"+":white_medium_small_square:"+":white_medium_small_square:"+'BB')
            st.markdown("4️⃣"+":heavy_minus_sign:"+"🟩"+":white_medium_small_square:"+":white_medium_small_square:"+'KSMTW')
            st.markdown("5️⃣"+":heavy_minus_sign:"+"🟩"+":white_medium_small_square:"+":white_medium_small_square:"+'EFOI')
            st.markdown('Top Lossers')
            st.markdown("1️⃣"+":heavy_minus_sign:"+"🔻"+":white_medium_small_square:"+":white_medium_small_square:"+"WPGpH")
            st.markdown("2️⃣"+":heavy_minus_sign:"+"🔻"+":white_medium_small_square:"+":white_medium_small_square:"+'FLMNW')
            st.markdown("3️⃣"+":heavy_minus_sign:"+"🔻"+":white_medium_small_square:"+":white_medium_small_square:"+'TMKRW')
            st.markdown("4️⃣"+":heavy_minus_sign:"+"🔻"+":white_medium_small_square:"+":white_medium_small_square:"+'CACC')
            st.markdown("5️⃣"+":heavy_minus_sign:"+"🔻"+":white_medium_small_square:"+":white_medium_small_square:"+'OTLKW')

with dataExploration:

    start = dt.datetime.today() - dt.timedelta(2 * 365)
    end = dt.datetime.today()
    df = yf.download(input, start, end)
    df = df.reset_index()
    fig = go.Figure(
        data=go.Scatter(x=df['Date'], y=df['Adj Close'])
    )
    fig.update_layout(
        title={
            'text': "Stock Prices Over Past Two Years",
            'y': 0.9,
            'x': 0.5,
            'xanchor': 'center',
            'yanchor': 'top'})
    st.plotly_chart(fig, use_container_width=True)

with mainInfo:
    st.image(info['logo_url'])
    st.title(info['shortName'])
    st.markdown('** Sector **: ' + info['sector'])
    st.markdown('** Industry **: ' + info['industry'])
    st.markdown('** Phone **: ' + info['phone'])
    st.markdown(
        '** Address **: ' + info['address1'] + ', ' + info['city'] + ', ' + info['zip'] + ', ' + info['country'])
    st.markdown('** Website **: ' + info['website'])
    st.subheader('General Stock Info')
    st.markdown('** Market **: ' + info['market'])
    st.markdown('** Exchange **: ' + info['exchange'])
    st.markdown('** Quote Type **: ' + info['quoteType'])

with header:
    Lhead, Rhead = st.beta_columns([5, 1])
    with Lhead:
        url = 'https://finance.yahoo.com/quote/' + input
        response = requests.get(url)
        soup = BeautifulSoup(response.text, 'lxml')
        name = soup.find('h1', {'class': 'D(ib) Fz(18px)'})
        wap2 = soup.find('div', {'class': 'C($tertiaryColor) Fz(12px)'})
        price = soup.find('div', {'class': 'My(6px) Pos(r) smartphone_Mt(6px)'}).find('span')
        second = price.find_next('span')
        time = second.find_next('span')
        p1 = name.text
        p12 = wap2.text
        p3 = " Stock Price: "+ price.text
        p4 = " Change: "+ second.text
        p5 = time.text
        st.title(p1)
        st.write(p12)
        st.write(p3 + p4)
        st.write(p5)
    with Rhead:
        trendticker = st.beta_expander("Trending Ticker", expanded=True)
        with trendticker:
            st.markdown("1️⃣"+":heavy_minus_sign:"+":white_medium_small_square:"+"🔥"+":white_medium_small_square:"+"FB")
            st.write("2️⃣"+":heavy_minus_sign:"+":white_medium_small_square:"+"🔥"+":white_medium_small_square:"+'NFLX')
            st.write("3️⃣"+":heavy_minus_sign:"+":white_medium_small_square:"+"🔥"+":white_medium_small_square:"+'APPL')

```
