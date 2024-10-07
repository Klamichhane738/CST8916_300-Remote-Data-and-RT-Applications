From the given use cases, I have decided to concentrate on creating a **real-time stock market monitoring app** for this assignment since it is a crucial tool for traders and investors who require immediate access to real-time market data. I will examine the benefits and drawbacks of REST, GraphQL, and WebSockets through this project, evaluating how each can facilitate real-time data flow and identifying the most effective way to run a high-performance stock tracking program.

## Section 1: REST and GraphQL for Data Requests and Updates

**REST: Handling Data Requests and Updates**

An architectural technique for creating networked applications is called **REST (Representational State Transfer)**. RESTful systems use standard, stateless HTTP endpoints to offer resources (like stock data). For the stock market app, we'll be working with REST. Utilizing specified HTTP methods and endpoints, REST would manage various data operations (such obtaining stock information, adding stocks to the watchlist, or changing stock prices).

Endpoint Structure: 
- **GET/stocks**
A list of all stocks that are accessible for monitoring can be obtained by using the GET /stocks command. 

    Example: GET /stocks

    { 
        
        "id": "NVDA", "name": "NVIDIA Corp.", "price": 129.00
     }

- **GET /stocks/{id}** : Retrieves complete information regarding a particular stock by using its unique ID (NVDA, for NVIDIA Corp, for example).

    Example: GET /stocks/NVDA

    {

        "id": "NVDA",
        "name": "NVIDIA Corp",
        "price": 129.00,
    }

- **POST /stocks:** It is used for adding a new stock to the user's watchlist.

    POST /stocks with a request body looks like:

    {

        "id": "MSFT",
        "name": "Microsoft Corporation",
        "price": 400.00
    }

- **PUT /stocks/{id}**: PUT /stocks/AAPL

    Modifies the price and market capitalization of an already-existing stock.

    {

        "price": 150.00,

        "marketCap": "2.5T"
    }

- **DELETE /stocks/{id}:**  To remove a resource, such as a stock from the watchlist, we need to use the DELETE command.

    Example: DELETE /stocks/NVDA would remove NVIDIA from the list.

**GraphQL: Handling Data Requests and Updates**

With the help of GraphQL, an open-source query language for APIs, we can query and retrieve just the data we need, and it also provides us with a comprehensive schema of every field the API supports.

*Mutations in GraphQL* allow us to alter data on the server, allowing us to do operations on the backend database such as adding, removing, or editing information. Users can consult the GraphQL schema to ascertain the precise fields needed for every action, as these modifications are defined there. [1]

Note : Queries (to retrieve data) and Mutations (to modify data).

*GraphQL Queries (Similar to GET Requests)*

GraphQL reduces both over- and under-fetching by allowing clients to specify exactly what data they want, in contrast to REST, which lets us to hit an endpoint and retrieve all the specified data.
**Query example:** If the client wishes to retrieve particular stock information, such as name, price, and market capitalization:

    query 
    {
        stock(id: "NVDA") 
        { name
          price
          marketCap
        }
    }
Unlike REST, which would return complete stock details, we can query simply the price of a stock if that is all that matters to you using GraphQL.

*GraphQL Mutations (Similar to POST/PUT/DELETE Requests):*

A big advantage of mutations in GraphQL over REST's POST and PUT methods is that with a mutation, we can change data and get the updated result in one go. This means we don’t need to make two separate requests—one to change the data and another to fetch the updated data—making it quicker and easy.

- Example Mutation:

    mutation 

    {

        updateStock(id: "NVDA", data: { price: 260.00 })
        {
            id
            price
        }
    }






## References
1. https://www.geeksforgeeks.org/handling-data-updates-in-graphql/









