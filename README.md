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


### Explanation of Entities & Relationships:
**Entities:**
<img width="2567" height="1022" alt="image" src="https://github.com/user-attachments/assets/48939258-1d72-4b21-b01d-df1ecf82fe74" />

**Relationships:**
<img width="907" height="460" alt="image" src="https://github.com/user-attachments/assets/b2a4e5ac-b757-4c79-8e3e-23b98e7e209d" />


## Data Quality Assessment:

**Both of the original spreadsheets (which are named Sales_Dump and Product_Supplier_Master) contain several data quality issues:**

### Issues in Sales_Dump:
**Missing values:** <br/>
Several important fields are blank in many rows (i.e. customer_email, discount, tax, line_total, size_or_weight, and return_flag)

**Inconsistent formatting:** <br/>
The same type of information is frequently entered in different ways: Some examples are dates using different formats, Payment methods appearing as VISA, visa, Debit, MC, or Mastercard, and Country values appear as US, USA, CA, Canada, and Phone numbers are written in different styles

**Numbers stored as text:** <br/>
Some columns that should contain only numbers include words or symbols. For example, there are Prices like USD 18.99, Discounts like 10% or promo5, Tax values like 13%, 0.13, or HST 13%, and Quantity values like "2 units"

**Unreliable sales totals:** <br/>
Many line_total values are missing, and some totals do not match the quantity, unit price, discount, and tax.

**Mixed information in one field:** <br/>
Some columns contain more than one type of information: For example, customer_info mixes customer details together, and notes contains extra comments in its text

