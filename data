import streamlit as st
import pandas as pd
import numpy as np
import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots
import warnings
warnings.filterwarnings('ignore')

# Page config
st.set_page_config(
    page_title="E-commerce Analytics Dashboard",
    page_icon="🛒",
    layout="wide",
    initial_sidebar_state="expanded"
)

# Custom CSS
st.markdown("""
<style>
    .main-header {
        font-size: 2.5rem;
        color: #1f77b4;
        text-align: center;
        margin-bottom: 2rem;
        text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
    }
    .metric-card {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        padding: 1rem;
        border-radius: 15px;
        color: white;
        margin: 0.5rem 0;
        box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    }
    .section-header {
        background: linear-gradient(90deg, #ff6b6b, #4ecdc4);
        color: white;
        padding: 10px 20px;
        border-radius: 10px;
        margin: 20px 0 10px 0;
        text-align: center;
        font-weight: bold;
    }
    .risk-high {
        background-color: #ff4444;
        color: white;
        padding: 5px 10px;
        border-radius: 5px;
        font-weight: bold;
    }
    .risk-medium {
        background-color: #ffaa00;
        color: white;
        padding: 5px 10px;
        border-radius: 5px;
        font-weight: bold;
    }
    .risk-low {
        background-color: #00aa00;
        color: white;
        padding: 5px 10px;
        border-radius: 5px;
        font-weight: bold;
    }
</style>
""", unsafe_allow_html=True)

# File upload
st.markdown('<h1 class="main-header">🛒 E-commerce Analytics Dashboard</h1>', unsafe_allow_html=True)

uploaded_file = st.file_uploader("Upload your Excel file", type=['xlsx', 'xls'])

