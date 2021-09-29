# Functions

## Functions should be small

Functions should hardly ever be 20 lines long

{% hint style="success" %}
```php
public function getPaginator(Article $article, int $offset): Paginator
{
    $query = $this->createQueryBuilder('c')
        ->andWhere('c.article = :article')
        ->setParameter('article', $article)
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

Function is doing more than "one thing" if you can extract another function from it with a name that is not merely a restatement of its implementation.

{% hint style="danger" %}
```php
public function authenticate($data, $options)
{
    $user = User::findOne([
        $options['id'] => $data['id']
    ]);

    if ($user === null) {
        if ($options['enableCreate']) {
            $user = new User();
            foreach ($options['attributes'] as $key => $attribute) {
                $user->{$attribute} = $data[$key];
            }
            if (!$user->save()) {
                throw new SaveFailedException();
            }
        } else {
            throw new AccessDeniedException();
        }
    } elseif ($options['enableUpdate']) {
        foreach ($options['attributes'] as $key => $attribute) {
            $user->{$attribute} = $data[$key];
        }
        if (!$user->save()) {
            throw new SaveFailedException();
        }
    }
    $this->setUser($user);
}
```
{% endhint %}

If a function does only those steps that are one level below the stated name of the function, then the function is doing one thing.

{% hint style="success" %}
```php
public function authenticate($data, $options)
{
    $user = $this->findUser($data, $options);
    if ($user === null) {
        if ($options['enableCreate']) {
            $user = $this->createUser($data, $options);
        } else {
            throw new AccessDeniedException();
        }
    } elseif ($options['enableUpdate']) {
        $user = $this->updateUser($user, $data, $options);
    }
    $this->setUser($user);
}

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
        throw new CHttpException(404);
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
public function createIALOrder()
```
{% endhint %}

{% hint style="success" %}
```php
public function createInterArchivalLoanOrder()
```
{% endhint %}

## Use few arguments

The ideal number of arguments for a function is zero. Three arguments should be avoided where possible.

{% hint style="info" %}
Arguments are even harder from a testing point of view. Imagine the difficulty of writing all the test cases to ensure that all the various combinations of arguments work properly.
{% endhint %}

{% hint style="danger" %}
```php
public function createApplication($orderId=null, $batchIds=null, $token=null)
```
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
public function calculateProduct(Product $product): float
{
    $taxes = $this->calculateTaxes($product);
    $product->setTaxes($taxes);
    return $taxes;
}
```
{% endhint %}

{% hint style="success" %}
```php
public function calculateProduct(Product $product): void
{
    $taxes = $this->calculateTaxes($product);
    $product->setTaxes($taxes);
}
```
{% endhint %}

## Prefer exceptions to returning error codes

Returning error codes from command functions leads to deeply nested structures.

{% hint style="danger" %}
```php
public function run()
{
    $jsonData = Yii::$app->securityManager->decrypt($_POST['data']);

    $identity = new UserIdentity();
    $identity->authenticate($jsonData, $this->authOptions);

    if ($identity->errorCode == UserIdentity::ERROR_NONE) {
        Yii::$app->user->login($identity->getUser());
        $this->controller->redirect($this->redirectUrl);
    } elseif ($identity->errorCode == UserIdentity::ERROR_UNAUTHORIZED) {
        throw new ForbiddenHttpException();
    } else {
        switch ($identity->errorCode) {
            case UserIdentity::ERROR_INVALID_DATA:
                Yii::error('Invalid VAU login request');
                break;
            case UserIdentity::ERROR_EXPIRED_DATA:
                Yii::error('Expired VAU login request');
                break;
            case UserIdentity::ERROR_SYNC_DATA:
                Yii::error('Failed VAU user data sync');
                break;
        }
        throw new BadRequestHttpException();
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
        $jsonData = Yii::$app->securityManager->decrypt($_POST['data']);
        $identity = new UserIdentity();
        $identity->authenticate($jsonData, $this->authOptions);
        Yii::$app->user->login($identity->getUser());
        $this->controller->redirect($this->redirectUrl);
    } catch (VauAccessDeniedException $e) {
        throw new ForbiddenHttpException();        
    } catch (\Exception $e) {
        Yii::error($e->getMessage());
        throw new BadRequestHttpException();
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
    $jsonData = Yii::$app->securityManager->decrypt($_POST['data']);
    $identity = new UserIdentity();

    try {
        $identity->authenticate($jsonData, $this->authOptions);
    } catch (VauAccessDeniedException $e) {
        throw new ForbiddenHttpException();        
    } catch (\Exception $e) {
        Yii::error($e->getMessage());
        throw new BadRequestHttpException();
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
        $jsonData = Yii::$app->securityManager->decrypt($_POST['data']);
        $identity = new UserIdentity();
        $identity->authenticate($jsonData, $this->authOptions);
        Yii::$app->user->login($identity->getUser());
        $this->controller->redirect($this->redirectUrl);
    } catch (VauAccessDeniedException $e) {
        throw new ForbiddenHttpException();        
    } catch (\Exception $e) {
        Yii::error($e->getMessage());
        throw new BadRequestHttpException();
    }
}
```
{% endhint %}

## Write, test and refine

> When I write functions, they come out long and complicated. But I also have unit tests that cover every one of those clumsy lines of code. So then I massage and refine that code, splitting out functions, changing names, eliminating duplication.
>
> — Robert C. Martin



