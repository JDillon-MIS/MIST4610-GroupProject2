# Group B1 MIST4610 Group Project #2

## Group Name: 
Section 47114 Group B1

## Group Members & Their Roles:
1. Jacob Dillon [@JDillon](https://www.github.com/JDillon-MIS) - Group Leader
2. Jenjen Lin [@JLin](https://github.com/J2Lin) - SQL Writer
3. Graham Nash [@GNash](https://github.com/grahamenash) - Conceptual Modeler
4. Ved Cholleti [VCholleti](NoAccountProvided) - Database Designer/Data Wrangler

## Case Summary:

We're working with a small online-retail company called Northline Outfitters which sells a variety of lifestyle and tech accessories to its customers in both the United States and Canada. It currently purchases merchandise from outside vendors and sells directly to consumers.

Because the business is still growing, its records have been kept in two Excel spreadsheets instead of a relational database. The data contained in both of these spreadsheets ("Sales_Dump" and "Product_Supplier_Master") are dirty, not normalized, and contain a variety of inconsistencies, which we have documented and adressed below.

## Conceptual Model:
<img width="1065" height="920" alt="dm2" src="https://github.com/user-attachments/assets/c887bebc-5038-4a0b-b2c7-b1aeeb1e2112" />


#### Explanation of Entities & Relationships:

<img width="2567" height="1022" alt="image" src="https://github.com/user-attachments/assets/48939258-1d72-4b21-b01d-df1ecf82fe74" />

Relationships:
<img width="907" height="460" alt="image" src="https://github.com/user-attachments/assets/b2a4e5ac-b757-4c79-8e3e-23b98e7e209d" />


## Data Quality Assessment:

Both of the original spreadsheets (Sales_Dump & Product_Supplier_Master) contain several different data quality issues:

### Issues in Sales_Dump:
1. Missing values
Several important fields are blank in many rows, especially:

customer_email
discount
tax
line_total
size_or_weight
return_flag

2. Inconsistent formatting
The same type of information is entered in different ways:

Dates use different formats
Payment methods appear as VISA, visa, Debit, MC, Mastercard
Country values appear as US, USA, CA, Canada
Phone numbers are written in different styles

3. Numbers stored as text
Some columns that should contain only numbers include words or symbols:

Prices like USD 18.99
Discounts like 10% or promo5
Tax values like 13%, 0.13, HST 13%
Quantity values like 2 units

4. Unreliable sales totals
Many line_total values are missing, and some totals do not match the quantity, unit price, discount, and tax.

5. Mixed information in one field
Some columns contain more than one type of information:

customer_info mixes customer details together
notes contains extra comments in free text

6. Inconsistent units
Size and weight are recorded in different ways, such as:

11"
11 inches
11 in
### Issues in Product_Supplier_Master:
1. Missing values
Many rows are missing important product or supplier details, including:

reorder_level
pack_size
weight
length
discontinued

2. Duplicate and inconsistent product IDs
Some SKUs appear more than once, often with only capitalization differences, such as:

SKU-C-1002
sku-c-1002

3. Inconsistent product details
The same SKU may be linked to different descriptions or categories.
For example, one version may describe a product one way, while another uses a slightly different name or category.

4. Inconsistent category names
Categories are written in different ways, such as:

Tech / Student
Tech & Student
Desk Setup / Student

5. Mixed information in one field
Some text fields include extra comments instead of just clean master data.
For example, vendor_rep may include notes like “email missing.”

6. Inconsistent units of measure
Weight and size values are not standardized, such as:

272 grams vs 272g
0.22 kilograms vs 0.22kg

## Data Cleaning Process:

*Explain how you resolved the data quality issues identified previously. Include any SQL statements used to standardize, split, convert, or update the imported data*


### Required & Additional Queries:

*Provide answers for the 3 required queries and the 3 additional queries designed by your group*
