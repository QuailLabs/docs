# Should I use Quailrunner?

While we think that Quailrunner solves a lot of common use cases for many different types of products, there are definitely cases that we explicitly do not solve for. We hope this guide helps you understand whether or not Quailrunner is right for you.

### Quailrunner vs. change detection capture systems

#### What is change data capture?

Change data capture (CDC) is a software design pattern that identifies and tracks changes to data in a database, enabling real-time data integration and synchronization across systems.

As an example, imagine a retail database with a "Products" table. When an employee updates a product's price:

1. **Original:** ProductID: 101, Name: "Widget", Price: $9.99
2. **Update:** Price changed to $10.99
3. **CDC Process:**
   - Detects change
   - Captures: ProductID 101, _Price $9.99 → $10.99_
   - Notifies: Analytics, Inventory systems

**Result:** Real-time updates without constant database polling

#### How is Quailrunner different?

Quailrunner is NOT a CDC system - either a row meets the SQL criteria when we run it, or it does not. Rows meeting your criteria will have their corresponding actions ran. If a row WOULD have met the criteria at a point in time but no longer does when we query your database, the row will not be picked up.

#### Choose Quailrunner when you need:

1. Periodic batch processing of data
2. Complex SQL queries or business logic
3. Resource-efficient processing with some delay tolerance
4. Actions based on current data state, not change history
5. Triggers from aggregated data across multiple rows/tables
6. Simple setup without additional infrastructure

#### Choose CDC when you need:

1. Immediate processing of data changes
2. Real-time event-driven architecture
3. Near-instant synchronization across systems
4. Capture and processing within database transactions
5. Streaming of data changes to other systems
6. Efficient incremental backup strategies
