# Formatting

## Use vertical separation for separate concepts

Each blank line is a visual cue that identifies a new and separate concept.

{% hint style="danger" %}
```php
Yii::import('application.modules.enquiry.models.*');
Yii::import('application.modules.shop.models.*');
$model = $this->loadModel();
```
{% endhint %}

{% hint style="success" %}
```php
Yii::import('application.modules.enquiry.models.*');

Yii::import('application.modules.shop.models.*');

$model = $this->loadModel();
```
{% endhint %}

## Use vertical density for tightly related concepts

Lines of code that are tightly related should appear vertically dense.

{% hint style="danger" %}
```php
/**
 * The HTML tag name for the widget container
 */
public $containerTagName;

/**
 * The CSS class for the widget container
 */
public $containerCssClass;
```
{% endhint %}

{% hint style="success" %}
```php
public $containerTagName;
public $containerCssClass;
```
{% endhint %}

## Place dependent functions vertically close

If one function calls another, they should be vertically close, and the caller should be above the callee.

{% hint style="success" %}
```php
public function getTypeLabel(int $type): string
{
    $options = $this->getTypeOptions();
    return isset($options[$type]) ? $options[$type] : null;
}

public function getTypeOptions(): array
{
    return [
        self::TYPE_CLIENT => 'Client',
        self::TYPE_EMPLOYEE => 'Employee'
    ];
}
```
{% endhint %}

## Place similar functions vertically close

If a group of functions perform a similar operation, they should be vertically close.

{% hint style="success" %}
```php
public function upgraClientToEmployee(): void
{
    $this->type = self::TYPE_EMPLOYEE;
    $this->update('type');
}

public function makeEmployeeToClient(): void
{
    $this->revokeAll();
    $this->type = self::TYPE_CLIENT;
    $this->update('type');
}
```
{% endhint %}

## Declare variables vertically close to their usage

{% hint style="success" %}
```php
$result = [];
foreach ($models as $i => $model) {
    $model->validate($attributes);
    foreach ($model->getErrors() as $attribute => $errors) {
        $result[Html::getInputId($model, "[$i]" . $attribute)] = $errors;
    }
}
```
{% endhint %}

Good functions are very short and local variables may appear at the top of each function.

{% hint style="success" %}
```php
public static function validate($model, $attributes = null)
{
    $result = [];

    if ($attributes instanceof Model) {
        $models = func_get_args();
        $attributes = null;
    } else {
        $models = [$model];
    }

    foreach ($models as $model) {
        $model->validate($attributes);
        foreach ($model->getErrors() as $attribute => $errors) {
            $result[Html::getInputId($model, $attribute)] = $errors;
        }
    }

    return $result;
}
```
{% endhint %}

## Use horizontal separation and density so that code reads nicely

The factors have no white space between them because they are high precedence. The terms are separated by white space because addition and subtraction are lower precedence.

{% hint style="success" %}
```php
public function calculate(Product $product): float
{
    return $product->price - ($product->price*($product->discount/100));
}
```
{% endhint %}
