# techathon-bistro92
Smart table-based ordering system for Bistro 92 — Techathon Phase 1 submission.
## Section A: Quick Fixes

### Q1: Three Essential Features
1. **Real-Time Order Placement:** Customers can place their orders directly from the table, and the kitchen receives the order instantly.
2. **User-Friendly Interface:** The system should be easy to use for all types of customers, including tech novices.
3. **Order Customization Options:** Customers should be able to customize their food items based on their preferences.

### Q2: Two Design Principles for an Intuitive Interface

1. **Minimalist and Clean Layout:**  
The interface should have a simple design with limited buttons and clear labeling. Icons and text should be easy to understand, so even first-time users or older customers can navigate without confusion.

2. **Consistent Navigation and Feedback:**  
   The same buttons should always perform the same actions, and the system should provide immediate visual or sound feedback (like a beep or message) when an action is taken. This builds user confidence and reduces errors.

### Q3: Security Vulnerabilities and Solutions

1. **Unauthorized Access to the Admin Dashboard:**  
   - *Vulnerability:* If the admin dashboard is not protected, outsiders could view or manipulate orders and sales data.  
   - *Solution:* Implement authentication with strong passwords and role-based access control (RBAC) to restrict access.

2. **Order Tampering During Transmission:**  
   - *Vulnerability:* If order data is sent in plain text from the ESP32 to the cloud, it could be intercepted and modified.  
   - *Solution:* Use HTTPS or encrypted MQTT protocol to securely transmit order data.

3. **Theft or Misuse of Smart Pads:**  
   - *Vulnerability:* Someone could steal or misuse a smart pad to send fake orders.  
   - *Solution:* Assign each device a unique ID and validate device requests on the server side before processing.

### Q4: Strategies for Stability During Peak Hours

1. **Asynchronous Order Processing:**  
   Use asynchronous communication methods like message queues (e.g., MQTT or RabbitMQ) so orders are queued and processed smoothly without overloading the server. This ensures the system doesn't crash during high traffic.

2. **Load Balancing and Auto-Scaling:**  
   Host the backend system on a cloud platform (like AWS or Firebase) with load balancers and auto-scaling enabled. This distributes incoming traffic across multiple servers and automatically scales resources during high demand to maintain performance.

### Q5: Inventory System Integration Method

**Method: Use a Middleware API Layer**  
Build a middleware API that acts as a bridge between the ordering system and the existing inventory software. Every time a customer places an order, the system sends the order data to the middleware, which then updates the inventory in real-time.

This keeps the inventory data accurate and ensures that:
- Items are marked “out of stock” immediately when inventory runs low.
- The kitchen never prepares an item that’s unavailable.
- The integration is **non-intrusive** — it doesn't change the old inventory system, just connects to it.

✅ Example Tools: REST API, Webhooks, or IoT-friendly platforms like Node-RED.

## Section B: Tech Tricks

### Q1: Database Schema for Bistro 92 

#### 🧠 Purpose:
Design a relational database to store users, menu items, orders, tables, and payments in an efficient and scalable way.

---

#### 1. `Users`
Stores customer details .

| Column     | Type    | Description                         |
|------------|---------|-------------------------------------|
| user_id    | INT (PK)| Unique ID for each user             |
| name       | TEXT    | Customer name                       |
| email      | TEXT    | Customer email                      |
| phone      | TEXT    | Phone number                        |

---

#### 2. `Tables`
Tracks each table in the restaurant.

| Column       | Type    | Description                       |
|--------------|---------|-----------------------------------|
| table_id     | INT (PK)| Unique table ID                   |
| table_number | INT     | Table number (shown to customers) |
| status       | TEXT    | Status (free, occupied)           |

---

#### 3. `MenuItems`
List of available food/drink items.

| Column     | Type     | Description                        |
|------------|----------|------------------------------------|
| item_id    | INT (PK) | Unique ID for each item            |
| name       | TEXT     | Item name                          |
| description| TEXT     | Item details                       |
| price      | FLOAT    | Price of the item                  |
| category   | TEXT     | Category (Main, Drink, Dessert)    |
| available  | BOOLEAN  | Whether the item is in stock       |

---

#### 4. `Orders`
Main order info for each table session.

| Column     | Type     | Description                         |
|------------|----------|-------------------------------------|
| order_id   | INT (PK) | Unique order ID                     |
| table_id   | INT (FK) | Which table placed the order        |
| user_id    | INT (FK) | Who placed the order (optional)     |
| order_time | DATETIME | When the order was placed           |
| status     | TEXT     | Status (pending, preparing, served) |

---

#### 5. `OrderItems`
Individual items inside an order.

| Column             | Type     | Description                         |
|--------------------|----------|-------------------------------------|
| order_item_id      | INT (PK) | Unique line item ID                 |
| order_id           | INT (FK) | Linked to the main order            |
| item_id            | INT (FK) | Which item was ordered              |
| quantity           | INT      | How many of this item               |
| price_at_order_time| FLOAT    | Item price when ordered             |

---

#### 6. `Payments`
Tracks how orders are paid.

| Column        | Type     | Description                         |
|---------------|----------|-------------------------------------|
| payment_id    | INT (PK) | Unique payment ID                   |
| order_id      | INT (FK) | Which order was paid                |
| payment_method| TEXT     | Cash, Card, Mobile, etc.            |
| amount        | FLOAT    | Total amount paid                   |
| payment_time  | DATETIME | When payment occurred               |

---

> ⚙️ This schema is optimized for fast queries, scalability, and easy integration with cloud systems or dashboards.
