# Functions

## Functions should be small

Functions should hardly ever be 20 lines long

{% hint style="success" %}
```php
public function getCommentPaginator(Conference $conference, int $offset): Paginator
{
    $query = $this->createQueryBuilder('c')
        ->andWhere('c.conference = :conference')
        ->setParameter('conference', $conference)
        ->orderBy('c.createdAt', 'DESC')
        ->setMaxResults(self::PAGINATOR_PER_PAGE)
        ->setFirstResult($offset)
        ->getQuery()
    ;

    return new Paginator($query);
}
```
{% endhint %}

## Functions should do one thing

If a function does only those steps that are one level below the stated name of the function, then the function is doing one thing. Function is doing more than "one thing" if you can extract another function from it with a name that is not merely a restatement of its implementation.

{% hint style="danger" %}
```php

```
{% endhint %}

{% hint style="success" %}
```php

```
{% endhint %}

## Avoid nested structures

The blocks within if statements, else statements, while statements should be one line long. That line can be a function call.

{% hint style="danger" %}
```php

```
{% endhint %}

{% hint style="success" %}
```php

```
{% endhint %}

## Follow the stepdown rule

We want every function to be followed by those at the next level of abstraction so that we can read code from top to bottom.

{% hint style="success" %}
```php
public function run()
{
    if (Yii::app()->request->isPostRequest) {
        $model = $this->loadModel();
        $move = Yii::app()->request->getQuery('move');

        if ($move == 'up') {
            $model->moveUp();
        } elseif ($move == 'down') {
            $model->moveDown();
        }
    } else {
        throw new CHttpException(400);
    }
}

protected function loadModel()
{
    $model = null;

    if (isset($_GET['id'])) {
        $model = $this->getModel()->findbyPk($_GET['id']);
    }

    if ($model === null) {
        throw new CHttpException(404,'The requested page does not exist.');
    } else {
        return $model;
    }
}

protected function getModel()
{
    return CActiveRecord::model($this->modelName);
}
```
{% endhint %}

## Use descriptive names

Don’t be afraid to make a name long. A long descriptive name is better than a short enigmatic name. A long descriptive name is better than a long descriptive comment.

{% hint style="danger" %}
```php

```
{% endhint %}

{% hint style="success" %}
```php

```
{% endhint %}

## Use few arguments

The ideal number of arguments for a function is zero. Three arguments should be avoided where possible.

{% hint style="danger" %}
```php

```
{% endhint %}

{% hint style="success" %}
```php

```
{% endhint %}

{% hint style="info" %}
Arguments are even harder from a testing point of view. Imagine the difficulty of writing all the test cases to ensure that all the various combinations of arguments work properly.
{% endhint %}

## Avoid flag arguments

Function does more than one thing. It does one thing if the flag is true and another if the flag is false!

{% hint style="success" %}
```php

```
{% endhint %}

## Have No Side Effects

The side effect is the call to session_start(). The checkPassword function, by its name, says that it checks the password. The name does not imply that it initializes the session.

{% hint style="danger" %}
```php
public function checkPassword($username, $password)
{
    session_start();
```
{% endhint %}

## Separate command from query

Functions should either do something or answer something, but not both.

{% hint style="danger" %}
```php

```
{% endhint %}

{% hint style="success" %}
```php

```
{% endhint %}

## Prefer exceptions to returning error codes

Returning error codes from command functions leads to deeply nested structures.

{% hint style="danger" %}
```php
public function run()
{
        $jsonData = Yii::$app->vauSecurityManager->decrypt($_POST['data']);

        $identity = new VauUserIdentity();
        $identity->authenticate($jsonData, $this->authOptions);

        if ($identity->errorCode == VauUserIdentity::ERROR_NONE) {
            Yii::$app->user->login($identity->getUser());
            $this->controller->redirect($this->redirectUrl);
        } elseif ($identity->errorCode == VauUserIdentity::ERROR_UNAUTHORIZED) {
            throw new ForbiddenHttpException('You do not have the proper credential to access this page.');
        } else {
            switch ($identity->errorCode) {
                case VauUserIdentity::ERROR_INVALID_DATA:
                    Yii::error('Invalid VAU login request');
                    break;
                case VauUserIdentity::ERROR_EXPIRED_DATA:
                    Yii::error('Expired VAU login request');
                    break;
                case VauUserIdentity::ERROR_SYNC_DATA:
                    Yii::error('Failed VAU user data sync');
                    break;
            }
            throw new BadRequestHttpException('Bad request. Please do not repeat this request again.');
        }
}
```
{% endhint %}

If you use exceptions instead of returned error codes, then the error processing code can be separated from the happy path code.

{% hint style="success" %}
```php
public function run()
{
    try {
        $jsonData = Yii::$app->vauSecurityManager->decrypt($_POST['data']);
        $identity = new VauUserIdentity();
        $identity->authenticate($jsonData, $this->authOptions);
        Yii::$app->user->login($identity->getUser());
        $this->controller->redirect($this->redirectUrl);
    } catch (VauAccessDeniedException $e) {
        throw new ForbiddenHttpException('You do not have the proper credential to access this page.');        
    } catch (\Exception $e) {
        Yii::error($e->getMessage());
        throw new BadRequestHttpException('Bad request. Please do not repeat this request again.');
    }
}
```
{% endhint %}

## Put all code inside try block

If the keyword try exists in a function, it should be the very first word in the function and that there should be nothing after the catch/finally blocks.

{% hint style="danger" %}
```php
public function run()
{
    $jsonData = Yii::$app->vauSecurityManager->decrypt($_POST['data']);
    $identity = new VauUserIdentity();

    try {
        $identity->authenticate($jsonData, $this->authOptions);
    } catch (VauAccessDeniedException $e) {
        throw new ForbiddenHttpException('You do not have the proper credential to access this page.');        
    } catch (\Exception $e) {
        Yii::error($e->getMessage());
        throw new BadRequestHttpException('Bad request. Please do not repeat this request again.');
    }

    Yii::$app->user->login($identity->getUser());
    $this->controller->redirect($this->redirectUrl);    
}
```
{% endhint %}

{% hint style="success" %}
```php
public function run()
{
    try {
        $jsonData = Yii::$app->vauSecurityManager->decrypt($_POST['data']);
        $identity = new VauUserIdentity();
        $identity->authenticate($jsonData, $this->authOptions);
        Yii::$app->user->login($identity->getUser());
        $this->controller->redirect($this->redirectUrl);
    } catch (VauAccessDeniedException $e) {
        throw new ForbiddenHttpException('You do not have the proper credential to access this page.');        
    } catch (\Exception $e) {
        Yii::error($e->getMessage());
        throw new BadRequestHttpException('Bad request. Please do not repeat this request again.');
    }
}
```
{% endhint %}

## Write, test and refine

> When I write functions, they come out long and complicated. But I also have unit tests that cover every one of those clumsy lines of code. So then I massage and refine that code, splitting out functions, changing names, eliminating duplication.
>
> — Robert C. Martin



