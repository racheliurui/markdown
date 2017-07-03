# Sample flow implement single threading

flow client raise a request and will get response instantly.
The request will be processed asycronized (in sequence, 1 thread)

```
<?xml version="1.0" encoding="UTF-8"?>

<mule xmlns:vm="http://www.mulesoft.org/schema/mule/vm" xmlns:http="http://www.mulesoft.org/schema/mule/http" xmlns="http://www.mulesoft.org/schema/mule/core" xmlns:doc="http://www.mulesoft.org/schema/mule/documentation"
	xmlns:spring="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-current.xsd
http://www.mulesoft.org/schema/mule/core http://www.mulesoft.org/schema/mule/core/current/mule.xsd
http://www.mulesoft.org/schema/mule/http http://www.mulesoft.org/schema/mule/http/current/mule-http.xsd
http://www.mulesoft.org/schema/mule/vm http://www.mulesoft.org/schema/mule/vm/current/mule-vm.xsd">
    <http:listener-config name="HTTP_Listener_Configuration" host="0.0.0.0" port="8081" doc:name="HTTP Listener Configuration"/>

    <queued-asynchronous-processing-strategy name="allow1Threads" maxThreads="1" doc:name="Queued Asynchronous Processing Strategy"/>
    <vm:connector name="VM" validateConnections="true" doc:name="VM">
        <vm:queue-profile>
            <default-persistent-queue-store/>
        </vm:queue-profile>
    </vm:connector>
    <vm:connector name="VM1" validateConnections="true" doc:name="VM">
        <vm:queue-profile>
            <default-persistent-queue-store/>
        </vm:queue-profile>
    </vm:connector>
    <flow name="vmtransactionFlow">
        <http:listener config-ref="HTTP_Listener_Configuration" path="/registerTask" doc:name="HTTP"/>
        <set-payload value="#[java.util.UUID.randomUUID().toString()]" doc:name="Set Payload"/>
        <vm:outbound-endpoint exchange-pattern="one-way" path="TaskQ" connector-ref="VM" doc:name="VM"/>
        <logger message="Task Received, #[payload]" level="INFO" doc:name="Logger"/>
    </flow>
    <flow name="vmtransactionFlow1" processingStrategy="allow1Threads">
        <vm:inbound-endpoint exchange-pattern="one-way" path="TaskQ" connector-ref="VM" doc:name="VM">
        </vm:inbound-endpoint>
        <logger message="Started Processing #[payload]" level="INFO" doc:name="Logger"/>
        <component class="vmtransaction.demo.ProcessingTask" doc:name="Java"/>
        <logger message="finished the sync vm invoke #[payload]" level="INFO" doc:name="Logger"/>
    </flow>
</mule>
```
