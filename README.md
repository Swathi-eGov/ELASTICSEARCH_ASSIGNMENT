# ELASTICSEARCH_ASSIGNMENT


> CREATING AN INDEX WITH THE GIVEN MAPPING:

PUT /products
{
  "mappings": {
    "properties": {
      "id": {
        "type": "integer"
      },
      "name": {
        "type": "text"
      },
      "category": {
        "type": "keyword"
      },
      "price": {
        "type": "float"
      },
      "tags": {
        "type": "keyword"
      },
      "timestamp": {
        "type": "date"
      }
    }
  }
}


{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "products"
}

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


> ADDING SOME DATA

POST /products/_bulk
{ "index": { "_id": 1 }}
{ "id": 1, "name": "iPhone 12", "category": "Electronics", "price": 999, "tags": ["Apple", "Smartphone"], "timestamp": "2022-05-10" }
{ "index": { "_id": 2 }}
{ "id": 2, "name": "Samsung Galaxy S21", "category": "Electronics", "price": 899, "tags": ["Samsung", "Smartphone"], "timestamp": "2022-05-15" }
{ "index": { "_id": 3 }}
{ "id": 3, "name": "Sony PlayStation 5", "category": "Electronics", "price": 499, "tags": ["Sony", "Gaming"], "timestamp": "2022-06-01" }
{ "index": { "_id": 4 }}
{ "id": 4, "name": "Apple MacBook Pro", "category": "Electronics", "price": 1999, "tags": ["Apple", "Laptop"], "timestamp": "2022-06-15" }
{ "index": { "_id": 5 }}
{ "id": 5, "name": "Nike Air Max", "category": "Fashion", "price": 129, "tags": ["Nike", "Shoes"], "timestamp": "2022-07-05" }
{ "index": { "_id": 6 }}
{ "id": 6, "name": "Levi's Jeans", "category": "Fashion", "price": 79, "tags": ["Levi's", "Jeans"], "timestamp": "2022-07-10" }



{
  "took": 5,
  "errors": false,
  "items": [
    {
      "index": {
        "_index": "products",
        "_id": "1",
        "_version": 1,
        "result": "created",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 0,
        "_primary_term": 1,
        "status": 201
      }
    },
    {
      "index": {
        "_index": "products",
        "_id": "2",
        "_version": 1,
        "result": "created",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 1,
        "_primary_term": 1,
        "status": 201
      }
    },
    {
      "index": {
        "_index": "products",
        "_id": "3",
        "_version": 1,
        "result": "created",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 2,
        "_primary_term": 1,
        "status": 201
      }
    },
    {
      "index": {
        "_index": "products",
        "_id": "4",
        "_version": 1,
        "result": "created",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 3,
        "_primary_term": 1,
        "status": 201
      }
    },
    {
      "index": {
        "_index": "products",
        "_id": "5",
        "_version": 1,
        "result": "created",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 4,
        "_primary_term": 1,
        "status": 201
      }
    },
    {
      "index": {
        "_index": "products",
        "_id": "6",
        "_version": 1,
        "result": "created",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 5,
        "_primary_term": 1,
        "status": 201
      }
    }
  ]
}

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


1) Get the count of products in each category.

GET /products/_search
{
  "size": 0,
  "aggs": {
    "categories_count": {
      "terms": {
        "field": "category"
      }
    }
  }
}


{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 6,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "categories_count": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "Electronics",
          "doc_count": 4
        },
        {
          "key": "Fashion",
          "doc_count": 2
        }
      ]
    }
  }
}

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

2) Find the count of products in different price ranges.

GET /products/_search
{
  "size": 0,
  "aggs": {
    "price_ranges": {
      "range": {
        "field": "price",
        "ranges": [
          { "to": 100 },
          { "from": 100, "to": 500 },
          { "from": 500, "to": 1000 },
          { "from": 1000 }
        ]
      }
    }
  }
}



{
  "took": 6,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 6,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "price_ranges": {
      "buckets": [
        {
          "key": "*-100.0",
          "to": 100,
          "doc_count": 1
        },
        {
          "key": "100.0-500.0",
          "from": 100,
          "to": 500,
          "doc_count": 2
        },
        {
          "key": "500.0-1000.0",
          "from": 500,
          "to": 1000,
          "doc_count": 2
        },
        {
          "key": "1000.0-*",
          "from": 1000,
          "doc_count": 1
        }
      ]
    }
  }
}




