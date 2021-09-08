# Meaningful Names

## Use intention-revealing names

What does the list represent?

{% hint style="danger" %}
`$theList`
{% endhint %}

{% hint style="success" %}
`$gameBoard`
{% endhint %}

## Avoid disinformation

Of what type is the account list? String? Array of strings? Array of objects?

{% hint style="danger" %}
`$accountList`
{% endhint %}

{% hint style="success" %}
`$accounts`
{% endhint %}

## Avoid similar shapes

How long does it take to spot the subtle difference?

{% hint style="danger" %}
`class ControllerForEfficientHandlingOfStrings    
class ControllerForEfficientStorageOfStrings`
{% endhint %}

## Make meaningful distinctions

How do these different names convey different meanings?

{% hint style="danger" %}
`class Product    
class ProductInfo    
class ProductData`
{% endhint %}

## Use pronounceable names

How can you discuss it without sounding like an idiot?

{% hint style="danger" %}
`class CstmrRcrd`
{% endhint %}

{% hint style="success" %}
`class Customer`
{% endhint %}

## Add context by using prefixes

What does the state represent? Condition or country?

{% hint style="danger" %}
`$state`
{% endhint %}

{% hint style="success" %}
`$addressState`
{% endhint %}

{% hint style="info" %}
A better solution is to create a class named Address. If you need to differentiate between MAC addresses, port addresses, and Web addresses, consider PostalAddress, MAC, and URI.
{% endhint %}

## Adjust the length of a name to the size of its scope

Is it obvious outside the class body that WD is an acronym for work days per week?

{% hint style="danger" %}
`const WD`
{% endhint %}

{% hint style="success" %}
`const WORK_DAYS_PER_WEEK`
{% endhint %}

{% hint style="info" %}
If a variable or constant might be seen or used in multiple places in a body of code, it is imperative to give it a search-friendly and meaningful name.
{% endhint %}

## Avoid using the same name for different purposes

What does add mean? Concate strings? Insert a record in a table? Append a value to the end of an array?

{% hint style="danger" %}
`function add($value)    
function add($value)    
function add($value)`
{% endhint %}

{% hint style="success" %}
`function concate($value)    
function insert($value)    
function append($value)`
{% endhint %}

## Use problem domain names

What does the term "document" mean in the archives domain? Are photos considered documents?

{% hint style="danger" %}
`$document`
{% endhint %}

{% hint style="success" %}
`$record`
{% endhint %}

## Methods should have verb names

{% hint style="success" %}
`function postPayment()    
function deletePage()    
function save()`
{% endhint %}

## Classes should have noun names

{% hint style="success" %}
`class Customer    
class WikiPage    
class Account`
{% endhint %}

Avoid these words in the name of a class:

{% hint style="danger" %}
`Manager    
Processor    
Data    
Info`
{% endhint %}

