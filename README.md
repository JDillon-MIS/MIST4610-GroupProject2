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

<img width="986" height="946" alt="image" src="https://github.com/user-attachments/assets/10b36e71-8482-4c31-ab05-42e92017c19c" />

#### Explanation of Entities & Relationships:

<img width="2567" height="1022" alt="image" src="https://github.com/user-attachments/assets/48939258-1d72-4b21-b01d-df1ecf82fe74" />

Relationships:
- A *manager* supervises many *employees*, and each *employee* reports to only one *manager*.
- An *employee* handles many *orders*, and each *order* is handled by only one *employee*.
- A *customer* can place many *orders*, and each *order* belongs to only one *customer*.
- An *order* contains many *order_lines*, and each *order_line* belongs to just one *order*.
- Each *order_line* references just one *product* (or one of its varients), and each *product* can appear in multiple *order_lines*.
- A *product_variant* belongs to just one parent *product*, but a *product* can have many different *product_varients*.
- A *vendor* can supply many *products* and a *product* can be supplied by many *vendors* through *vendor_product*.

## Data Quality Assessment:

Both of the original spreadsheets (Sales_Dump & Product_Supplier_Master) contain several different data quality issues:

### Issues in Sales_Dump:
...

### Issues in Product_Supplier_Master:
...


## Data Cleaning Process:

*Explain how you resolved the data quality issues identified previously. Include any SQL statements used to standardize, split, convert, or update the imported data*


### Required & Additional Queries:

*Provide answers for the 3 required queries and the 3 additional queries designed by your group*
