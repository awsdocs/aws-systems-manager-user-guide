# Tutorial: Delete a Maintenance Window \(CLI\)<a name="mw-cli-tutorial-delete-mw"></a>

To delete a Maintenance Window you created in these tutorials, run the following command:

```
aws ssm delete-maintenance-window --window-id "mw-0c5ed765acEXAMPLE"
```

The system returns information like the following:

```
{
   "WindowId":"mw-0c5ed765acEXAMPLE"
}
```