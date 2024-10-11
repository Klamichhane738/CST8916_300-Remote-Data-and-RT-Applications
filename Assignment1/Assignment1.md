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

With the help of GraphQL, an open-source query language for APIs, we can query and retrieve just the data we need, and it also provides us with a comprehensive schema of every field the API supports [2].

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

| Aspect                  | REST                                                | GraphQL                                                     |
|-------------------------|-----------------------------------------------------|-------------------------------------------------------------|
| **Single Operation**     | Requires multiple requests (e.g., PUT to update, GET to retrieve) | Both update (mutation) and data retrieval occur in a single operation |
| **Customizable Response**| Returns a fixed set of fields (unless specified)    | Allows clients to specify exactly which fields to return     |

**Comparison of REST vs. GraphQL for Stock market monitoring app**
   
 **Pros and Cons of using REST Approach**   
| **Pros**                                   | **Cons**                                      |
|--------------------------------------------|-----------------------------------------------|
| Simple and easy to understand structure    | **Obtaining Data (GET):** To obtain stock information, a REST API may include an endpoint like `/stocks/{id}`. Even if a client just needs a few specific pieces of information, they still receive all the stock details each time they request the data. |
| Well-established and widely used           | **Updating Data (PUT):** The client performs a PUT request to `/stocks/{id}` in order to update stock data, including its price. After the update is complete, they will need to submit another GET request if they wish to view the updated data. |

**Pros and Cons of using GraphQL Approach**  
| **Pros**                                   | **Cons**                                      |
|--------------------------------------------|-----------------------------------------------|
| **Request Only What You Need:** With GraphQL, clients can ask for just the stock data they want. For example, if a client only needs the current price of a stock, they can request just that instead of getting all the details. <br> **Example:** If a client wants to know just the price of Stock A, they can query GraphQL for only the price, avoiding unnecessary data transfer. | **More Complex Setup:** Setting up GraphQL can be harder and may require more work to manage, especially if you have a lot of stock data to handle. <br> **Example:** If a company is new to GraphQL, they might find it challenging to set up and maintain, especially as their stock data grows. |
| **Update and Get Data in One Go:** When updating stock prices, clients can send one request to update the price and get the new price back at the same time. **For Example** <br> if a client updates the price of Stock A, they can receive the updated price immediately without making a second request. | **Less Built-in Caching:** Unlike REST, which can easily use caching to speed up data delivery, GraphQL doesn't have built-in caching. This means you may need to create custom solutions to cache stock data effectively. <br> **Example:** Developers may need to implement caching solutions using external tools like Redis, adding complexity to the application. |

## Section 2: WebSockets for Real-time Communication
WebSockets are used to create a constant two way communication between client(web browser) and server using a secured TCP connection.
Apps that require real-time updates, such stock market trackers that see rapid fluctuations in stock prices, can greatly benefit from this strategy.
Real-time information updating becomes much easier by WebSockets, which allow messages to be sent between the client and server at any moment.

**Durable connection:** 

- Establishing the Connection: When a client, such as a web browser, requests to switch from a standard HTTP connection to a WebSocket connection, the WebSocket connection gets started. After that, the link remains open, enabling constant information sharing between the two parties at the same time. WebSocket offers this feature.
In general, WebSockets improve users' experience and ability to make decisions by making it simpler for users to track stock prices in real-time.
Features like alerts or notifications when a stock hits a specific price are beneficial.

**Real-time Updates:** 

- While using real time stock monitoring app, users can subscribe to specific stocks. And on getting the subscription, the server sends updates to the client over the WebSocket connection as soon as changes occur (like price updates or dividend (bonus))declaration from stocks.

    For example, it facilities for pushing updates, like, if the stock price of Apple Inc. rises then the server immediately pushes this update to all clients subscribed to AAPL which provides the updates quickly to users.

**Decreased Latency:**
- By cutting down on the time it takes for users to receive updates in comparison to typical HTTP queries, the persistent connection improves the user experience all around.

## How WebSockets Differ from REST and GraphQL

**1. Communication Model:**

In stock monitoring app using **REST or GraphQL**, the app (client) has to ask the server for stock updates every time. The server only sends information when the client makes a request, and after that, the connection closes.  And after that each time the client wants new data, it has to send a separate request, and ongoing connection between the client and server doesn't exist.

But in the case of a **WebSocket-based** stock app, after the first connection is established, it stays open. This allows both the app (client) and the server to send messages to each other whenever required. This method of communication is known as full-duplex communication, in which both sides can talk at the same time. It’s perfect for real-time updates, like quickly showing stock price changes as they change or providing the immediate updates to the clients.

