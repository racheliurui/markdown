```xml
<flow name="get:/setVariable:api-config">
	<set-variable variableName="username" value="" doc:name="username"/>

	<scripting:component doc:name="Groovy">
	<scripting:script engine="Groovy">
	<![CDATA[
	    String value = 'felipe'
	    message.setInvocationProperty('username', value)
	]]></scripting:script>
	</scripting:component>
</flow>
```



```Groovy
//More options of set Value inside Groovy
message.setInvocationProperty('myFlowVariable', 'value') // sets a flow variable, like <set-variable/>
message.setOutboundProperty('myProperty', 'value') // sets an outbound message property, like <set-property/>
message.setProperty('myInboundProperty', 'value', PropertyScope.INBOUND) // sets an inbound property
```
