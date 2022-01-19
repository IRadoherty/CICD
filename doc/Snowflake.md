### Integrate with Snowflake using JavaScript

InRule® currently offers the ability to convert a compatible rule application into a JavaScript® (js) file. We now support integrating that same JavaScript file into Snowflake as a function or procedure. This gives the ability to run rules directly in Snowflake without leaving the UI.

The Snowflake CICD helper routes the rule application's minifiied JavaScript to Snowflake on catalog check-in. The name of the rule application will be the name of the produced function or procedure in Snowflake. 

The configuration is as below. The first grouping ties to the connection information. 

````
	<add key="Snowflake.User" value="{AccountUserName}"/>
	<add key="Snowflake.Password" value="{AccountPassword}"/>
	<add key="Snowflake.Account" value="{Account}"/>
	<add key="Snowflake.Host" value="{Host}"/>
	<add key="Snowflake.Warehouse" value="{WarehouseName}"/>
	<add key="Snowflake.Database" value="{Database}"/>
	<add key="Snowflake.Schema" value="{Schema}"/>
  
	<add key="Snowflake.Type" value="Procedure"/>
	
	<add key="Snowflake.RevisionTracking" value="false"/>
````
A helpful tip for the Account and the Host is if your Snowflake instance is "nt85445.east-us-2.azure.snowflakecomputing.com" the Account name is "nt85445" and the Host is "nt85445.east-us-2.azure.snowflakecomputing.com"

The Snowflake.Type field allows for a funciton or procedure for the rules to run in Snowflake as. 

The Snowflake.RevisionTracking can become useful if you would like the rule applications' revisions to be brought into the function or procedure to back test against other versions. For example, if turned on, the revision number would be attached to the end of the function or procedure name where "function_4" would run revision 4 and "function_5" would run revision 5.

Run auto rules:
CALL RuleApplicationName('RootEntityName', parse_json('{Initial Entity State}'));

Run explicit rule set:
CALL RuleApplicationName('RootEntityName', parse_json('{Initial Entity State}'), 'Explicit Rule Set');

An example of how to call a rule application named "MultiplicationApp" is provided below. It runs in the context of the MultiplicationProblem entity, recieves an initial entity state of {FactorA: "3", FactorB: "5"}, then runs the explicit rule set MultiplyAfterRounding.
CALL MultiplicationApp('MultiplicationProblem', parse_json('{FactorA: ", FactorB: 5}'), 'MultiplyAfterRounding');
