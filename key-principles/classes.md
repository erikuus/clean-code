# Classes

## Classes should have one responsibility—one reason to change

Consider a class that compiles and prints a report. Such a class can be changed for two reasons. First, the content of the report could change. Second, the format of the report could change. These two things change for different causes. These two aspects of the problem are two separate responsibilities, and should be in separate classes.

{% hint style="danger" %}
```php
interface Report
{
    public function compile(): self;
    public function printout(): string;
}
```
{% endhint %}

## Classes should be cohesive—cohesion results in many small classes

Classes should have a small number of instance variables. The more variables a method manipulates the more cohesive that method is to its class. Try to separate the variables and methods into many small classes such that the classes are more cohesive.

{% hint style="danger" %}
```php
class Person
{
    public $givenName;
    public $familyName;
    public $street;
    public $city;
    public $state;

    public function getFullName()
    {
        return $this->givenName.' '.$this->familyName;
    }    

    public function getFullAddress()
    {
        return $this->street.', '.$this->city.', '.$this->state;   
    }
}
```
{% endhint %}

{% hint style="success" %}
```php
class Name
{
    public $givenName;
    public $familyName;

    public function getFullName()
    {
        return $this->givenName.' '.$this->familyName;
    }
}

class Address
{
    public $street;
    public $city;
    public $state;

    public function format()
    {
        return $this->street.', '.$this->city.', '.$this->state;       
    }
}
```
{% endhint %}

## Follow the Law of Demeter

The Law of Demeter says that a method _f_ of a class _C_ should only call the methods of these:

1. _C_
2. An object created by _f_
3. An object held in an instance variable of _C_
4. An object passed as an argument to _f_

{% hint style="success" %}
```php
class CommentRepository extends Repository
{
    public function getReader(): DataReaderInterface
    {
        $sort = $this->getSort(); // rule 1
        return (new EntityReader())->withSort($sort); // rule 2
    }

    private function getSort(): Sort
    {
        return Sort::only(['id', 'created_at', 'post_id', 'user_id'])
            ->withOrder(['id' => 'asc']);
    }
}
```
{% endhint %}

{% hint style="success" %}
```php
class Context
{
    private TaxCalculator $taxCalculator;

    public function __construct(TaxCalculator $taxCalculator)
    {
        $this->taxCalculator = $taxCalculator;
    }

    public function calculateProduct(Product $product): void
    {
        $taxes = $this->taxCalculator->calculate($product); // rule 3
        $product->setTaxes($taxes); // rule 4
    }
}
```
{% endhint %}

## Use data abstraction

Abstraction gives the freedom to change implementation.

{% hint style="danger" %}
```php
interface Vehicle 
{
    public function getFuelTankCapacityInGallons(): float;
    public function getGallonsOfGasoline(): float;
}
```
{% endhint %}

{% hint style="success" %}
```php
interface Vehicle 
{
    public function getPercentFuelRemaining(): float;
}
```
{% endhint %}

## Incorporate new features by extending the class

Incorporate new features by extending the class, not by making modifications to existing class, to reduce the risk of change.

{% hint style="danger" %}
```php
class SqlCommand 
{
    protected $tableName;
    protected $columns;

    public function __construct(string $tableName, array $columns)
    {
        $this->tableName = $tableName;
        $this->columns = $columns;
    }

    public function insert(): string { }

    public function select(): string { }
}
```
{% endhint %}

{% hint style="success" %}
```php
abstract class SqlCommand 
{
    protected $tableName;
    protected $columns;

    public function __construct(string $tableName, array $columns)
    {
        $this->tableName = $tableName;
        $this->columns = $columns;
    }

    abstract public function generate(): string;
}

class InsertCommand extends SqlCommand 
{
    public function generate(): string { }
}

class SelectCommand extends SqlCommand
{
    public function generate(): string { }
}
```
{% endhint %}

## Treat the Active Record as a data structure

Create separate objects that contain the business rules. Data structures expose their data. Objects expose functions that operate on data.

{% hint style="success" %}
```php
class Comment
{
    private $id;
    private $author;
    private $text;
    private $createdAt;
    private $image;

    public function getId(): ?int
    {
        return $this->id;
    }

    public function getAuthor(): ?string
    {
        return $this->author;
    }

    public function setAuthor(string $author): self
    {
        $this->author = $author;
        return $this;
    }

    /* and so on */
}

class CommentRepository extends ServiceEntityRepository
{
    public const PAGINATOR_PER_PAGE = 2;

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
}
```
{% endhint %}
