# Managing exit codes in Run Command commands<a name="command-exit-codes"></a>

Run Command lets you define how exit codes are handled in your scripts\. By default, the exit code of the last command run in a script is reported as the exit code for the entire script\. For example, you have a script that contains three commands\. The first one fails but the following ones succeed\. Because the final command succeeded, the status of the execution is reported as `succeeded`\.

**Shell scripts**  
To fail the entire command at the first failure, you can include a shell conditional statement to exit the script if any command before the final one fails\. Use the following approach\.

```
<command 1>
if [ $? != 0 ]
then
    exit <N>
fi
<command 2>
<command 3>
```

In the following example, the entire script fails if the first command fails\.

```
cd /test
if [ $? != 0 ]
then
    echo "Failed"
    exit 1
fi
date
```

**PowerShell scripts**  
PowerShell requires that you call `exit` explicitly in your scripts for Run Command to successfully capture the exit code\.

```
<command 1>
if ($?) {<do something>}
else {exit <N>}
<command 2>
<command 3>
exit <N>
```

Here is an example:

```
cd C:\
if ($?) {echo "Success"}
else {exit 1}
date
```