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


