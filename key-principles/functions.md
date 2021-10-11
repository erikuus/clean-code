# Functions

## Functions should be small

Functions should hardly ever be 20 lines long.

{% hint style="success" %}
```php
public function getPaginator(Image $image, int $offset): Paginator
{
    $query = $this->createQueryBuilder('c')
        ->andWhere('c.image = :image')
        ->setParameter('image', $image)
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
public function pay(EmployeeRepository $employeeRepository): void
{
    foreach ($employeeRepository->findAll() as $employee) {
        if ($employee->isPayday()) {
            $pay = $employee->calculatePay();
            $employee->deliverPay($pay);
        }
    }
}
```
{% endhint %}

{% hint style="success" %}
```php
public function pay(EmployeeRepository $employeeRepository): void
{
    for ($employeeRepository->findAll() as $employee) {
        $this->payIfNecessary($employee);
    }
}

protected function payIfNecessary(Employee $employee): void
{
    if ($employee->isPayday())
        $this->calculateAndDeliverPay($employee);
}

protected function calculateAndDeliverPay(Employee $employee): void
{
    $pay = $employee->calculatePay();
    $employee->deliverPay($pay);
}
```
{% endhint %}

## Follow the stepdown rule

We want every function to be followed by those at the next level of abstraction so that we can read code from top to bottom.

{% hint style="success" %}
```php
// First level of abstraction
public function pay(EmployeeRepository $employeeRepository): void
{
    for ($employeeRepository->findAll() as $employee) {
        $this->payIfNecessary($employee);
    }
}

// Second level of abstraction
protected function payIfNecessary(Employee $employee): void
{
    if ($employee->isPayday())
        $this->calculateAndDeliverPay($employee);
}

// Third level of abstraction
protected function calculateAndDeliverPay(Employee $employee): void
{
    $pay = $employee->calculatePay();
    $employee->deliverPay($pay);
}
```
{% endhint %}

## Avoid nested structures

The blocks within if statements, else statements, while statements should ideally be one line long. That line can be a function call.

{% hint style="danger" %}
```php
public function authenticate(array $data, array $options): void
{
    $user = $this->findUser($data, $options);

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

{% hint style="success" %}
```php
public function authenticate(array $data, array $options): void
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

{% hint style="danger" %}
```php
public function create(int $id=null, string $csv=null, string $token=null)
```
{% endhint %}

{% hint style="info" %}
Arguments are even harder from a testing point of view. Imagine the difficulty of writing all the test cases to ensure that all the various combinations of arguments work properly.
{% endhint %}

## Avoid flag arguments

Function with flag argument does more than one thing. It does one thing if the flag is true and another if the flag is false!

{% hint style="danger" %}
```php
public function checkRoom(array $rooms, bool $in): bool
{
    return $in ?
        in_array($this->room, $rooms) : !in_array($this->room, $rooms);
}
```
{% endhint %}

{% hint style="success" %}
```php
public function isInOneOfRooms(array $rooms): bool
{
    return in_array($this->room, $rooms);
}

public function isNotInAnyRooms(array $rooms): bool
{
    return !in_array($this->room, $rooms);
}
```
{% endhint %}

## Have No Side Effects

The side effect is the call to session_start(). The checkPassword function, by its name, says that it checks the password. The name does not imply that it initializes the session.

{% hint style="danger" %}
```php
public function checkPassword(string $password): bool
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
    $taxes = $this->taxCalculator->calculate($product);
    $product->setTaxes($taxes);
    return $taxes;
}
```
{% endhint %}

{% hint style="success" %}
```php
public function calculateProduct(Product $product): void
{
    $taxes = $this->taxCalculator->calculate($product);
    $product->setTaxes($taxes);
}
```
{% endhint %}

## Prefer exceptions to returning error codes

Returning error codes from command functions leads to deeply nested structures.

{% hint style="danger" %}
```php
public function remoteLogin(string $data): void
{
    $jsonData = $this->decrypt($data);

    $identity = new UserIdentity();
    $identity->authenticate($jsonData);

    if ($identity->errorCode == UserIdentity::ERROR_NONE) {
        $this->login($identity->getUser());
        $this->redirect($this->redirectUrl);
    } elseif ($identity->errorCode == UserIdentity::ERROR_UNAUTHORIZED) {
        throw new ForbiddenHttpException();
    } else {
        switch ($identity->errorCode) {
            case UserIdentity::ERROR_INVALID:
                $this->error('Invalid login request');
                break;
            case UserIdentity::ERROR_EXPIRED:
                $this->error('Expired login request');
                break;
            case UserIdentity::ERROR_SYNC:
                $this->error('Failed user data sync');
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
public function remoteLogin(string $data): void
{
    try {
        $jsonData = $this->decrypt($data);
        $identity = new UserIdentity();
        $identity->authenticate($jsonData);
        $this->login($identity->getUser());
        $this->redirect($this->redirectUrl);
    } catch (AccessDeniedException $e) {
        throw new ForbiddenHttpException();        
    } catch (\Exception $e) {
        $this->error($e->getMessage());
        throw new BadRequestHttpException();
    }
}
```
{% endhint %}

## Put all code inside try block

If the keyword try exists in a function, it should be the very first word in the function and there should be nothing after the catch/finally blocks.

{% hint style="danger" %}
```php
public function remoteLogin(string $data): void
{
    $jsonData = $this->decrypt($data);
    $identity = new UserIdentity();

    try {
        $identity->authenticate($jsonData);
    } catch (AccessDeniedException $e) {
        throw new ForbiddenHttpException();        
    } catch (\Exception $e) {
        $this->error($e->getMessage());
        throw new BadRequestHttpException();
    }

    $this->login($identity->getUser());
    $this->redirect($this->redirectUrl);    
}
```
{% endhint %}

{% hint style="success" %}
```php
public function remoteLogin(string $data): void
{
    try {
        $jsonData = $this->decrypt($data);
        $identity = new UserIdentity();
        $identity->authenticate($jsonData);
        $this->login($identity->getUser());
        $this->redirect($this->redirectUrl);
    } catch (AccessDeniedException $e) {
        throw new ForbiddenHttpException();        
    } catch (\Exception $e) {
        $this->error($e->getMessage());
        throw new BadRequestHttpException();
    }
}
```
{% endhint %}

## Write, test and refine

> When I write functions, they come out long and complicated. But I also have unit tests that cover every one of those clumsy lines of code. So then I massage and refine that code, splitting out functions, changing names, eliminating duplication.
>
> — Robert C. Martin
