# How can we improve customer retention and lifetime value?

## Modelling & Evaluation

Improving customer retention and lifetime value (CLTV) is crucial for the long-term success of any eCommerce business. Retaining existing customers is generally more cost-effective than acquiring new ones, as it eliminates the need for extensive marketing efforts and reduces overall acquisition costs. It is estimated that customer acquisition can be 5 times more expensive than customer retention<sup>1</sup>.   

High retention rates foster customer loyalty, leading to increased repeat purchases and positive word-of-mouth referrals, which can significantly boost revenue. Additionally, by focusing on CLTV, businesses can better understand the long-term profitability of their customer base, enabling more strategic resource allocation and targeted marketing efforts. Ultimately, enhancing customer retention and CLTV not only drives immediate sales growth but also contributes to sustainable business development in an increasingly competitive marketplace.

### Feature Engineering
The data used to conduct this analysis involved combining the online_sales and products table from the database. A total_spent column was created to get the overall cost of a specific transaction.

Formula: If a coupon was used, total_spent = actual_price x (1 - discount_percentage) x quantity. Otherwise, total_spent = actual price x quantity

The dataset contains transaction data spanning from January 1, 2019, to December 31, 2019. During this period, a coupon retention strategy was implemented, offering customers discount coupons on various products. 

### Synthetic Data Generation
Due to data constraints, we lack information for the period before the coupon retention strategy was implemented. To facilitate comparison, we will generate synthetic data for 2018 to simulate user transactions without this strategy.

Transaction IDs and Dates: We generated unique transaction IDs for each synthetic entry. Each transaction was assigned a random date within 2018, ensuring an even distribution across the year.
```python
# Generate synthetic transaction IDs (unique)
transaction_ids = np.arange(1, n_transactions + 1)

# Generate random dates in 2018
start_date = pd.to_datetime("2018-01-01")
end_date = pd.to_datetime("2018-12-31")
transaction_dates = start_date + (end_date - start_date) * np.random.rand(n_transactions)
```

Product Details: Using product_id data from 2019, we sampled synthetic product_ids to approximate realistic product distributions. Quantities were randomly generated, capped at the 90th percentile of 2019’s data to avoid unusually high values.
```python
# Generate synthetic product_ids sampled from the 2019 data
product_ids = np.random.choice(df_2019['product_id'].unique(), size=n_transactions)

# Generate synthetic quantities
quantities = np.random.randint(1, np.percentile(df_2019['quantity'], 90) + 1, size=n_transactions)
```

Joining Product Information: The 2018 data was merged with product details from a products_df table to include information on product_name, category, and actual_price.
```python
# Left join with the products table
df_2018 = pd.merge(df_2018, products_df, on='product_id', how='left')
```

Coupon Columns: To reflect the absence of a coupon strategy, we added coupon_code, coupon_status, and discount_percentage columns, all set to null or zero.
```python
# Add empty columns to simulate no coupon strategy is used
df_2018.insert(loc = df_2018.columns.get_loc('category') + 1, column='coupon_code', value=np.nan)
df_2018.insert(loc = df_2018.columns.get_loc('coupon_code') + 1, column='coupon_status', value=np.nan)
df_2018.insert(loc = df_2018.columns.get_loc('coupon_status') + 1, column='discount_percentage', value=0)
```

User IDs by Quarter: To maintain realistic user behaviour, we assigned synthetic user_ids based on actual user patterns from 2019, using a dictionary of unique users per quarter in 2019.
```python
# Generate synthetic user_ids based on 2019 data
# Create a dictionary with unique user_ids for each quarter in 2019
unique_users_per_quarter_2019 = df_2019.groupby('quarter')['user_id'].unique().to_dict()

# Create a function to assign similar user ids which appear in each quarter in 2019 to each quarter in 2018.
def assign_user_ids(row):
   quarter = row['quarter']
   unique_users = unique_users_per_quarter_2019.get(quarter, [])
   return np.random.choice(unique_users, size=1)[0]

# Assign user ids
df_2018.insert(loc = 0, column = 'user_id', value=df_2018.apply(assign_user_ids, axis=1))
```

