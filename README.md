## 🏗️ Architecture Overview

The system consists of three services communicating with each other:

```
                  ┌───────────────────────────┐
                  │      React Frontend       │
                  │   http://localhost:3000   │
                  └──────┬─────────────┬──────┘
                         │             │
        1. Fetch Users   │             │ 2. Fetch Orders
      (GET /api/users)   │             │ (GET /api/orders)
                         ▼             ▼
  ┌───────────────────────────┐   ┌───────────────────────────┐
  │      user-service         │◀──┤      order-service        │
  │    Golang | Port 8081     │   │    Golang | Port 8082     │
  └───────────────────────────┘   └───────────────────────────┘
                                   3. Fetch Enriched User Data
                                    (GET /api/user?id=X)
```

1. **User Service**: Serves a hardcoded list of users and individual user details lookup on port `8081`.
2. **Order Service**: Serves a hardcoded list of orders on port `8082`. When orders are fetched, it internally queries the User Service via HTTP to enrich the order with the user's name.
3. **React Frontend**: A minimal dashboard built with React + Vite that displays services status, users, and orders.
