# MonitoreQueue-

# Problem-
There are some SQS queues on amazon (+/- 10 queues, but the number of queues is growing
every month). Queues are for different applications, different teams, and have different
characteristics (error queues, different purposes, different thresholds etc.). The teams are
responsible for adding new queues and specifying which queues are applicable to be
monitored.
# Target-
As a DevOps team we would like to monitor these queues by providing an automation solution
which creates a monitor per queue according to a definition file where the queues are
specified. So that we can be alerted whenever something goes wrong with the queues.

# General Requirement-
- Any language / automation tool may be used;
- It should be easy to specify new queues in the definition file, all stakeholder-teams
should be able to add or modify the monitors for their queues/applications easily;
tip: create a good structure for the definition file, some queues share the
same characteristics;
- A monitor per queue needs to be created;
- The monitor should notify whenever defined thresholds are exceeded;
- A monitor can either be an output of a script which indicates that a queue has an error
or any monitoring solution like nagios, zabbix, datadog, prometheus etc.;

Requirements for monitoring:
1. All teams want to be notified if there are any errors in the error SQS (_error queues).
2. Team 1 and team 2 want to be notified if there are more than 10 messages in
test_devops_new_houses and test_devops_makelaars queues.
3. Team 3 wants to be notified if there are more than 25 messages in standard non-error
queues.


# Solution-
Slack as a communication platform between teams, sending a notification to a team Slack channel is a good way to integrate between the alarting system and team communication.

Created template.yaml file as per requirement given the number to monitor
A team can define thier queues in this simple YAML format:
sqs:
  - team1:
    - test_devops_makelaars: 10
    - test_devops_makelaars_errors: 0

  - team2:
    - test_devops_new_houses: 10
    - test_devops_new_houses_errors: 0
    - test_devops_edited_houses: x
    - test_devops_edited_houses_errors: 0
    - test_devops_removed_houses: x
    - test_devops_removed_houses_errors: 0

  - team3: 
    - test_devops_stats_phone_clicks: 25
    - test_devops_stats_facebook_clicks: 25
    - test_devops_stats_phone_clicks_errors: 0
    - test_devops_stats_facebook_clicks_errors: 0
The ERROR queue will have limit value 0, where you can use x for the queue you want to skip.

AWS Lambda function will run every minute to check the defined queue in the YAML file stored in GITHUB, where stakeholder can edit it easily.

# Setup-

Setup steps:

Create Slack App and allow it to post to a team channel.
Create AWS SQS Queues .
Edit the keys in Lambda source code.
Create AWS Lambda function and delopy the code.
Test it.
