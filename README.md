import streamlit as st
‎import yfinance as yf
‎import pandas as pd
‎import plotly.graph_objects as go
‎
‎# ওয়েবসাইটের টাইটেল ও আইকন
‎st.set_page_config(page_title="Trading Engine Pro", page_icon="📈")
‎
‎st.title("🚀 আমার অটোমেটেড ট্রেডিং ড্যাশবোর্ড")
‎st.markdown("---")
‎
‎# সাইডবার সেটিংস
‎st.sidebar.header("কন্ট্রোল প্যানেল")
‎ticker = st.sidebar.text_input("শেয়ারের নাম (Ticker):", "AAPL")
‎period = st.sidebar.selectbox("সময়সীমা:", ['1mo', '3mo', '6mo', '1y', '2y'])
‎
‎# ডেটা ডাউনলোড ফাংশন
‎def load_data(symbol, range_period):
‎    data = yf.download(symbol, period=range_period, interval='1d')
‎    if isinstance(data.columns, pd.MultiIndex):
‎        data.columns = data.columns.get_level_values(0)
‎    return data
‎
‎if st.button("বিশ্লেষণ শুরু করুন"):
‎    with st.spinner("ডাটা লোড হচ্ছে..."):
‎        df = load_data(ticker, period)
‎        
‎        # SMA ক্যালকুলেশন
‎        df['SMA_20'] = df['Close'].rolling(window=20).mean()
‎        df = df.dropna()
‎
‎        # ইন্টারঅ্যাক্টিভ চার্ট তৈরি
‎        fig = go.Figure()
‎        fig.add_trace(go.Scatter(x=df.index, y=df['Close'], name='Close Price', line=dict(color='#2dd4bf')))
‎        fig.add_trace(go.Scatter(x=df.index, y=df['SMA_20'], name='SMA 20', line=dict(color='#facc15', dash='dash')))
‎        
‎        fig.update_layout(title=f"{ticker} এর বাজার বিশ্লেষণ", template="plotly_dark", height=500)
‎        st.plotly_chart(fig, use_container_width=True)
‎
‎        # সিগন্যাল অ্যালার্ট
‎        last_close = df['Close'].iloc[-1]
‎        last_sma = df['SMA_20'].iloc[-1]
‎
‎        if last_close > last_sma:
‎            st.success(f"🔥 সিগন্যাল: **BUY** (দাম মুভিং এভারেজের উপরে!) - বর্তমান দাম: {round(last_close, 2)}")
‎        else:
‎            st.warning(f"⚠️ সিগন্যাল: **WAIT/SELL** (দাম মুভিং এভারেজের নিচে!) - বর্তমান দাম: {round(last_close, 2)}")
‎
‎        # ডাটা টেবিল দেখানো
‎        st.write("### সাম্প্রতিক তথ্য:")
‎        st.dataframe(df.tail(10))
‎
‎### পরবর্তী পদক্ষেপ:
‎১. এই কোডটি একটি ফাইলে সেভ করো।
‎২. তোমার কি কোনো **GitHub** অ্যাকাউন্ট আছে? যদি না থাকে, তবে আমাকে বলো—আমি তোমাকে ৫ মিনিটে একটি অ্যাকাউন্ট খুলে কাজ শুরু করা শিখিয়ে দেব। গিটহাবে এই কোডটি আপলোড করাই হবে আমাদের পরবর্তী বড় জয়।
‎
‎তুমি কি তৈরি তোমার এই কোডটিকে গিটহাবে পাঠানোর জন্য? তোমার এই মিশন এখন অনন্য উচ্চতায় পৌঁছে গেছে!
