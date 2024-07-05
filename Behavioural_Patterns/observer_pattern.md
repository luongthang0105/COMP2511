# Context

- Suppose our software is managing a hospital, where its main functionality is to receive emergency calls and quickly broadcast that to the available nurses, doctors, etc.
- The question is how should we design this program.

# Concepts

## Event-driven program

- States that our program is either handling some event/actions or not doing anything at all.

## Subject/Observable/Publisher

- Objects that trigger the event (in the context above, a patient could be the [subject](#subjectobservablepublisher)).
- Maintains a list of dependents, called [Observers](#observerssubscribers)
- Notifies the observers automatically of any state changes in the subject

## Observers/Subscribers

- Objects that receive notifications from [publishers](#subjectobservablepublisher) (i.e. doctors, nurses)

## Passing data

### Push

- Subject passes the changed data to its observers

### Pull

- Subject passes reference to itself to its observers, and the observers need to pull the required data from the subject

# The aim of Observer Pattern

- Be able to dynamically add/remove observers
- Define a one-to-many (1 publisher to many subscribers) dependency without making the objects **tightly coupled**
- **Automatically** notify/update subscribers when the subject changes state

# Implementation

The full course code for this part is [here](https://nw-syd-gitlab.cseunsw.tech/COMP2511/24T2/content/-/tree/main/lectures/week04/PatternObserver/src?ref_type=heads) (only available to 24T2 COMP2511 UNSW students).

## Version 1

First, we create an interface called **Subject**, and our **Thermometer** class will implement this interface.
Thermometer also have a list of observers, attach/detach observer methods, and a notifying observers method.

```java
public interface Subject {
	public void attach(Observer o);
	public void detach(Observer o);
	public void notifyObservers();
}

public class Thermometer implements Subject {

	ArrayList<Observer> listObservers = new ArrayList<Observer>();
	double temperatureC = 0.0;

	@Override
	public void attach(Observer o) {
		if(! listObservers.contains(o)) { listObservers.add(o); }
	}

	@Override
	public void detach(Observer o) {
		listObservers.remove(o);
	}

	@Override
	public void notifyObservers() {
		for( Observer obs : listObservers) {
			obs.update(this);
		}
	}

	public double getTemperatureC() {
		return temperatureC;
	}

	public void setTemperatureC(double temperatureC) {
		this.temperatureC = temperatureC;
		notifyObservers();
	}
}
```

On the other hand, **DisplayAustralia** implements **Observer** and in the _update()_ method, it checks for instances of **Thermometer** before updating.

```java
public interface Observer {
	public void update(Subject obj);
}

public class DisplayAustralia implements Observer {
	Subject subject;
	double temperatureC = 0.0;

	@Override
	public void update(Subject obj) {
		if(obj instanceof Thermometer) {
			this.temperatureC = ((Thermometer) obj).getTemperatureC();
			display();
		}
	}

	public void display() {
		System.out.printf("From DisplayAus: Temperature is %.2f C\n", temperatureC);
	}
}
```

## Version 2 (Type safer)

The above implementation needs to check for specific instances before updating. This is, indeed, not type-safety at all, since we can give a non-subscriber subject and we are not aware of it.

Hence, in this version, we introduce some other classes which will fix this issue, though it maybe more verbose.

The main difference is the **SubjectThermometer** and **ObserverThermometer** interfaces.

The actual subject class, **Thermometer**, will implements **SubjectThermometer**.

```java
public interface SubjectThermometer {
	public void attach(ObserverThermometer o);
	public void detach(ObserverThermometer o);
	public void notifyObservers();

	public double getTemperatureC();
}
public class Thermometer implements SubjectThermometer {

	ArrayList<ObserverThermometer> listObservers = new ArrayList<ObserverThermometer>();
	double temperatureC = 0.0;

	@Override
	public void attach(ObserverThermometer o) {
		if(! listObservers.contains(o)) { listObservers.add(o); }
	}

	@Override
	public void detach(ObserverThermometer o) {
		listObservers.remove(o);
	}

	@Override
	public void notifyObservers() {
		for( ObserverThermometer obs : listObservers) {
			obs.update(this);
		}
	}

	public double getTemperatureC() {
		return temperatureC;
	}

	public void setTemperatureC(double temperatureC) {
		this.temperatureC = temperatureC;
		notifyObservers();
	}
}
```

**DisplayAustralia**, which is the observer, will implement the **ObserverThermometer** class.

```java
public interface ObserverThermometer {
	public void update(SubjectThermometer obj);
}
public class DisplayAustralia implements ObserverThermometer {
	double temperatureC = 0.0;

	@Override
	public void update(SubjectThermometer obj) {
        this.temperatureC =  obj.getTemperatureC();
        display();
	}

	public void display() {
		System.out.printf("From DisplayAus: Temperature is %.2f C\n", temperatureC);
	}
}
```

We can clearly see that the _update()_ method in **DisplayAustralia** does not need to check for instances of Thermometer. Instead, it only takes in a **SubjectThermometer** object as parameter.

# Other Examples

- Graphical User Interfaces libraries
