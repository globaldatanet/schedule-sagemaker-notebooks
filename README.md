# schedule-sagemaker-notebooks
Scheduling start and stop of on-demand notebook instances

## Creating Lambdas for Starting and Stopping

1. Create Lambda function for starting and stopping on-demand notebook instances with specific keywords in their name. For this example, our development team’s on-demand notebook instances have names starting with dev-.

2. Your Lambda function should have the SageMakerFullAccess policy attached to its execution IAM role.

3. Under Basic Settings, change Timeout to 15 minutes (max).
This step makes sure the function has the maximum allowable timeout range during stopping and starting multiple notebooks.

## Creating a CloudWatch event
Now that you have created the functions, you need to create an event to trigger these functions on a specific schedule.

We use cron expression format for the schedule. For more information about creating your custom cron expression, see Schedule Expressions for Rules. All scheduled events use UTC time zone, and the minimum precision for schedules is 1 minute.

For example, the cron expression for 7:00 AM, Monday through Friday throughout the year, is `0 7 ? * MON-FRI *`, and for 9:00 PM on the same days is `0 21 ? * MON-FRI *`.

To create the event for stopping your instances on a specific schedule, complete the following steps:

1. On the CloudWatch console, under Events, choose Rules.
2. Choose Create rule.
3. Under Event Source, select Schedule, and then select Cron expression.
4. Enter your cron expression (for example, 21 ? * MON-FRI * for 9:00 PM Monday through Friday).
5. Under Targets, choose Lambda function.
6. Choose your function from the list 
7. Choose Configure details
8. Add a name for your event, such as `Stop-Notebooks-Event`, and a description.
9. Leave Enabled
10 Choose Create
