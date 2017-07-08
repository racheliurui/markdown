title: Design Pattern - Factory
date: 2016-01-05 20:10:33
tags:
- basic
- designpattern
- java
---


# Factory

```java
{  
        //real case invoking
        invokeSharedCode(new BikeFactory());
        invokeSharedCode(new CarFactory());
}

//object being created depending on factory type, and behavior is defined in interface also
public static void invokeSharedCode(TransportFactory factory){
    	 Transport transport = factory.create();
       System.out.println(transport.drive());
}

```

## core concept
Shared code interface not hardcoded with typeA or typeB,
but typeA and B all extend the same factory and initialization using same function.

After create, we can invoke the same name function provided by typeA or B.
