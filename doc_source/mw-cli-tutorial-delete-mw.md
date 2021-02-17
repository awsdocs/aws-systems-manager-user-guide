# Tutorial: Delete a maintenance window \(AWS CLI\)<a name="mw-cli-tutorial-delete-mw"></a>

To delete a maintenance window you created in these tutorials, run the following command:

```
aws ssm delete-maintenance-window --window-id "mw-0c50858d01EXAMPLE"
```

The system returns information like the following:

```
{
   "WindowId":"mw-0c50858d01EXAMPLE"
}
```