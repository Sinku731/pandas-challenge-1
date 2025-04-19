# pandas-challenge-1
Summary:
The analysis of the top clients revealed significant spending patterns. The top five clients, collectively contributed millions in revenue & profit, Client ID 24741 generating the highest profit with approximately $36.58 million which was almost 10x more than the next highest client. Additionally,Total Units purchased along with Shipping cost ended up playing a considerable role in overall expenses, and final profitability.

Key Formulas:
# Format the data and rename the columns to names suitable for presentation.
money_columns = ['total_shipping_price', 'total_revenue', 'total_cost', 'total_profit']

# Define the function that converts a dollar amount to millions.
def currency_format_millions(amount):
    return f"${amount / 1_000_000:,.2f}M" if amount >= 1_000_000 else f"${amount:,.2f}"

# Apply the currency_format_millions function to the money columns.
for col in money_columns:
    summary_df[col] = summary_df[col].apply(currency_format_millions)

# Rename the columns to reflect the change in the money format.
summary_df.rename(columns={
    'client_id': 'Client ID',
    'total_qty': 'Total Units Purchased',
    'total_shipping_price': 'Total Shipping Price (in millions)',
    'total_revenue': 'Total Revenue (in millions)',
    'total_cost': 'Total Cost (in millions)',
    'total_profit': 'Total Profit (in millions)'
}, inplace=True)

# Display the final formatted data.
summary_df[['Client ID', 'Total Units Purchased', 'Total Shipping Price (in millions)', 
            'Total Revenue (in millions)', 'Total Cost (in millions)', 'Total Profit (in millions)']]

# Top 5 client IDs from earlier
top_clients = [33615, 66037, 46820, 38378, 24741]

# Total amount spent (sum of line_price) by each of these clients
top_clients_spend = df[df['client_id'].isin(top_clients)].groupby('client_id')['line_price'].sum().round(2)
top_clients_spend

# Create a column for the cost of each line using unit cost, qty, and shipping price.
df['total_weight'] = df['unit_weight'] * df['qty']
df['shipping_price'] = df['total_weight'].apply(lambda x: 7 * x if x > 50 else 10 * x)
df['line_cost'] = (df['unit_cost'] * df['qty']) + df['shipping_price']

# Display the relevant data.
df[['first', 'last', 'unit_cost', 'qty', 'shipping_price', 'line_cost']].head()
