# DBMS Unit V — Two-Tier and Three-Tier Architecture

**Topic:** Client-server architecture  
**Exam focus:** Diagram, working, differences, advantages and limitations

## 1. What is a tier?

A **tier** is a logical layer in which part of an application runs. In a database application, tiers describe how the client, application logic and database are separated and deployed.

Do not confuse two-tier/three-tier architecture with the three-schema architecture (external, conceptual and internal levels). They describe different ideas.

## 2. Two-tier architecture

In a two-tier system, the client communicates directly with the database server.

```text
+-----------------------------+
| Client                      |
| User interface              |
| Some application logic      |
+--------------+--------------+
               |
        SQL requests/results
               |
+--------------v--------------+
| Database Server             |
| DBMS + Database             |
+-----------------------------+
```

### How it works

1. The user performs an action in the client application.
2. The client sends an SQL query or database request directly to the database server.
3. The DBMS processes the request and accesses the stored data.
4. The database server returns the result to the client.
5. The client displays the result.

### Example

A desktop library application on computers in a small college LAN connects directly to one central database server. The desktop program handles the screens and sends SQL queries to the server.

### Advantages

- Simple design and easy to develop for a small system.
- Fewer communication layers can give good response time on a local network.
- Suitable for small applications with a limited number of clients.

### Limitations

- Many clients connect directly to the database, increasing connection-management work.
- Database access details may be exposed in client applications.
- Updating business rules can require changes on multiple client installations.
- Scaling and securing a large number of clients is harder.

## 3. Three-tier architecture

A three-tier system separates the user interface, application logic and database into three layers.

```text
+-----------------------------+
| Presentation Tier           |
| Browser / Mobile / UI       |
+--------------+--------------+
               |
        HTTP / API request
               |
+--------------v--------------+
| Application Tier            |
| Business rules, validation, |
| authentication, services     |
+--------------+--------------+
               |
        SQL / database request
               |
+--------------v--------------+
| Data Tier                   |
| DBMS + Database             |
+-----------------------------+
```

### How it works

1. The user interacts with the presentation tier, such as a browser or mobile app.
2. The presentation tier sends a request to the application tier through an API.
3. The application tier checks the request, applies business rules and decides what data is needed.
4. It communicates with the database tier.
5. The database returns the requested data to the application tier.
6. The application tier prepares a response and sends it to the presentation tier.

### Example

In an online shopping application:
- **Presentation tier:** website showing products and the shopping cart.
- **Application tier:** checks stock, calculates totals and validates orders.
- **Data tier:** stores customers, products, orders and payments.

### Advantages

- Better separation of responsibilities makes the application easier to maintain.
- Business rules are managed centrally in the application tier.
- The database can be kept behind the application layer instead of being exposed to every client.
- Application servers can often be scaled independently when demand grows.

### Limitations

- More components make the system more complex to build and monitor.
- Extra network communication can add latency.
- Application-tier servers need deployment, security and capacity management.

## 4. Two-tier vs three-tier

| Feature | Two-tier | Three-tier |
|---|---|---|
| Main layers | Client, database server | Presentation, application, database |
| Database access | Client connects directly | Application tier accesses database |
| Business logic | Often partly in client | Centralized in application tier |
| Complexity | Lower | Higher |
| Maintenance | Client updates may be needed | Central application logic is easier to update |
| Security boundary | Database is reached by clients | Database can be isolated behind application tier |
| Typical use | Small LAN/desktop system | Web, mobile and enterprise systems |

## 5. Quick exam answer

**Two-tier architecture:** A client-server architecture in which the client application communicates directly with the database server. It is simple and suitable for small applications, but becomes harder to secure and scale when many clients connect.

**Three-tier architecture:** An architecture that separates an application into presentation, application and data tiers. The application tier handles business logic between the user interface and database. It improves maintainability, security boundaries and scalability, but adds complexity and communication overhead.

## 6. Remember the request path

- **Two-tier:** Client → Database → Client
- **Three-tier:** Presentation → Application → Database → Application → Presentation

## 7. Check your understanding

1. In which architecture does the client connect directly to the database server?
2. Which tier validates an order in an online shopping application?
3. Give one advantage and one limitation of three-tier architecture.
4. Why is three-tier architecture generally preferred for large web applications?

### Answers

1. Two-tier architecture.
2. Application tier.
3. Advantage: centralized business logic or improved separation. Limitation: added complexity or network overhead.
4. It separates the interface, business rules and data storage, making the system easier to maintain and scale independently.
