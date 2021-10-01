# Comments

## Explain yourself in code

Rather than spend your time writing the comments that explain the mess you’ve made, spend it cleaning that mess.

{% hint style="danger" %}
```php
// Check to see if the employee is eligible for full benefits  

if ($employee->flag && ($employee->age > 65))
```
{% endhint %}

{% hint style="success" %}
```php
if ($employee->isEligibleForFullBenefits())
```
{% endhint %}

## Avoid redundant comments

A comment is redundant if it describes something that adequately describes itself. Comments should say things that the code cannot say for itself.

{% hint style="danger" %}
```php
// create directory recursively and save file content  

mkdir($uploadDirectory, 0777, true);  
file_put_contents($uploadDirectory.'/'.$filename, $content));
```
{% endhint %}

## Give up mandated comments

It is just plain silly to have a rule that says that every function and variable must have a comment. Comments like this just clutter up the code.

{% hint style="danger" %}
```php
/**
 * Returns the static model of the specified AR class.
 * @return Order the static model class
 */
public static function model($className=__CLASS__)
{
    return parent::model($className);
}
```
{% endhint %}

## Don't make noise

Noise comments restate the obvious and provide no new information.

{% hint style="danger" %}
```php
/**
 * Check whether it is automated request
 * @return boolean whether it is automated request
 */
public function isAutoRequest(): bool
{
    return isset($_REQUEST['VK_AUTO']) && $_REQUEST['VK_AUTO'] == 'Y';
}
```
{% endhint %}

## Avoid closing brace comments

If you find yourself wanting to mark your closing braces, try to shorten your functions instead.

{% hint style="danger" %}
```php
        $newline .= $c;
    } // end of for
    $output .= $newline . $eol;
} // end of while
return $output;
```
{% endhint %}

## Avoid position markers

There are rare times when they make sense, but in general they are clutter that should be eliminated.

{% hint style="danger" %}
```php
/////////////////////////////////////////////////
// PROPERTIES, PRIVATE AND PROTECTED
/////////////////////////////////////////////////

private $smtp_conn;
private $error;
private $helo_rply;
```
{% endhint %}

## Avoid HTML comments

It makes the comments hard to read in the one place where they should be easy to read—the editor/IDE.

{% hint style="danger" %}
```php
/**
 * To use this widget, you may insert the following code in a view:
 * <pre>
 * $this->widget('ext.widgets.password.XPasswordStrength', [
 *     'model' => $model,
 *     'attribute' => 'password'
 * ]);
 * </pre>
 */
```
{% endhint %}

## There is no need for attributions and journal comments

Nowadays we have source code control systems that does it for us.

{% hint style="danger" %}
```php
/**
 * 2016-03-19
 * New Method {@link  getReturnUrlRoute()}
 * New Method {@link  getReturn2Url()}
 * Updated Method {@link  getReturnUrl()}
 *
 * @author Erik Uus <erik.uus@gmail.com>
 */
```
{% endhint %}

## Don't give too much information

Don’t put interesting historical discussions or irrelevant descriptions of details into your comments.

{% hint style="danger" %}
```php
// The encoding process represents 24-bit groups of input bits as
// output strings of 4 encoded characters. Proceeding from left to right
// a 24-bit input group is formed by concatenating 3 8-bit input groups.
// These 24 bits are then treated as 4 concatenated 6-bit groups ...
```
{% endhint %}

## Don’t offer systemwide information

If you must write a comment, then make sure it describes the code it appears near.

{% hint style="danger" %}
```php
// Port on which app would run defaults to 8082
```
{% endhint %}