Simulating a Lower Retention Rate: To mimic a lack of retention strategy, we removed 20% of rows from the synthetic dataset, which simulates a poorer performance of the total number of transactions.
```python
# Randomly remove 20% of rows to simulate poorer performance due to no retention strategy
df_2018= df_2018.sample(frac=0.8, random_state=42)
df_2018 = df_2018.reset_index(drop=True)
```

### Calculate customer churn rates and identify at risk customers
To calculate churn rate, we will need to define a time period to determine whether we will consider that customer to be churned. In our analysis, we will be defining churn quarterly. For example, a user that makes a transaction in the previous quarter but not in the next quarter will be marked as churned.
```python
# Group unique users into the respective year and quarter
customers_per_quarter = df.groupby('year_quarter')['user_id'].unique().reset_index()

# Define a function to calculate churn rate

def calculate_churn_rate(customers_per_quarter):
   churn_rates = []
   for i in range(1, len(customers_per_quarter)):
       current_customers = set(customers_per_quarter['user_id'].iloc[i])
       previous_customers = set(customers_per_quarter['user_id'].iloc[i - 1])
      
       #Taking a set difference to identify customers present in the previous     quarter but not in the current quarter
       churned_customers = previous_customers - current_customers
      
       # Churn rate: percentage of previous customers who churned
       churn_rate = len(churned_customers) / len(previous_customers)
      
       churn_rates.append({
           'year_quarter': customers_per_quarter['year_quarter'].iloc[i],
           'churn_rate': churn_rate
       })
  
   return pd.DataFrame(churn_rates)

#Calculate churn rate per quarter
churn_rate_df = calculate_churn_rate(customers_per_quarter)
```

Similarly, we need to define an at risk customer. Based on the Recency, Frequency and Monetary (RFM) segmentation from the previous section, an at risk customer will be defined as one who falls into the ‘Low’ segment.
```python
# User ID's of the at risk customers
user_risk = rfm[rfm['Segment'] == 'Low']['user_id']
user_risk = user_risk.tolist()
```

### Calculate Customer Lifetime Value (CLTV)
Customer Lifetime Value (CLTV) is a key metric that helps predict the total revenue a customer is expected to generate over their relationship with a business. It provides valuable insights into customer profitability over time.

The formula we'll use for CLTV is:
CLTV = ((Average Order Value x Purchase Frequency) / Churn Rate) x Profit Margin

To compare CLTV effectively across different time periods, we will filter our data into 2018 and 2019 transactions, allowing us to calculate separate CLTV values for each customer in each year. This will enable a clear comparison of customer lifetime value between 2018 and 2019.

## Analytical Findings

### Analyse the effectiveness of the coupon retention strategy

We will analyse the effectiveness of the coupon strategy by comparing the total number of transactions, total revenue, improvements in customer lifetime value as well as churn rates between 2018 and 2019.

Line plot of churn rates 2018 - 2019:

<img width="1318" alt="Screenshot 2024-11-12 at 10 08 43 AM" src="https://github.com/user-attachments/assets/a2b9c083-cf71-4185-ba9e-d0e31630bf6b">

From the line graph, churn rate is relatively similar over the quarters from 2018 to 2019, with the churn rate peaking during 2019 Quarter 1.

Stacked bar chart for total transactions:

<img width="1309" alt="Screenshot 2024-11-12 at 10 09 10 AM" src="https://github.com/user-attachments/assets/ee1b263b-2254-4788-aa6b-0f111f5cc75c">

From the stacked bar chart, we can see that the number of transactions is higher in 2019, after the retention strategy (Coupon) is applied.

Line plot for total revenue in 2018 and 2019:

<img width="1311" alt="Screenshot 2024-11-12 at 10 09 30 AM" src="https://github.com/user-attachments/assets/031b032d-4895-42f1-a17f-9717f16ff431">

From the line graph, we see that after the coupon retention strategy is used in 2019, total revenue by quarter increases with respect to the total revenue in 2018.

Customer Lifetime Value (CLTV) is calculated for each customer in 2018 and 2019. By taking a difference in the 2019 CLTV with 2018 CLTV for each customer, we are able to determine whether the CLTV for the customer has increased. Overall, 51.43% of customers have an increase in CLTV from 2018 to 2019.

