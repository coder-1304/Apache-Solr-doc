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
    "id": "1",
    "title": "Doc 1"
  },
  {
    "id": "2",
    "title": "Doc 2"
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

