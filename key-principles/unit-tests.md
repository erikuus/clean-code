# Unit Tests

## Keep tests clean

{% hint style="info" %}
Test code should be maintained to the same standards of quality as their production code.
{% endhint %}

## Focus on readability

Readability is even more important in unit tests than it is in production code.

{% hint style="danger" %}
```php
function testValidate()
```
{% endhint %}

{% hint style="success" %}
```php
function testValidateReturnsFalseIfEmployeeIdIsMissing()
```
{% endhint %}

## Use single concept per test

The number of asserts in a test ought to be minimized. Test a single concept in each test function.

{% hint style="danger" %}
```php
function testValidate()   
{   
    $form = new Form();  

    $model = new Whitelist();  
    $model->setAttributes($form->get('noPattern'));  
    $this->assertFalse($model->validate());  

    $model = new Whitelist();  
    $model->setAttributes($form->get('noEmployeeId'));  
    $this->assertFalse($model->validate());  

    $model = new Whitelist();  
    $model->setAttributes($form->get('validForm'));  
    $this->assertTrue($model->validate());  
}
```
{% endhint %}

{% hint style="success" %}
```php
function testValidateReturnsFalseIfEmployeeIdIsMissing()  
{  
    $form = new Form();  

    $model = new Whitelist();  
    $model->setAttributes($form->get('noEmployeeId'));  
    $this->assertFalse($model->validate());  
}
```
{% endhint %}

## Follow five rules that form the acronym F.I.R.S.T.

### 🏁 Fast

Tests should be fast. They should run quickly. When tests run slow, you won’t want to run them frequently.

### 👤 Independent

Tests should not depend on each other. One test should not set up the conditions for the next test. You should be able to run each test independently and run the tests in any order you like.

### 💻 Repeatable

Tests should be repeatable in any environment. You should be able to run the tests in the production environment, in the QA environment, and on your laptop while riding home on the train without a network. If your tests aren’t repeatable in any environment, then you’ll always have an excuse for why they fail.

### ✔ Self-Validating

The tests should have a boolean output. Either they pass or fail. You should not have to read through a log file to tell whether the tests pass. You should not have to manually compare two different text files to see whether the tests pass.

### 🕗 Timely

The tests need to be written in a timely fashion. Unit tests should be written just before the production code that makes them pass. If you write tests after the production code, then you may find the production code to be hard to test.