**2. Data Flow**

In REST or GraphQL, the app (client) is required to ask the server every time for stock price updates by sending a request to the server. This means if the app wants updates in real time, it has to keep sending requests over every period of time, using more resources and possibly causing delays for each request. For example, the app might ask the server for the price of a stock (like AAPL) every 10 seconds, even if the price hasn’t changed which is frustrating.
WebSockets:

But in case of **WebSockets**, the app doesn’t require to keep asking for updates. Instead, the app subscribes to a stock (like AAPL or NVDA), and the server will automatically send updates whenever the price fluctuates. The app doesn’t have to request the data over and over which makes easy access to price updates, making the process faster and more efficient without delays.

**3. Effectiveness:**

In **REST or GraphQL**, the app(user) need to ask the server for updates on a regular basis which causes extra work for both the app and the server. This constant asking (polling) consumes a lot of resources because the connection opens and closes every time. Lets consider an example, if we're tracking five stocks (like AAPL, GOOG, MSFT, TSLA, and AMZN), the app has to send separate requests for each stock, over and over again.
This process makes the process slow and there will be waste of resources associated with the server, especially when there is no change in the stock prices.
Hence the overall system becomes less effective or efficient.

And now, when taking **GraphQL** in consideration, a single connection stays open, which reduces the extra work of starting new connections and sending repeated requests to the server. And when the WebSocket connection is active, updates will begin coming constantly. This means stock prices get updated instantly, without needing to keep asking for updates. This is crucial in stock market apps, where fast performance and real-time accuracy are very important factor for users who need to track high-frequency trades closely and in bulk amount.


For example: like day traders who might get affected with even small delays in updates.

## Section 3: Technology Recommendation and Justification

After careful consideration from section 1 and section 2 for real time stock monitoring app, following technology is recommended.
The recommended approach is **Hybrid Approach.**

**1. REST/GraphQL for Data Requests:**
For straightforward operations like adding or deleting stocks from a user's watchlist, use REST. REST is simple to use and effective for tasks that don't call for real-time updates.

    Example: When a user needs to add a stock to their watchlist, they will send the server a POST /watchlist request along with the stock symbol (such as "AAPL").

**2. GraphQL:** When the user need precise, detailed data, use GraphQL. GraphQL is efficient because it can receive the precise data required in a single request, such as the price, history, and business details, if the user wishes to retrieve detailed information about a stock.
    
    Example: User can request to fetch the stock's current price, its historical prices, and company details in just one request.

**3.Provide real-time stock price updates using WebSockets.**
The user receives the updated price from the server via the WebSocket connection as soon as AAPL's price changes.

**Justification:**

1. Real-time Requirements:

    Users expects real-time notifications, as stock values fluctuate regularly. In order to provide these immediate updates without requiring the client to continuously poll (ask) for fresh data, WebSockets are needed. This enables real-time response.

2. Data Complexity:

    In the case of managing complicated data requests, GraphQL is appropriate. For example, if a user wishes to access extensive information on various stocks, like their prices, histories, and related data, GraphQL allows the client to request exactly what is needed. This increases efficiency and lowers data transfer.

3. Scalability:

    The system can scale better by using REST/GraphQL for non-real-time operations and WebSockets for real-time updates. With WebSockets, there's no need for continuous polling, which might overwhelm the server when user traffic grows.
    To reduce bandwidth and server resources, the server only transmits updates when prices change, as compared with every client asking updates every second.

4. Developer Ease 
    - **REST** is easy to use and suitable for simple tasks like watching user lists of stocks. REST endpoints makes easy for tasks like adding stocks or managing user accounts.
    - **GraphQL** on other side can be used for more complicated queries where customers might only need certain data (such stock prices and corporate information, dividend information, company's future plans and projects.)
    - Instant stock price changes are made possible for users by **WebSockets**, which facilitate seamless real-time communication.

## Conclusion

The stock market software achieves the best of both worlds by fusing WebSockets and REST/GraphQL:

Simple operations are efficiently handled via REST.
Complex data can be efficiently and flexibly retrieved with GraphQL.
Users can make sure they don't miss any pricing changes using WebSockets' speedy real-time updates.

## References
1. https://www.geeksforgeeks.org/handling-data-updates-in-graphql/
2. https://jaredpogi.medium.com/ive-seen-real-world-use-of-graphql-in-the-financial-sector-in-theory-it-looks-really-cool-that-5bc6865acbf









