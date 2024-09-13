# Overview

This guide outlines how Quailrunner runs the actions associated with the triggers that you create.

### First, we look for rows.

Every ten seconds, Quailrunner scans the DB table associated with your trigger using the SQL query that you wrote to retrieve all qualified rows. For example, if you wrote this SQL query:

```sql
SELECT uuid AS entity_pk, email
FROM public.users
WHERE deleted_at IS NULL;
```

we would scan your `public.users` table for all rows where `deleted_at IS NULL`, AND where we have never successfully run the action associated with this particular trigger, AND where we have retries left.

By default, triggered actions will retry 5 times if they do not succeed. You can manually queue them up for additional retries.

> **Notes on writing your SQL queries**
>
> - You _must_ `SELECT` the primary key of your table and rename it in your query to `entity_pk` — it should look something like this: `SELECT primary_key AS entity_pk FROM "schema"."tableName";`. You could also create your own "primary key" by `SELECT`ing a combination of fields and naming it `entity_pk`, as long as that combination will be unique to each row you want to run your action on.
> - Other than that, you are free to `SELECT` any other columns on any other tables you are able to join to the primary entity table. You’ll want to `SELECT` any and all fields you plan on using in the action. For example, if you are going to send an email using a third party transactional email service, you’ll want to `SELECT` the column that contains the email address you want to send the email to, and any other columns that will help you populate the email body.
> - Remember that this query will be hitting _your_ database, so only use PostgreSQL features that your version of Postgres supports.

### Next, we perform the action.

When we find qualified rows, we will send one `POST` request per row to your specified endpoint.

We send the following authentication header with each request, in case you are sending a request to your own server and wish to authenticate it. You can find your unique organization UUID in the task creation form.

```
X-Quail-Task-Secret: <your-unique-uuid>
```

We also allow you to send custom headers and a custom body as you wish, which means that you can easily integrate third-party APIs directly into Quailrunner.

When constructing your request body, you can use static values and/or any of the columns that you `SELECT` in your SQL predicate. To use columns from your database, use the following syntax:

```js
{
  "field1": "{{.column1Name}}",
  "field2": "{{.column2Name}}"
}
```

Make sure to respond to our request with a `200` status code - otherwise, Quailrunner will think that the action has failed and will simply retry it.

### Finally, we record each run result.

If the action succeeded, we record the result of the run and we mark the row as `completed`. No more runs of this action will be attempted on that row.

If the action failed, we automatically retry the row up to 5 times before marking the row as `failed`. If a row is marked as `failed`, you can queue it up for additional retries via your dashboard.
