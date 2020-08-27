# Create a multi\-line parameter \(AWS CLI\)<a name="param-create-cli-multiline"></a>

You can use the AWS CLI to create a parameter with line breaks\. Adding line breaks lets you break up the text in longer parameter values for better legibility or, for example, more easily update multi\-paragraph parameter content for a web page\. You can include the content in a JSON file and use the `--cli-input-json` option, using line break characters like `/n`, as shown in the following example\.

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a multi\-line parameter\. 

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "MultiLineParameter" \
       --type String \
       --cli-input-json file://MultiLineParameter.json
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "MultiLineParameter" ^
       --type String ^
       --cli-input-json file://MultiLineParameter.json
   ```

------

   The following example shows the contents of the file `MultiLineParameter.json`\.

   ```
   {
       "Value": "<para>Paragraph One</para>\n<para>Paragraph Two</para>\n<para>Paragraph Three</para>"
   }
   ```

The saved parameter value is stored as follows\.

```
<para>Paragraph One</para>
<para>Paragraph Two</para>
<para>Paragraph Three</para>
```