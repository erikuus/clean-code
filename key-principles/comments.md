# Comments

## Explain yourself in code

Rather than spend your time writing the comments that explain the mess you’ve made, spend it cleaning that mess.

{% hint style="danger" %}
`// Check to see if the employee is eligible for full benefits  
   
if (($employee->flags & HOURLY_FLAG) && ($employee->age > 65))`
{% endhint %}

{% hint style="success" %}
`if ($employee->isEligibleForFullBenefits())`
{% endhint %}

## Choose your words carefully

Any comment that forces you to look in another module for the meaning of that comment has failed.

{% hint style="danger" %}

{% endhint %}

## Avoid redundant comments

A comment is redundant if it describes something that adequately describes itself. Comments should say things that the code cannot say for itself.

{% hint style="danger" %}
`// create directory recursively and save file content  
  
mkdir($uploadDirectory, 0777, true);  
file_put_contents($uploadDirectory.'/'.$filename, $content));`
{% endhint %}

## There is no need for attributions and journal comments

## Give up mandated comments

## Don't make noise

## Avoid position markers

## Avoid closing brace comments

## Avoid HTML comments

## Don’t offer systemwide information

## Don't give too much information

