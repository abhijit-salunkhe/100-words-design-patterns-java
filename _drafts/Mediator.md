---
layout: page
title: Mediator
permalink: /Mediator/
tag: pattern
---



### Story 

Defines an object that controls how a set of objects interact.

Radio Taxi is an example of the Mediator pattern.
Taxi drivers communicate with the Mediator(Radio Taxi Call Center), rather than with each other. 

When customer needs a taxi, he calls Radio Taxi Call Center. 
All taxis have a GPS unit which tells where the taxi is present right now, also there is a central information system which tells which taxi is available to serve the customer. 
The call center will contact the available taxi nearest to customer’s location and send them to serve the customer.



### UML 
![]({{site.baseurl}}/assets/img/mediator.png)

#### *Colleague.java* 
```java 
package com.hundredwordsgof.mediator;

/**
 * Colleague defines an interface for communication with another Colleague via mediator.
 *
 */
abstract class Colleague {

	protected Mediator mediator;
	
	protected String receivedMessage;
	
	public Colleague(Mediator mediator){
		this.mediator = mediator;
	}
	
	abstract void notifyColleague(String message);

	abstract void receive(String message);

	public String getReceivedMessage() {
		return this.receivedMessage;
	}

}
```

#### *ConcreteColleague1.java* 
```java 
package com.hundredwordsgof.mediator;


/**
 * ConcreteColleague1 implements Colleague interface.
 *
 */
public class ConcreteColleague1 extends Colleague {
	
	public ConcreteColleague1(Mediator mediator) {
		super(mediator);
	}

	public void notifyColleague(String message){
		this.mediator.notifyColleague(this, message);
	}

	public void receive(String message) {
		this.receivedMessage = message;
	}
		
}
```

#### *ConcreteColleague2.java* 
```java 
package com.hundredwordsgof.mediator;

/**
 * ConcreteColleague2 implements Colleague interface.
 *
 */
public class ConcreteColleague2 extends Colleague {

	public ConcreteColleague2(Mediator mediator) {
		super(mediator);
	}

	public void notifyColleague(String message){
		this.mediator.notifyColleague(this, message);
	}

	public void receive(String message) {
		this.receivedMessage = message;	
	}
}
```

#### *ConcreteMediator.java* 
```java 
package com.hundredwordsgof.mediator;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/**
* ConcreteMediator implements Mediator, coordinates between Colleague objects.
*
*/
public class ConcreteMediator implements Mediator{

	private List<Colleague> colleagues;
	
	public ConcreteMediator(){	
		colleagues = new ArrayList<Colleague>();
	}
	
	public void addColleague(Colleague colleague){
		colleagues.add(colleague);
	}
	
	public void notifyColleague(Colleague colleague, String message) {

		for (Iterator iterator = colleagues.iterator(); iterator.hasNext();) {
			Colleague receiverColleague = (Colleague) iterator.next();
		
			if(colleague != receiverColleague){
				receiverColleague.receive(message);
			}
		}
	}
	
	
	
}
```

#### *Mediator.java* 
```java 
package com.hundredwordsgof.mediator;

/**
 * Mediator defines an interface for communicating with Colleague objects.
 *
 */
public interface Mediator {

	void notifyColleague(Colleague colleague, String message);
}
```
