# Error Handling

## Use wrapping to return a common exception type

We can simplify our code considerably by wrapping the API that we are calling and making sure that it returns a common exception type.

{% hint style="danger" %}
```php
try {   
    $port->open();   
} catch (DeviceResponseException $e) {   
    log('Device response exception', $e->getMessage());   
} catch (UnlockedException $e) {    
    log('Unlock exception', $e->getMessage());   
} catch (GMXError $e) {   
    log('GMX error', $e->getMessage());   
}
```
{% endhint %}

{% hint style="success" %}
```php
try {  
    $localPort = new LocalPort($port);      
    $localPort->open();  
} catch (PortDeviceFailure $e) {  
    log('Port device failure', $e->getMessage());  
}

class LocalPort   
{   
    private $port;  

    public function _construct(Port $port)  
    {  
        $this->port=$port;   
    }  

    public function open(): void  
    {   
        try {   
            $this->port->open();   
        } catch (DeviceResponseException $e) {   
            throw new PortDeviceFailure($e->getMessage());   
        } catch (UnlockedException $e) {   
            throw new PortDeviceFailure($e->getMessage());   
        } catch (GMXError $e) {   
            throw new PortDeviceFailure($e->getMessage());   
        }   
    }   
}
```
{% endhint %}

## Prefer exceptions to returning error codes

Returning error codes leads to deeply nested structures.

[Read more](https://erik-uus.gitbook.io/clean-code/key-principles/functions#prefer-exceptions-to-returning-error-codes)

## Put all code inside try block

If the keyword try exists in a function, it should be the very first word in the function and there should be nothing after the catch/finally blocks.

[Read more](https://erik-uus.gitbook.io/clean-code/key-principles/functions#put-all-code-inside-try-block)