------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

3) Search for products with a specific name.

GET /products/_search
{
  "query": {
    "match": {
      "name": "iPhone 12"
    }
  }
}

{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 1,
      "relation": "eq"
    },
    "max_score": 3.4318776,
    "hits": [
      {
        "_index": "products",
        "_id": "1",
        "_score": 3.4318776,
        "_source": {
          "id": 1,
          "name": "iPhone 12",
          "category": "Electronics",
          "price": 999,
          "tags": [
            "Apple",
            "Smartphone"
          ],
          "timestamp": "2022-05-10"
        }
      }
    ]
  }
}


------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

4) Search for products matching a search term in multiple fields

GET /products/_search
{
  "query": {
    "multi_match": {
      "query": "Electronics",
      "fields": ["name", "category"]
    }
  }
}

{
  "took": 0,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 4,
      "relation": "eq"
    },
    "max_score": 0.44183272,
    "hits": [
      {
        "_index": "products",
        "_id": "1",
        "_score": 0.44183272,
        "_source": {
          "id": 1,
          "name": "iPhone 12",
          "category": "Electronics",
          "price": 999,
          "tags": [
            "Apple",
            "Smartphone"
          ],
          "timestamp": "2022-05-10"
        }
      },
      {
        "_index": "products",
        "_id": "2",
        "_score": 0.44183272,
        "_source": {
          "id": 2,
          "name": "Samsung Galaxy S21",
          "category": "Electronics",
          "price": 899,
          "tags": [
            "Samsung",
            "Smartphone"
          ],
          "timestamp": "2022-05-15"
        }
      },
      {
        "_index": "products",
        "_id": "3",
        "_score": 0.44183272,
        "_source": {
          "id": 3,
          "name": "Sony PlayStation 5",
          "category": "Electronics",
          "price": 499,
          "tags": [
            "Sony",
            "Gaming"
          ],
          "timestamp": "2022-06-01"
        }
      },
      {
        "_index": "products",
        "_id": "4",
        "_score": 0.44183272,
        "_source": {
          "id": 4,
          "name": "Apple MacBook Pro",
          "category": "Electronics",
          "price": 1999,
          "tags": [
            "Apple",
            "Laptop"
          ],
          "timestamp": "2022-06-15"
        }
      }
    ]
  }
}

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

5) Search for products with a specific category.

GET /products/_search
{
  "query": {
    "bool": {
      "should": 
        {
          "match": {
            "category": "Electronics"
          }
        }
    }
  }
}




or 



GET /products/_search
{
  "query": {
    "match":{
      "category":"Electronics"
    }
  }
}





{
  "took": 0,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 4,
      "relation": "eq"
    },
    "max_score": 0.44183272,
    "hits": [
      {
        "_index": "products",
        "_id": "1",
        "_score": 0.44183272,
        "_source": {
          "id": 1,
          "name": "iPhone 12",
          "category": "Electronics",
          "price": 999,
          "tags": [
            "Apple",
            "Smartphone"
          ],
          "timestamp": "2022-05-10"
        }
      },
      {
        "_index": "products",
        "_id": "2",
        "_score": 0.44183272,
        "_source": {
          "id": 2,
          "name": "Samsung Galaxy S21",
          "category": "Electronics",
          "price": 899,
          "tags": [
            "Samsung",
            "Smartphone"
          ],
          "timestamp": "2022-05-15"
        }
      },
      {
        "_index": "products",
        "_id": "3",
        "_score": 0.44183272,
        "_source": {
          "id": 3,
          "name": "Sony PlayStation 5",
          "category": "Electronics",
          "price": 499,
          "tags": [
            "Sony",
            "Gaming"
          ],
          "timestamp": "2022-06-01"
        }
      },
      {
        "_index": "products",
        "_id": "4",
        "_score": 0.44183272,
        "_source": {
          "id": 4,
          "name": "Apple MacBook Pro",
          "category": "Electronics",
          "price": 1999,
          "tags": [
            "Apple",
            "Laptop"
          ],
          "timestamp": "2022-06-15"
        }
      }
    ]
  }
}