if uploaded_file is not None:
    try:
        # Read all sheets
        excel_data = pd.ExcelFile(uploaded_file)
        sheet_names = excel_data.sheet_names
        
        st.sidebar.header("📊 Available Data Sheets")
        st.sidebar.write(f"Found {len(sheet_names)} sheets:")
        for i, sheet in enumerate(sheet_names, 1):
            st.sidebar.write(f"{i}. {sheet}")
        
        # Load all sheets into dictionary
        data_sheets = {}
        for sheet in sheet_names:
            try:
                data_sheets[sheet] = pd.read_excel(uploaded_file, sheet_name=sheet)
                st.sidebar.success(f"✅ {sheet}")
            except Exception as e:
                st.sidebar.error(f"❌ {sheet}: {str(e)}")
        
        # Main dashboard sections
        tab1, tab2, tab3, tab4, tab5 = st.tabs([
            "📈 Performance Overview", 
            "🚚 Delivery Analytics", 
            "⚠️ Risk Assessment", 
            "📝 Reviews & Complaints",
            "📊 Detailed Data"
        ])
        
        with tab1:
            st.markdown('<div class="section-header">Performance Overview</div>', unsafe_allow_html=True)
            
            col1, col2, col3, col4 = st.columns(4)
            
            # KPI Metrics
            with col1:
                if 'Fastest sellers' in data_sheets:
                    fastest_avg = data_sheets['Fastest sellers']['Avg Delivery Delay (Days)'].mean()
                    st.metric("🚀 Fastest Avg Delivery", f"{fastest_avg:.1f} days")
                
            with col2:
                if 'Slowest sellers' in data_sheets:
                    slowest_avg = data_sheets['Slowest sellers']['Avg Delivery Delay (Days)'].mean()
                    st.metric("🐌 Slowest Avg Delivery", f"{slowest_avg:.1f} days")
                
            with col3:
                if 'Top Complaints' in data_sheets:
                    total_complaints = data_sheets['Top Complaints']['Complaint Count'].sum()
                    st.metric("📞 Total Complaints", f"{total_complaints:,}")
                
            with col4:
                if 'Risk Data' in data_sheets:
                    total_orders = data_sheets['Risk Data']['total_orders'].sum()
                    st.metric("📦 Total Orders", f"{total_orders:,}")
            
            # Charts row 1
            col1, col2 = st.columns(2)
            
            with col1:
                if 'Top Complaints' in data_sheets:
                    st.subheader("📊 Top Complaints by Category")
                    complaints_df = data_sheets['Top Complaints'].sort_values('Complaint Count', ascending=False)
                    fig_complaints = px.bar(
                        complaints_df.head(10), 
                        x='Complaint Count', 
                        y='Product Category',
                        orientation='h',
                        title="Top 10 Complaint Categories",
                        color='Complaint Count',
                        color_continuous_scale='reds'
                    )
                    fig_complaints.update_layout(yaxis={'categoryorder':'total ascending'})
                    st.plotly_chart(fig_complaints, use_container_width=True)
            
            with col2:
                if 'Region delivery delays' in data_sheets:
                    st.subheader("🌍 Delivery Delays by Region")
                    region_df = data_sheets['Region delivery delays'].sort_values('Avg Delivery Delay (Days)', ascending=False)
                    fig_region = px.bar(
                        region_df,
                        x='Customer Region',
                        y='Avg Delivery Delay (Days)',
                        title="Average Delivery Delay by Region",
                        color='Avg Delivery Delay (Days)',
                        color_continuous_scale='viridis'
                    )
                    fig_region.update_xaxis(tickangle=45)
                    st.plotly_chart(fig_region, use_container_width=True)
        
        with tab2:
            st.markdown('<div class="section-header">Delivery Analytics</div>', unsafe_allow_html=True)
            
            col1, col2 = st.columns(2)
            
            with col1:
                if 'Fastest sellers' in data_sheets and 'Slowest sellers' in data_sheets:
                    st.subheader("⚡ Fastest vs Slowest Sellers")
                    
                    # Combine fastest and slowest
                    fastest = data_sheets['Fastest sellers'].copy()
                    fastest['Type'] = 'Fastest'
                    slowest = data_sheets['Slowest sellers'].copy()
                    slowest['Type'] = 'Slowest'
                    
                    combined = pd.concat([fastest.head(10), slowest.head(10)])
                    
                    fig_speed = px.scatter(
                        combined,
                        x='Seller ID',
                        y='Avg Delivery Delay (Days)',
                        color='Type',
                        title="Seller Delivery Performance Comparison",
                        color_discrete_map={'Fastest': 'green', 'Slowest': 'red'}
                    )
                    fig_speed.update_xaxis(tickangle=45)
                    st.plotly_chart(fig_speed, use_container_width=True)
            
            with col2:
                if 'Delivery by sellers by days' in data_sheets:
                    st.subheader("📈 Delivery Delay vs Complaints")
                    delivery_df = data_sheets['Delivery by sellers by days']
                    
                    fig_scatter = px.scatter(
                        delivery_df,
                        x='avg_delay',
                        y='complaints',
                        hover_data=['seller_id'],
                        title="Correlation: Delivery Delay vs Complaints",
                        trendline="ols"
                    )
                    fig_scatter.update_layout(
                        xaxis_title="Average Delay (Days)",
                        yaxis_title="Number of Complaints"
                    )
                    st.plotly_chart(fig_scatter, use_container_width=True)
            
            # Delivery performance distribution
            if 'Delivery by sellers by days' in data_sheets:
                st.subheader("📊 Delivery Performance Distribution")
                delivery_df = data_sheets['Delivery by sellers by days']
                
                fig_hist = px.histogram(
                    delivery_df,
                    x='avg_delay',
                    nbins=20,
                    title="Distribution of Average Delivery Delays",
                    marginal="box"
                )
                fig_hist.update_layout(
                    xaxis_title="Average Delay (Days)",
                    yaxis_title="Number of Sellers"
                )
                st.plotly_chart(fig_hist, use_container_width=True)
        
        with tab3:
            st.markdown('<div class="section-header">Risk Assessment</div>', unsafe_allow_html=True)
            
            col1, col2 = st.columns(2)
            
            with col1:
                if 'Risk Data' in data_sheets:
                    st.subheader("⚠️ Return Risk Analysis")
                    risk_df = data_sheets['Risk Data']
                    
                    # Calculate return rate
                    risk_df['return_rate'] = (risk_df['actual_returns'] / risk_df['total_orders']) * 100
                    
                    fig_risk = px.scatter(
                        risk_df,
                        x='total_orders',
                        y='return_rate',
                        size='avg_predicted_return_risk',
                        color='suspend_seller',
                        hover_data=['seller_id'],
                        title="Return Risk vs Order Volume",
                        color_discrete_map={True: 'red', False: 'green'}
                    )
                    fig_risk.update_layout(
                        xaxis_title="Total Orders",
                        yaxis_title="Return Rate (%)"
                    )
                    st.plotly_chart(fig_risk, use_container_width=True)
            
            with col2:
                if 'Top 10 risk category' in data_sheets:
                    st.subheader("📊 Risk by Product Category")
                    risk_cat_df = data_sheets['Top 10 risk category']
                    
                    fig_risk_cat = px.bar(
                        risk_cat_df,
                        x='product_category',
                        y='Predicted Return Rate',
                        title="Predicted Return Rate by Category",
                        color='Predicted Return Rate',
                        color_continuous_scale='reds'
                    )
                    fig_risk_cat.update_xaxis(tickangle=45)
                    st.plotly_chart(fig_risk_cat, use_container_width=True)
            
            # Sellers to suspend
            if 'Sellers to suspend' in data_sheets:
                st.subheader("🚨 High-Risk Sellers Requiring Action")
                suspend_df = data_sheets['Sellers to suspend'].sort_values('Predicted Return Probability', ascending=False)
                
                # Color code recommendations
                def color_recommendation(val):
                    if val == 'Suspend':
                        return 'background-color: #ff4444; color: white; font-weight: bold;'
                    elif val == 'Monitor':
                        return 'background-color: #ffaa00; color: white; font-weight: bold;'
                    else:
                        return 'background-color: #00aa00; color: white; font-weight: bold;'
                
                styled_df = suspend_df.style.applymap(color_recommendation, subset=['Recommendation'])
                st.dataframe(styled_df, use_container_width=True)
            
            # Next month predictions
            if 'Return risk next month' in data_sheets:
                st.subheader("🔮 Next Month Risk Predictions")
                next_month_df = data_sheets['Return risk next month']
                
                risk_counts = next_month_df['Risk Status for Next Month'].value_counts()
                fig_pie = px.pie(
                    values=risk_counts.values,
                    names=risk_counts.index,
                    title="Risk Status Distribution for Next Month",
                    color_discrete_map={
                        'High Risk': '#ff4444',
                        'Medium Risk': '#ffaa00',
                        'Low Risk': '#00aa00'
                    }
                )
                st.plotly_chart(fig_pie, use_container_width=True)
        
        with tab4:
            st.markdown('<div class="section-header">Reviews & Complaints Analysis</div>', unsafe_allow_html=True)
            
            col1, col2 = st.columns(2)
            
            with col1:
                if 'Negative Reviews' in data_sheets:
                    st.subheader("👎 Negative Reviews by Seller")
                    neg_reviews_df = data_sheets['Negative Reviews'].sort_values('Negative Review Count', ascending=False)
                    
                    fig_neg = px.bar(
                        neg_reviews_df.head(15),
                        x='Seller ID',
                        y='Negative Review Count',
                        title="Top 15 Sellers with Most Negative Reviews",
                        color='Negative Review Count',
                        color_continuous_scale='reds'
                    )
                    fig_neg.update_xaxis(tickangle=45)
                    st.plotly_chart(fig_neg, use_container_width=True)
            
            with col2:
                if 'Repetative Review' in data_sheets:
                    st.subheader("🔄 Repetitive Reviews by Seller")
                    rep_reviews_df = data_sheets['Repetative Review'].sort_values('Repeat Review Count', ascending=False)
                    
                    fig_rep = px.bar(
                        rep_reviews_df.head(15),
                        x='Seller ID',
                        y='Repeat Review Count',
                        title="Top 15 Sellers with Most Repetitive Reviews",
                        color='Repeat Review Count',
                        color_continuous_scale='oranges'
                    )
                    fig_rep.update_xaxis(tickangle=45)
                    st.plotly_chart(fig_rep, use_container_width=True)
            
            # Combined review analysis
            if 'Negative Reviews' in data_sheets and 'Repetative Review' in data_sheets:
                st.subheader("📊 Combined Review Analysis")
                
                # Merge negative and repetitive reviews
                neg_df = data_sheets['Negative Reviews'].set_index('Seller ID')
                rep_df = data_sheets['Repetative Review'].set_index('Seller ID')
                
                combined_reviews = pd.merge(neg_df, rep_df, left_index=True, right_index=True, how='outer').fillna(0)
                combined_reviews.reset_index(inplace=True)
                
                fig_combined = px.scatter(
                    combined_reviews,
                    x='Negative Review Count',
                    y='Repeat Review Count',
                    hover_data=['Seller ID'],
                    title="Negative Reviews vs Repetitive Reviews",
                    trendline="ols"
                )
                st.plotly_chart(fig_combined, use_container_width=True)
        
        with tab5:
            st.markdown('<div class="section-header">Detailed Data Explorer</div>', unsafe_allow_html=True)
            
            # Sheet selector
            selected_sheet = st.selectbox("Select a data sheet to explore:", sheet_names)
            
            if selected_sheet in data_sheets:
                df = data_sheets[selected_sheet]
                
                # Basic info
                col1, col2, col3 = st.columns(3)
                with col1:
                    st.metric("Total Rows", len(df))
                with col2:
                    st.metric("Total Columns", len(df.columns))
                with col3:
                    st.metric("Memory Usage", f"{df.memory_usage(deep=True).sum() / 1024:.1f} KB")
                
                # Data preview
                st.subheader(f"Data Preview: {selected_sheet}")
                st.dataframe(df.head(20), use_container_width=True)
                
                # Summary statistics
                st.subheader("Summary Statistics")
                st.dataframe(df.describe(), use_container_width=True)
                
                # Download option
                csv = df.to_csv(index=False)
                st.download_button(
                    label=f"Download {selected_sheet} as CSV",
                    data=csv,
                    file_name=f"{selected_sheet.replace(' ', '_')}.csv",
                    mime="text/csv"
                )
        
        # Footer
        st.markdown("---")
        st.markdown("""
        <div style='text-align: center; color: #666; padding: 20px;'>
            <p>🛒 E-commerce Analytics Dashboard • Built with Streamlit</p>
            <p>Upload your Excel file to get started with comprehensive analytics</p>
        </div>
        """, unsafe_allow_html=True)
        
    except Exception as e:
        st.error(f"Error processing the Excel file: {str(e)}")
        st.info("Please ensure your Excel file contains the expected sheets with the correct column names.")

