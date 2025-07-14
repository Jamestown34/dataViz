🛒 E-commerce Analytics Dashboard
A comprehensive Streamlit dashboard for analyzing e-commerce seller performance, delivery metrics, risk assessment, and customer complaints.
📊 Features
📈 Performance Overview

Key performance indicators (KPIs)
Top complaints by product category
Regional delivery delay analysis

🚚 Delivery Analytics

Fastest vs slowest sellers comparison
Delivery delay vs complaints correlation
Performance distribution analysis

⚠️ Risk Assessment

Return risk analysis and predictions
Product category risk evaluation
Seller suspension recommendations
Next month risk forecasting

📝 Reviews & Complaints

Negative reviews analysis
Repetitive reviews detection
Combined review correlation insights

📊 Data Explorer

Interactive data sheet explorer
Summary statistics
Data export capabilities

🚀 Quick Start
1. Clone the Repository
bashgit clone https://github.com/yourusername/ecommerce-analytics-dashboard.git
cd ecommerce-analytics-dashboard
2. Install Dependencies
bashpip install -r requirements.txt
3. Add Your Dataset
Place your Excel file in the data/ folder and name it ecommerce_data.xlsx
4. Run the Dashboard
bashstreamlit run app.py
📁 Project Structure
ecommerce-analytics-dashboard/
├── app.py                    # Main Streamlit application
├── requirements.txt          # Python dependencies
├── README.md                # This file
├── data/                    # Data folder
│   └── ecommerce_data.xlsx  # Your Excel dataset
└── assets/                  # Screenshots and images
    └── dashboard_preview.png
📊 Expected Data Format
Your Excel file should contain the following sheets:

Fastest sellers: Seller ID, Avg Delivery Delay (Days)
Slowest sellers: Seller ID, Avg Delivery Delay (Days)
Top Complaints: Product Category, Complaint Count
Region delivery delays: Customer Region, Avg Delivery Delay (Days)
Repetative Review: Seller ID, Repeat Review Count
Negative Reviews: Seller ID, Negative Review Count
Risk Data: seller_id, avg_predicted_return_risk, total_orders, actual_returns, suspend_seller
Return risk next month: seller_id, Predicted Return Probability, Total Orders in Test Set, Actual Returns in Test Set, Risk Status for Next Month
Top 10 risk category: product_category, Predicted Return Rate, Total Items in Category, Predicted Returns Count, Actual Returns Count, Avg Predicted Probability, Recommendation
Sellers to suspend: seller_id, Predicted Return Probability, Total Orders, Actual Returns, Recommendation
Delivery by sellers by days: seller_id, avg_delay, complaints

🛠️ Technologies Used

Streamlit: Web application framework
Pandas: Data manipulation and analysis
Plotly: Interactive visualizations
NumPy: Numerical computing
OpenPyXL: Excel file handling

🎯 Key Insights Provided

Delivery Performance: Identify fastest and slowest sellers
Risk Management: Predict return risks and seller suspension needs
Customer Satisfaction: Analyze complaints and negative reviews
Regional Analysis: Understand delivery delays by geographic region
Product Category Insights: Risk assessment by product type

🔧 Customization
The dashboard is designed to be easily customizable:

Add New Visualizations: Modify the plotting functions in app.py
Change Color Schemes: Update the Plotly color configurations
Add New Metrics: Include additional KPIs in the metrics section
Modify Layout: Adjust the Streamlit layout and styling

📱 Live Demo
Visit the live dashboard: [Your Streamlit Cloud URL]
🤝 Contributing

Fork the repository
Create a feature branch (git checkout -b feature/new-feature)
Commit your changes (git commit -am 'Add new feature')
Push to the branch (git push origin feature/new-feature)
Create a Pull Request

📄 License
This project is licensed under the MIT License - see the LICENSE file for details.
📞 Support
For questions or support, please open an issue in the GitHub repository.

Built with ❤️ using Streamlit and Plotly
