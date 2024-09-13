# Frequently asked questions

### Is every change to my database captured?

No. Theoretically, if a row in your database only meets the criteria of your triggering SQL query for a short period of time (less than the interval at which we query your database), there is a chance that Quailrunner won't pick it up. If creating triggers based on changes is important to your use case, we recommended creating a log table to capture those changes.