else:
    st.info("👆 Please upload your Excel file to begin the analysis")
    
    # Show expected format
    st.markdown("### 📋 Expected Data Format")
    st.markdown("""
    Your Excel file should contain the following sheets:
    
    1. **Fastest sellers**: Seller ID, Avg Delivery Delay (Days)
    2. **Slowest sellers**: Seller ID, Avg Delivery Delay (Days)  
    3. **Top Complaints**: Product Category, Complaint Count
    4. **Region delivery delays**: Customer Region, Avg Delivery Delay (Days)
    5. **Repetative Review**: Seller ID, Repeat Review Count
    6. **Negative Reviews**: Seller ID, Negative Review Count
    7. **Risk Data**: seller_id, avg_predicted_return_risk, total_orders, actual_returns, suspend_seller
    8. **Return risk next month**: seller_id, Predicted Return Probability, Total Orders in Test Set, Actual Returns in Test Set, Risk Status for Next Month
    9. **Top 10 risk category**: product_category, Predicted Return Rate, Total Items in Category, Predicted Returns Count, Actual Returns Count, Avg Predicted Probability, Recommendation
    10. **Sellers to suspend**: seller_id, Predicted Return Probability, Total Orders, Actual Returns, Recommendation
    11. **Delivery by sellers by days**: seller_id, avg_delay, complaints
    """)
