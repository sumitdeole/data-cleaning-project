# Data Cleaning - Case study
## Introduction
The company Eniac wants the Data Analysis team to settle an ongoing debate within the team: whether or not it’s beneficial to discount products.

- The Marketing Team Lead is convinced that offering discounts is beneficial in the long run. She believes discounts improve customer acquisition, satisfaction and retention, and allow the company to grow.
- The main investors in the Board are worried about offering aggressive discounts. They have pointed out how the company’s recent quarterly results showed an increase in orders placed, but a decrease in the total revenue. They prefer that the company positions itself in the quality segment, rather than competing to offer the lowest prices in the market.

We will settle the debate by calculating product-level and order-level discounts and testing whether discounts are offered efficiently. Our findings will be summarized in a 5-minute presentation [click here]([https://link-url-here.org](https://docs.google.com/presentation/d/1jNubrzZlCHuaJ7_pp88AUhXJDCow6Q3ySpCZ4KALq-c/edit#slide=id.gc6f980f91_0_10)) presented to the Eniac CEO.

## Data
The data consists of 4 data files. Here’s a description of each table and its columns:
- **orders.csv** – Every row in this file represents an order.
    - order_id – a unique identifier for each order
    - created_date – a timestamp for when the order was created
    - total_paid – the total amount paid by the customer for this order, in euros
    - state - order states
        - “Shopping basket” – products have been placed in the shopping basket
        - “Place Order” – the order has been placed but is awaiting shipment details
        - “Pending” – the order is awaiting payment confirmation
        - “Completed” – the order has been placed and paid, and the transaction is completed.
        - “Cancelled” – the order has been canceled and the payment returned to the customer.
- **orderlines.csv** – Every row represents each one of the different products involved in an order.
    - id – a unique identifier for each row in this file
    - id_order – corresponds to orders.order_id
    - product_id – an old identifier for each product, nowadays not in use
    - product_quantity – how many units of that product were purchased on that order
    - sku – stock keeping unit: a unique identifier for each product
    - unit_price – the unitary price (in euros) of each product at the moment of placing that order
    - date – timestamp for the processing of that product
- **products.csv**
    - sku – stock keeping unit: a unique identifier for each product
    - name – product name
    - desc – product description
    - price – base price of the product, in euros
    - promo_price – promotional price, in euros
    - in_stock – whether or not the product was in stock at the moment of the data extraction
    - type – a numerical code for product type
- **brands.csv**
    - short – the 3-character code by which the brand can be identified in the first 3 characters of products.sku
    - long – brand name

## Issues
The data, however, suffers from **inconsistencies and corruption**. This data corruption hinders the computation of discounts --> obstacle to settling the debate...! 
- Corrupted numerical columns with **2 dots** in them. The columns per table are listed below
    - Products table
        - "price" column from the products table - _affects 3.58%_ of the rows --> **Dropped!**
        - "promo price" column from the products table - _affects 43.64%_ of the rows --> **Cleaned using the following two assumptions** (also see code here)
            * **Assumption 1:** Price column is correctly specified
            * **Assumption 2:** Negative discounts (<-1) are not possible (discount = price-promo price)
    - Orderlines table
        - "unit price" column from the  - _affects 12.3%_ of the rows --> **Dropped!**
- Typical issue of duplicates/missing values/outliers --> **Dropped!**

## First results
It turned out that the cleaning of promo prices worked out well. The following plot shows that promo prices share a close resemblance with unit prices. 

![Image here](/images/distribution_cleaned_promo_price.png)


### **As the *unit price* (which sales department obtains) is conceptually different from the *promotional price* (which the marketing department offered), we continue discount calculation using the promotional price.** 
