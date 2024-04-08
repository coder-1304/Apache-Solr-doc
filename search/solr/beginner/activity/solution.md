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







