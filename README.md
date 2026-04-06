# Instagram Thrift & Handmade Store - Database ER Diagram

## Project Overview

This repository contains the Entity-Relationship (ER) diagram for an Instagram-based creator store that sells both thrifted fashion items and handmade products. The database is designed to handle the transition from manual DM/WhatsApp orders to a structured inventory and order management system.

## Business Logic & Design Decisions

The core challenge of this business model is managing two distinct types of inventory:

1. **Thrift Items:** Unique, 1-of-1 pieces with specific conditions (e.g., "Minor fade").
2. **Handmade Items:** Items that can be produced in batches with varying attributes (e.g., sizes, colors, multiple in stock).

To solve this, the database normalizes products into two tables:

- `PRODUCT`: Stores the base catalog item (e.g., "Vintage Denim Jacket" or "Crocheted Beanie").
- `PRODUCT_VARIANT`: Stores the physical inventory. A thrift item gets a single variant with `stock_quantity = 1`. A handmade item can have multiple variants for different sizes/colors with varying `stock_quantity`.

## Entities and Relationships

- **CUSTOMER:** Stores buyer details, including Instagram handle and WhatsApp number, reflecting the business's origin.
  - _1-to-Many_ with **ORDER** (A customer can place multiple orders).
- **PRODUCT & PRODUCT_VARIANT:** Separates conceptual products from physical stock/attributes.
  - _1-to-Many_ relationship between Product and Variant.
- **ORDER:** Represents a customer's purchase event.
  - _1-to-Many_ with **PAYMENT** and **SHIPPING** (Separating transient statuses from the core order details).
- **ORDER_ITEM (Junction Table):** Links Orders and Product Variants.
  - _Many-to-Many_ resolution. Captures `quantity` and importantly, `unit_price_at_purchase` to lock in the price at the time of sale.
- **PAYMENT:** Tracks transaction references, methods (UPI, Cash, Transfer), and success status.
- **SHIPPING:** Tracks courier service, tracking numbers, and delivery statuses.
