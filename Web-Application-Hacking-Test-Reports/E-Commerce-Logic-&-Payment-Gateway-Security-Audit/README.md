# Vulnerability Report: Business Logic Flaw – Price Manipulation via Excessive Trust in Client-Side Controls

## 1. Executive Summary
During the security assessment of the e-commerce shopping cart and checkout workflow, a critical Business Logic Flaw was identified in the product order placement mechanism (POST /cart). The application relies on client-side input for the product price parameter rather than fetching and validating product pricing strictly from the server-side database.

An attacker can manipulate the price parameter in the HTTP request using an intercepting proxy (Burp Suite) to alter item prices to arbitrary values (e.g., $0.01) and order multiple units at the modified price, resulting in Direct Financial Loss and complete control over order checkout logic.
2. Vulnerability Details

    Vulnerability Class: Business Logic Flaw / Insecure Client-Side Control

    Vulnerability Type: Price Manipulation / Parameter Tampering

    Severity: High / Critical (Direct Financial & Inventory Impact)

    Target Endpoint: POST /cart

Root Cause Analysis:

    Flawed Logic Implementation: The application accepts the product price directly from the HTTP request sent by the client browser during the "Add to Cart" action.

    Lack of Server-Side Validation: The server processes the user-supplied price and updates the user's cart without validating the productId against the authoritative server database.

    Business Impact: An attacker can reduce product costs to $0.01, scale up product quantities at the fraudulent rate, and finalize checkout successfully—causing immediate inventory drain and revenue loss for the store owner.

## 3. Proof of Concept & Step-by-Step Exploitation
Step 1: Target Product Selection & Initial Request

The target product is selected from the store inventory (Lightweight l33t Leather Jacket, originally priced at $1,337.00).

<img width="1458" height="606" alt="1" src="https://github.com/user-attachments/assets/de501abe-324c-4640-96e0-8f0c9152238e" />

Step 2: Request Interception & Price Manipulation

Upon clicking "Add to Cart", the HTTP request is intercepted using Burp Suite before reaching the backend server. The price parameter inside the POST /cart request payload is modified from its original value to 1 ($0.01).

Modified HTTP Request Payload:

Step 2: Request Interception & Price Manipulation

Upon clicking "Add to Cart", the HTTP request is intercepted using Burp Suite before reaching the backend server. The price parameter inside the POST /cart request payload is modified from its original value to 1 ($0.01).

<img width="1195" height="551" alt="4" src="https://github.com/user-attachments/assets/bd9aaf48-73b1-4dc9-a174-9a8e697752b8" />

Step 3: Cart Manipulation & Quantity Expansion

The server accepts the tampered price without server-side verification. Once the price is modified to $0.01, additional quantities of the product can be added or updated, maintaining the fraudulent unit pricing in the shopping cart total.

<img width="1195" height="573" alt="5" src="https://github.com/user-attachments/assets/cbdfab4e-a31b-41b7-a4d4-de45d2cdc881" />

    Step 4: Checkout Execution & Successful Exploitation

The checkout process is executed using the cart's manipulated total. The application processes the transaction as valid, completing the order for the arbitrary price defined by the attacker.

<img width="1256" height="496" alt="solved" src="https://github.com/user-attachments/assets/61852f02-3c84-439b-9813-e14a4e70f9c1" />

"Excessive trust in client-side controls occurs when a web application relies on data provided by the user's browser (such as item prices, roles, or discounts) to make business decisions without validating that data on the server. Because attackers fully control all request data leaving their browser, they can alter parameters like product prices before they reach your backend, leading to unauthorized discounts or free purchases."

## 5. Remediation Recommendations

Remove Price Data from Client Requests: The client-side POST /cart request should only transmit non-sensitive parameters such as productId and quantity.

    Mandatory Server-Side Price Lookup: The backend server must query its internal, secure database using the productId to retrieve the true item price before calculating cart totals.

    Integrity Validation: Ensure all sensitive transactions (cart updates, discounts, order checkouts) perform server-side calculations exclusively.
