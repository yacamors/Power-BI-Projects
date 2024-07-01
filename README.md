The relational model development process began with importing data from a file, available at the following link: From this data, individual tables were created and structured without the need for bridge tables in this initial stage.

The construction of the relational model started with importing data, which were organized into three main tables, each with its respective columns:

Invoice: Contains information related to invoices, including Branch, City, COGS, Customer Type, Date, Gender, Gross Income, and Gross Margin Percentage columns.
Customer: Stores customer data, with columns such as Customer Type, Gender, Invoice ID, Payment, and Rating.
Product: Focuses on sold products, with columns Invoice ID and Product Line. These tables were related in a one-to-one structure, establishing relationships reflecting data connections according to business context.
For creating calculated measures, a new table named "Measures" was added. Specific formulas were implemented within this table to calculate key metrics. These measures were applied to the data in the previously created tables, enabling more detailed and relevant analysis for decision-making.

Rating % by City: Calculates the average rating for specific cities: Boston, New York, and Washington.


Rating % by City = CALCULATE( AVERAGE('Invoice'[Rating]), FILTER( 'Invoice', 'Invoice'[City] IN {"Boston", "New York", "Washington"} ) )
Count of Customer Type rows in Invoice table:


Customer Types = COUNT(Invoice[Customer Type])
Count of Invoice ID rows in Customer table:


Customer Count = COUNT(Customer[Invoice ID])
Count of gross income rows in Invoice table:


Income Count = COUNT(Invoice[gross income])
Count of rows in Customer Type column of Invoice table:


Member Count = COUNT(Invoice[Customer Type])
Count of male and female customers:


Male Count = CALCULATE([Customer Count], Customer[Gender]="Male")
Female Count = CALCULATE([Customer Count], Customer[Gender]="Female")
Count of normal and member customers:


Member Types Count = CALCULATE([Customer Types], Invoice[Customer Type]="Member")
Normal Types Count = CALCULATE([Customer Types], Invoice[Customer Type]="Normal")
Count of transactions in Invoice table for cities:


Boston Transactions Count = COUNTROWS(FILTER(Invoice, Invoice[City] = "Boston"))
New York Transactions Count = COUNTROWS(FILTER(Invoice, Invoice[City] = "New York"))
Washington Transactions Count = COUNTROWS(FILTER(Invoice, Invoice[City] = "Washington"))
Total quantity of products sold:


Total Products Sold = SUM('Invoice'[Quantity])
Total quantity of products sold by Product Line:


Products Sold by Product Line = CALCULATE( SUM('Invoice'[Quantity]), ALLEXCEPT('Invoice', 'Invoice'[Product Line]) )
Count of transactions for payment methods: Cash, Credit card, and Ewallet:


Cash Transactions = COUNTROWS(FILTER(Invoice, Invoice[Payment] = "Cash"))
Credit Card Transactions = COUNTROWS(FILTER(Invoice, Invoice[Payment] = "Credit card"))
Ewallet Transactions = COUNTROWS(FILTER(Invoice, Invoice[Payment] = "Ewallet"))
Count of transactions for each Product Line:


Electronic Accessories Transactions = COUNTROWS(FILTER(Product, Product[Product Line] = "Electronic accessories"))
Fashion Accessories Transactions = COUNTROWS(FILTER(Product, Product[Product Line] = "Fashion accessories"))
Sports and Travel Transactions = COUNTROWS(FILTER(Product, Product[Product Line] = "Sports and travel"))
Food and Beverages Transactions = COUNTROWS(FILTER(Product, Product[Product Line] = "Food and beverages"))
Health and Beauty Transactions = COUNTROWS(FILTER(Product, Product[Product Line] = "Health and beauty"))
Home and Lifestyle Transactions = COUNTROWS(FILTER(Product, Product[Product Line] = "Home and lifestyle"))
Average Rating:


Average Rating = AVERAGE('Invoice'[Rating])
Average Gross Income by Payment method:


Average Gross Income by Payment = CALCULATE( AVERAGE('Invoice'[gross income]), ALLEXCEPT('Invoice', 'Invoice'[Payment]) )
Average Quantity Sold by Payment method:


Average Quantity Sold by Payment = CALCULATE( AVERAGE('Invoice'[Quantity]), ALLEXCEPT('Invoice', 'Invoice'[Payment]) )
Total gross income by City:


Gross Income by City = CALCULATE( SUM('Invoice'[gross income]), ALLEXCEPT('Invoice', 'Invoice'[City]) )
Average rating of products sold by selected Product Lines:


Rating by Product Line = CALCULATE( AVERAGE('Invoice'[Rating]), FILTER( 'Invoice', 'Invoice'[Product Line] IN {"Electronic accessories", "Fashion accessories", "Health and beauty", "Home and lifestyle", "Food and beverages", "Sports and travel"} ) )
Average rating of purchases by males:


Average Rating by Males = CALCULATE( AVERAGE(Invoice[Rating]), Invoice[Gender] = "Male" )
Average rating of purchases by females:


Average Rating by Females = CALCULATE( AVERAGE(Invoice[Rating]), Invoice[Gender] = "Female" )
Calendar Table:
Before creating the table, we determined the minimum and maximum dates present in the dataset. Then we created a new table using the following DAX formula:


Calendar Table = CALENDAR(MIN(Invoice[Date]), MAX(Invoice[Date]))
Generating Dates in Specified Range: Using available functions or tools, you can generate a set of dates spanning from the minimum date to the maximum date. This can be done by generating a sequence of dates or by specifying a time interval (daily, weekly, monthly, etc.). Once the calendar table is created, it is related to the Invoice ID column in the Invoice table for data integration.


