# About Concurrency and Error Thresholds<a name="automation-working-rate-controls"></a>

You can control the execution of an Automation workflow across a fleet of AWS resources by specifying a concurrency value and an error threshold\. Concurrency and error threshold are collectively called *rate controls*\.

**Concurrency**  
Concurrency enables you to specify how many resources are allowed to run an Automation simultaneously\. Concurrency helps to limit the impact or downtime on your resources when processing an Automation\. You can specify either an absolute number of resources, for example 20, or a percentage of the target set, for example 10%\.

The queueing system delivers the Automation to a single resource and waits until the initial invocation is complete before sending the Automation to two more resources\. The system exponentially sends the Automation to more resources until the concurrency value is met\.

**Error Thresholds**  
An error threshold enables you to specify how many Automation workflows are allowed to fail before Systems Manager stops sending the Automation to other resources\. You can specify either an absolute number of errors, for example 10, or a percentage of the target set, for example 10%\.

If you specify an absolute number of 3 errors, for example, the system stops sending the Automation when the fourth error is received\. If you specify 0, then the system stops sending the Automation to additional resources after the first error result is returned\.

If you send an Automation to, for example, 50 instances and set the error threshold to 10%, then the system stops sending the command to additional instances when the sixth error is received\. Invocations that are already running an Automation when an error threshold is reached are allowed to be completed, but some of these Automations might fail as well\. If you need to ensure that there won’t be more errors than the number specified for the error threshold, then set the **Concurrency** value to 1 so that Automations proceed one at a time\. 