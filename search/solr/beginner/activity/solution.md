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

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/ccabaadb-0637-4172-8c07-161fa9ad3e73)

## 2. Basic Querying

**a. Searching for all orders with the product name "smartphone"**

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/9e0fb543-9fbc-4bc6-9189-5d78dfb7a730)

**b. Searching all orders with electronics category**

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/844e9b28-c0db-417c-9f16-f6fa7db13d33)

**c. Searching by order_id**

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/b06363d9-08a6-4a32-9fe6-21f07f9837bf)

**d. Fetching orders where order date is between March 1st, 2022, and March 3rd, 2022**

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/16b38119-88da-4596-b1df-fb0f846a4d42)






