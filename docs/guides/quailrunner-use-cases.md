# Is Quailrunner right for your use case?

Quailrunner is designed to solve many common data processing needs across various product types. However, it's important to understand its specific strengths and limitations to determine if it's the best fit for your project.

## Understanding Quailrunner's approach

Quailrunner operates on the current state of your data, rather than tracking changes over time. Here's how it works:

1. You define SQL criteria for selecting rows from your database.
2. Quailrunner periodically runs this query.
3. For each row that meets the criteria, Quailrunner executes the specified actions.
4. Rows that don't meet the criteria when the query runs are not processed, even if they met the criteria at some point in the past.

## When to choose Quailrunner

Quailrunner is an excellent choice when you need:

1. Periodic batch processing of data
2. Execution of complex SQL queries or business logic
3. Resource-efficient processing with some tolerance for delay
4. Actions based on the current state of your data
5. Triggers from aggregated data across multiple rows or tables
6. A simple setup without additional infrastructure requirements

## Handling change-based triggers

While Quailrunner doesn't inherently track changes, it can still be useful for change-based scenarios:

If your application logic depends on triggering actions based on data changes, Quailrunner can still be a good fit. You can create a log table to record these changes and then base your Quailrunner actions on that table. This approach allows you to leverage Quailrunner's strengths while still capturing and responding to data changes in your system.

## When to consider alternatives

While Quailrunner is versatile, there are scenarios where other solutions might be more appropriate:

1. You require immediate processing of data changes
2. Your architecture demands real-time, event-driven responses
3. You need near-instant synchronization across multiple systems
4. Your use case involves capturing and processing changes within database transactions
5. You're looking to stream data changes to other systems
6. Your strategy involves efficient incremental backups based on data changes

## Making the right choice

When evaluating Quailrunner, consider your specific needs:

- Do you need to process data based on its current state, or do you need to track and respond to every change?
- Is periodic processing sufficient, or do you require real-time updates?
- Are you looking for a solution that's easy to set up and maintain?

By understanding these factors, you can determine whether Quailrunner aligns with your project requirements or if an alternative solution would be more suitable.
