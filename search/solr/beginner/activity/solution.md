Created a new collection named `orders` with all default settings as created in the SolrCloud.md. 
Using Postman for performing operations using Solr's API


# Tasks

## 1. Document Indexing

Using Solr's `/update` endpoint for indexing data and adding orders data in the request body.
**API**
- **Method:** POST
- **Endpoint:** `http://localhost:8983/solr/orders/update`
- **Content-Type:** `application/json`
- **Request body:**
```json
[
  {
    "order_id": "1",
    "product_name": "Laptop",
    "customer_name": "John Doe",
    "order_date": "2022-03-01",
    "category": "Electronics",
    "price": 1200.00,
    "reviews": [
      {"customer": "customer1", "comment": "Fast delivery, great product."},
      {"customer": "customer2", "comment": "Excellent performance."},
      {"customer": "customer3", "comment": "Satisfied with the purchase."}
    ]
  },
  {
    "order_id": "2",
    "product_name": "Smartphone",
    "customer_name": "Alice Smith",
    "order_date": "2022-03-02",
    "category": "Electronics",
    "price": 800.00,
    "reviews": [
      {"customer": "customer4", "comment": "Good value for money."},
      {"customer": "customer5", "comment": "User-friendly interface."},
      {"customer": "customer6", "comment": "Happy with the purchase."}
    ]
  },
  {
    "order_id": "3",
    "product_name": "Running Shoes",
    "customer_name": "Bob Johnson",
    "order_date": "2022-03-03",
    "category": "Fashion",
    "price": 80.00,
    "reviews": [
      {"customer": "customer7", "comment": "Comfortable and stylish."},
      {"customer": "customer8", "comment": "Perfect fit."},
      {"customer": "customer9", "comment": "Great for running."}
    ]
  },
  {
    "order_id": "4",
    "product_name": "Coffee Maker",
    "customer_name": "Emily Davis",
    "order_date": "2022-03-04",
    "category": "Home & Kitchen",
    "price": 50.00,
    "reviews": [
      {"customer": "customer10", "comment": "Easy to use and clean."},
      {"customer": "customer11", "comment": "Makes delicious coffee."},
      {"customer": "customer12", "comment": "Happy with the purchase."}
    ]
  },
  {
    "order_id": "5",
    "product_name": "Books Set",
    "customer_name": "Charlie Brown",
    "order_date": "2022-03-05",
    "category": "Books",
    "price": 120.00,
    "reviews": [
      {"customer": "customer13", "comment": "Exciting reads, fast delivery."},
      {"customer": "customer14", "comment": "Great collection."},
      {"customer": "customer15", "comment": "Satisfied with the purchase."}
    ]
  }
]
```

Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/a39749fc-3cbe-40ff-83d9-2a5955d2470b)



## 2. Basic Querying

**a. Searching for all orders with the product name "smartphone"**

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?q=product_name:smartphone`
  
Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/0e120ef8-a7e1-4633-b732-2192e98a65a2)


**b. Searching all orders with electronics category**

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?q=category:electronics`
  
Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/76a8951d-4e55-460f-9892-641452afca35)


**c. Searching by order_id**

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?q=order_id:3`
  
Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/e966c83c-13f3-48e7-af97-84e9de7c3310)

**d. Fetching orders where order date is between March 1st, 2022, and March 3rd, 2022**

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?q=order_date:[2022-03-01T00:00:00Z TO 2022-03-03T00:00:00Z]`
  
Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/21350da7-5fb6-423e-a15f-2c6fb8b52439)


**e. Fetching orders with price higher than 100**

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?q=price:[100 TO *]`
  
Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/40ae88c9-d9db-4a82-a39f-ada3e8578aec)

**f. Fetching orders with price lower than 50**

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?q=price:[* TO 50]`
  
Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/bfad47a2-42d0-48cc-b57f-e0b18adf0731)


## 3. Faceting

**Faceting Queries:**

### a. Field Faceting 

#### Fetching the number of orders for each category
**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?q=*:*&facet=true&facet.field=category&rows=0`
  
Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/e55dca58-4dbd-42f1-8ea1-94296a583eab)

#### Fetching the number of orders for each category where price is higher than 500
**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?q=price:[500 TO *]&facet=true&facet.field=category&rows=0`
  
Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/43fc1695-3cb3-4549-b493-854e890b4d48)

### b. Range Faceting (Price Ranges):

#### Getting price analysis of orders, grouping by their price ranges

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?q=*:*&facet=true&facet.range=price&facet.range.start=0&facet.range.end=1400&facet.range.gap=200&rows=0`
  
Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/919fd578-f5e7-4e19-a070-9dbd3d6364bb)

#### Counting the daily orders from March 1st to March 6th, 2022.

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?q=*:*&facet=true&facet.range=order_date&facet.range.start=2022-03-01T00:00:00Z&facet.range.end=2022-03-06T00:00:00Z&facet.range.gap=%2B1DAY&rows=0`
  
Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/a3639fc4-2cc2-4f3b-8ba4-54f30fc56dec)

### c. Pivot Faceting (Product Name and Category):

#### This query will return the count of orders grouped by both "product_name" and "category," providing insights into the distribution of products across different categories.

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?q=price:[* TO 50]&facet=true&facet.pivot=product_name,category`
  
Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/d2acc0c7-b1a4-4a00-ad44-4c55fe70e054)


## 4. Sorting

### a. Sorting orders based on order_date

**API**
- **Method:** GET
- **Endpoint:**
  - For ascending order: `http://localhost:8983/solr/orders/select?indent=true&q=*:*&sort=order_date%20asc`
  - For descending order: `http://localhost:8983/solr/orders/select?indent=true&q=*:*&sort=order_date%20desc`

### b. Sorting orders based on customer's name

Using copy field customer_name_str

**API**
- **Method:** GET
- **Endpoint:**
  - For ascending order: `http://localhost:8983/solr/orders/select?indent=true&q=*:*&sort=customer_name_str%20asc`
  - For descending order: `http://localhost:8983/solr/orders/select?indent=true&q=*:*&sort=customer_name_str%20desc`
 
### c. Sorting orders based on price

**API**
- **Method:** GET
- **Endpoint:**
  - For ascending order: `http://localhost:8983/solr/orders/select?indent=true&q=*:*&sort=price%20asc`

## 5. Filtering

### a. Filtering orders where order date starting fro 4 March 2022 till now

**API**

- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?indent=true&q=order_date:[2022-03-04T00:00:00Z TO NOW]`


### b. Filtering orders where customer's name starts with `A`

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?indent=true&q=customer_name:A*`

Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/920ea908-3ed8-4f5b-8dcb-89dbe9366beb)

### c. Fetching non-electronic category orders with price greater than 100

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?indent=true&q=price:[100 TO *]%20NOT%20category:electronics`

### d. Retrieving orders for the electronics category with product reviews containing the phrase "fast delivery"

**API**
- **Method:** GET
- **Endpoint:** `http://localhost:8983/solr/orders/select?indent=true&q=reviews:"fast delivery"%20AND%20category:electronics`

Response:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/d6f00f88-ebc3-407f-a598-92040a56fe92)