**Inconsistent units:** <br/>
Size and weight are recorded in a number of different ways (such as 11", 11 inches, and 11 in)

### Issues in Product_Supplier_Master:
**Missing values:** <br/>
Many rows are missing important product or supplier details, including reorder_level, pack_size, weight, length, and discontinued

**Duplicate and inconsistent product IDs:** <br/>
Some SKUs appear more than once, often with only differences in capitalization (i.e. SKU-C-1002 versus sku-c-1002)

**Inconsistent product details:** <br/>
The same SKU may be linked to different descriptions or categories. For example, one version may describe a product one way, while another uses a slightly different name or category

**Inconsistent category names:** <br/>
Categories are written in different ways (such as Tech / Student, Tech & Student, or Desk Setup / Student)

**Mixed information in one field:** <br/>
Some text fields include extra comments instead of just clean master data. For example, vendor_rep sometime includes notes like “email missing.”

**Inconsistent units of measure:** <br/>
Weight and size values are not standardized, with some examples being 272 grams vs 272g and 0.22 kilograms vs 0.22kg

## Data Cleaning Process:

****Explain how you resolved the data quality issues identified previously. Include any SQL statements used to standardize, split, convert, or update the imported data****

**SKU casing:** <br/>
SKUs were inconsistently capitalized across both sheets (e.g. sku-c-1002 vs SKU-C-1002). We standardized all SKUs to uppercase so joins between the sales and product tables would work correctly.

**Ship country:** <br/>
Values included US, USA, CA, Canada, and some blanks. We normalized everything to either the US or CA. Blank rows were inferred from the order ID prefix: CORD- orders were assigned CA and UORD- orders were assigned US.

**Sale dates:** <br/>
The sale_date column had at least six different formats mixed together (e.g. 10-11-2025, Oct 17 25, October 5 25, 31/10/2025). We standardized all dates to YYYY-MM-DD format. One record had a date of May 1 2026 which is a future date and probably just a data entry error, so we changed it to "2025-05-01".

**Customer name:** <br/>
The raw customer_info field stored everything in one unstructured text blob using inconsistent delimiters (|, ;, /). To match our model's split name columns, we extracted the full name as everything before the first delimiter, then split it into customer_F_Name and customer_L_Name on the first space.

**Customer type and loyalty:** <br/>
The same customer_info field also mixed checkout type and loyalty status together. Since our model separates these into two columns, we parsed them independently: customer_type was set to Guest or Standard based on whether the word "guest" appeared, and customer_loyalty was set to Loyalty, Student, or NULL based on keywords in the field.

**Discount:** <br/>
The discount column had four different formats: percentage strings (10%), plain integers (5), decimal fractions (0.05), and text codes (promo5, student 10%). We converted everything to a consistent decimal rate: so 10%, 10, and student 10% all become 0.10, and promo5 becomes 0.05.

**Tax:** <br/>
Similar problem as discount: values appeared as 8.25%, 0.0825, HST 13%, and 13. We applied the same logic and converted everything to a decimal rate, stripping any text labels like "HST" before converting.

**Unit price:** <br/>
Some prices had currency labels attached (USD 18.99, CAD 46.99) while others were plain numbers. We stripped the labels and stored the currency separately based on ship_country: US orders get USD, Canadian orders get CAD.

**Quantity:** <br/>
Some rows stored quantity as text like "2 units" instead of just 2. We stripped the non-numeric characters to get a plain integer.

**Missing line totals:** <br/>
Several rows had a blank line_total. For those rows only, we recomputed the value as quantity × unit_price × (1 − discount) × (1 + tax). Rows that already had a value were left untouched.

**Payment method:** <br/>
Values like VISA, visa, MC, Mastercard, and AMEX were all normalized to consistent title-case labels (Visa, Mastercard, Amex, etc.) using a lookup table.

**Return flag:** <br/>
Some rows had a blank return flag instead of Y or N. Blanks were defaulted to N.

**Product duplicates:** <br/>
The Product_Supplier_Master had the same SKU appearing multiple times for product variants (e.g. different sizes or editions of the same item). We kept one canonical row per unique SKU for the product table and moved the remaining rows into product_variant using the parent_sku field to link them back to their parent product.

**Vendor name typo:** <br/>
"Urban Sources" appeared in several rows instead of the correct "Urban Source". Fixed with a simple find and replace.

**Vendor rep names:** <br/>
Some reps had "/ email missing" appended to their name, one had an honorific (Ms. Anika Roy), and one had a typo (Mia Dia zFernandez). We stripped the suffixes, removed the honorific, and corrected the typo.

**Reorder level:** <br/>
One row had the word "ten" instead of the number 10. Converted to an integer.

**Phone numbers:** <br/>
Vendor phone numbers used three different formats (dashes, dots, parentheses). Normalized everything to XXX-XXX-XXXX.

**Categories:** <br/>
Some category values had multiple segments separated by / or ,, and some duplicated the same category twice (e.g. "Accessories / Accessories"). We extracted just the first segment as the canonical category for each product.


## Required & Additional Complex Queries:


**Required Query #1.** "Which products generated the highest total sales revenue, by country?"
<img width="1880" height="860" alt="image" src="https://github.com/user-attachments/assets/b68927bb-7331-46fd-bd15-c7fe27e88d15" />


**Required Query #2.** "Which employees handled the largest number of orders, and how do their results compare with other employees under the same manager?"
<img width="1987" height="1085" alt="image" src="https://github.com/user-attachments/assets/c9d348d7-2117-41a2-bba2-78c9df9119d0" />


**Required Query #3.** "Which vendors supply products that appear in more than one category?"
insert SQL Output


**Additional Query #1.** "What is the trend in month-to-month revenue for the US versus what it is for Canada?"

 insert SQL Script

Business Justification: This helps Northline Outfitters understand whether or not thier revenue is growing and when peak seasons occur. Seperating it by country allows the company to identify whether US and Canadian markets are behaving differently.

**Additional Query #2.** "

insert SQL Script

Business Justification:

**Additional Query #3.** "

insert SQL Script

Business Justification: High return rates in specific categories (e.g. Apparel & Electronics) can indicate problems like sizing issues, misleading descriptions, and poor fit for its target market. Northline Outfitters can use this information to help them decide which categories should be updated with more accurate details, or perhaps dropped entirely if it's a bad fit for its target market.

