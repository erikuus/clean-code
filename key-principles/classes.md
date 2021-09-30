# Classes

## Classes should have one responsibility—one reason to change

Consider a class that compiles and prints a report. Such a class can be changed for two reasons:

- First, the content of the report could change. 
- Second, the format of the report could change. 

These two things change for different causes. These two aspects of the problem are two separate responsibilities, and should be in separate classes.

## Classes should be cohesive—cohesion results in many small classes

Classes should have a small number of instance variables. The more variables a method manipulates the more cohesive that method is to its class. 

{% hint style="info" %}
Try to separate the variables and methods into many small classes such that the classes are more cohesive.
{% endhint %}

## Follow the law of demeter

The Law of Demeter says that a method *f* of a class *C* should only call the methods of these:

- *C*
- An object created by *f*
- An object passed as an argument to *f*                                             
- An object held in an instance variable of C

## Treat the Active Record as a data structure

Create separate objects that contain the business rules.

{% hint style="info" %}
Data structure expose their data. Objects expose functions that operate on data.
{% endhint %}

## Incorporate new features by extending the class



## Use data abstraction

Abstraction gives the freedom to change implementation.

{% hint style="danger" %}
```php
public interface Vehicle 
{
	public function getFuelTankCapacityInGallons(): float;
	public function getGallonsOfGasoline(): float;
}
```
{% endhint %}

{% hint style="success" %}
```php
public interface Vehicle {
	public function getPercentFuelRemaining(): float;
}
```
{% endhint %}

