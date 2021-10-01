# Classes

## Classes should have one responsibility—one reason to change

Consider a class that compiles and prints a report. Such a class can be changed for two reasons. First, the content of the report could change. Second, the format of the report could change. These two things change for different causes. These two aspects of the problem are two separate responsibilities, and should be in separate classes.

{% hint style="danger" %}
```php
public interface Report
{
	public function compile(): self;
	public function print(): string;
}
```
{% endhint %}

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
public interface Vehicle 
{
	public function getPercentFuelRemaining(): float;
}
```
{% endhint %}

## Classes should be cohesive — cohesion results in many small classes

Classes should have a small number of instance variables. The more variables a method manipulates the more cohesive that method is to its class.

{% hint style="success" %}
```php

```
{% endhint %} 

{% hint style="info" %}
Try to separate the variables and methods into many small classes such that the classes are more cohesive.
{% endhint %}

## Follow the law of demeter

The Law of Demeter says that a method *f* of a class *C* should only call the methods of these:

1. *C*
2. An object created by *f*
3. An object passed as an argument to *f*                                             
4. An object held in an instance variable of *C*

{% hint style="success" %}
```php
class UserRepository extends Select\Repository implements IdentityRepositoryInterface
{
    public function findByLogin(string $login): ?IdentityInterface
    {
        return $this->findIdentityBy('login', $login); // rule 1
    }

    private function findIdentityBy(string $field, string $value): ?IdentityInterface
    {
        return $this->findOne([$field => $value]);
    }    

    public function findAll(array $scope = [], array $orderBy = []): DataReaderInterface
    {
        return new EntityReader($this->select()->where($scope)->orderBy($orderBy)); // rule 2
    }
}

class Context
{
    private TaxCalculatorStrategy $taxCalculatorStrategy;

    public function __construct(TaxCalculatorStrategy $taxCalculatorStrategy)
    {
        $this->taxCalculatorStrategy = $taxCalculatorStrategy;
    }

    public function calculateProduct(Product $product): void
    {
        
        $taxes = $this->taxCalculatorStrategy->calculate($product); // rule 4
        $product->setTaxes($taxes); // rule 3
    }
}
```
{% endhint %}

## Treat the Active Record as a data structure

Create separate objects that contain the business rules.

{% hint style="success" %}
```php
class Comment
{
    private $id;
    private $author;
    private $text;
    private $createdAt;
    private $conference;

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

    public function getText(): ?string
    {
        return $this->text;
    }

    public function setText(string $text): self
    {
        $this->text = $text;
        return $this;
    }

    public function getCreatedAt(): ?\DateTimeInterface
    {
        return $this->createdAt;
    }

    public function setCreatedAt(\DateTimeInterface $createdAt): self
    {
        $this->createdAt = $createdAt;
        return $this;
    }

    public function getConference(): ?Conference
    {
        return $this->conference;
    }

    public function setConference(?Conference $conference): self
    {
        $this->conference = $conference;
        return $this;
    }
}

class CommentRepository
{
    public const PAGINATOR_PER_PAGE = 2;

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
}

```
{% endhint %} 

{% hint style="info" %}
Data structure expose their data. Objects expose functions that operate on data.
{% endhint %}

## Incorporate new features by extending the class

