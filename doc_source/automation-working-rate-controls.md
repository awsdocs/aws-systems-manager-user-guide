# About concurrency and error thresholds<a name="automation-working-rate-controls"></a>

You can control the deployment of an automation across a fleet of AWS resources by specifying a concurrency value and an error threshold\. Concurrency and error threshold are collectively called *rate controls*\.

**Concurrency**  
Concurrency enables you to specify how many resources are allowed to run an automation simultaneously\. Concurrency helps to limit the impact or downtime on your resources when processing an automation\. You can specify either an absolute number of resources, for example 20, or a percentage of the target set, for example 10%\.

The queueing system delivers the automation to a single resource and waits until the initial invocation is complete before sending the automation to two more resources\. The system exponentially sends the automation to more resources until the concurrency value is met\.

**Error Thresholds**  
An error threshold enables you to specify how many automations are allowed to fail before Systems Manager stops sending the automation to other resources\. You can specify either an absolute number of errors, for example 10, or a percentage of the target set, for example 10%\.

If you specify an absolute number of 3 errors, for example, the system stops running the automation when the fourth error is received\. If you specify 0, then the system stops running the automation on additional targets after the first error result is returned\.

If you send an automation to, for example, 50 instances and set the error threshold to 10%, then the system stops sending the command to additional instances when the fifth error is received\. Invocations that are already running an automation when an error threshold is reached are allowed to be completed, but some of these automations might fail as well\. If you need to ensure that there won’t be more errors than the number specified for the error threshold, then set the **Concurrency** value to 1 so that automations proceed one at a time\. 