In terms of the total number of transactions and revenue, 2019 performed better than 2018 after the coupon strategy was applied. There is also an increase in CLTV for about 51% of users from 2018 to 2019. 

However, when we look at the churn rate, it is relatively similar over the quarters from 2018 to 2019. This could mean that the coupon strategy was not effective in customer retention. Instead, it might have helped encourage customers to make more purchases and in higher quantities, leading to a higher total number of transactions, revenue and customer lifetime value. The coupon strategy seems to be effective in encouraging customers to spend more, but does not have a significant impact on customer retention.

## Recommendations
The coupon strategy has shown promise in boosting annual revenue; however, its impact on customer retention remains limited. To fully capitalize on the revenue gains brought by the coupon strategy, we recommend integrating additional retention strategies that can simultaneously drive revenue and improve customer loyalty. Suggested strategies include:
- Tiered Loyalty Programs: Introducing a tiered loyalty system can encourage repeat purchases by offering progressively valuable rewards as customers advance through the tiers. Higher-tier customers can be granted access to exclusive perks, such as personalized coupons or early access to new products, enhancing their sense of exclusivity and fostering long-term engagement.
- Birthday Deals and Exclusive Vouchers for Inactive Customers: Offering personalized deals for customers’ birthdays, such as discounts, free gifts, or loyalty points, can make them feel valued and appreciated, fostering a stronger connection with the brand. Additionally, providing exclusive vouchers to inactive customers can be an effective strategy to re-engage them. These vouchers, sent after a defined period of inactivity, serve as a gentle reminder of the brand and encourage customers to return and make a purchase, thus improving retention and reducing churn.

### Sample implementation of the tiered loyalty program:
Quarter 1: Planning and Design
Set clear goals and KPIs for revenue growth and churn reduction, and define customer segments to create meaningful tiers with distinct benefits. Determine rewards for each level, focusing on perks like exclusive coupons and birthday offers. Select a loyalty program platform that integrates well with existing systems, ensuring both technical and team readiness.

Quarter 2: Development and Initial Testing
Build the loyalty infrastructure, set up a points system, and launch a pilot program for a small customer segment to gather initial feedback. Train customer service teams on the program details and monitor early KPIs such as engagement and spending trends within the pilot group, making minor adjustments as needed based on customer input.

Quarter 3: Full Rollout and Promotion
Launch the loyalty program to the entire customer base and promote it through targeted campaigns. Track core metrics like churn, purchase frequency, and tier migration while conducting periodic customer feedback surveys to understand user satisfaction. Adjust the program in response to engagement trends and specific behavioral insights.

Quarter 4: Analysis and Optimization
Analyze the year’s data to measure the program’s impact on revenue and retention, refining tier benefits and re-engagement offers for inactive customers as needed. Prepare a final report detailing key findings and ROI, with recommendations for program scaling or adjustments based on performance. Plan a roadmap for potential expansion or further integration to support long-term retention.

### Recommendation impacts
Implementing a tiered loyalty program is expected to enhance customer engagement and retention by rewarding long-term commitment, which can ultimately drive revenue growth. The program encourages repeat purchases as customers work towards progressively valuable rewards and exclusive benefits, such as coupons and birthday deals, fostering a sense of exclusivity. By promoting customer loyalty and reducing churn, the program can generate a more consistent revenue stream and increase lifetime customer value, offering both immediate financial gains and lasting customer relationships.

## Future Work
For more effective insights in the future, we recommend that the data collection process is expanded so that it can include user activity both before and after introducing retention strategies. This will enable a comparative analysis to gauge its impact on both new and returning customers. 
Furthermore, collecting multi-year data before and after the retention strategy implementation would enable a more robust analysis of customer behaviour trends, allowing us to accurately measure changes in engagement and churn rates. With a more comprehensive dataset, we could also examine how different customer segments respond to retention strategies, helping to refine approaches for both new and existing users.

## References
1. Gallo, A. (2014, October 29). *The value of keeping the right customers*. Harvard Business Review. https://hbr.org/2014/10/the-value-of-keeping-the-right-customers






