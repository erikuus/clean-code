# Error Handling

## Use wrapping to return a common exception type

We can simplify our code considerably by wrapping the API that we are calling and making sure that it returns a common exception type.

{% hint style="danger" %}
`try { port.open(); } catch (DeviceResponseException e) { reportPortError(e); logger.log(“Device response exception”, e); } catch (ATM1212UnlockedException e) { reportPortError(e); logger.log(“Unlock exception”, e); } catch (GMXError e) { reportPortError(e); logger.log(“Device response exception”); }`
{% endhint %}

{% hint style="success" %}
`public class LocalPort { public void open() { try { port.open(); } catch (DeviceResponseException e) { throw new PortDeviceFailure(e); } catch (ATM1212UnlockedException e) { throw new PortDeviceFailure(e); } catch (GMXError e) { throw new PortDeviceFailure(e); } } }`

`try {  
    LocalPort.open();  
} catch (PortDeviceFailure e) {  
    reportError(e);  
    logger.log(e.getMessage(), e);  
}`
{% endhint %}

## Prefer exceptions to returning error codes

Returning error codes leads to deeply nested structures.

{% page-ref page="functions.md" %}

## Put all code inside try block

If the keyword try exists in a function, it should be the very first word in the function and there should be nothing after the catch/finally blocks.

{% page-ref page="functions.md" %}





