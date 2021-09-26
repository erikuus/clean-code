# Formatting

## Use vertical separation for separate concepts

Each blank line is a visual cue that identifies a new and separate concept.

{% hint style="success" %}
`package fitnesse.wikitext.widgets;`

`import java.util.regex.*;`

`public class BoldWidget extends ParentWidget {`
{% endhint %}

{% hint style="danger" %}
`package fitnesse.wikitext.widgets;   
import java.util.regex.*;   
public class BoldWidget extends ParentWidge {`
{% endhint %}

## Use vertical density for tightly related concepts

Lines of code that are tightly related should appear vertically dense.

{% hint style="success" %}
`private m_className;   
private m_properties;`
{% endhint %}

{% hint style="danger" %}
`/**  
 * The class name of the reporter listener  
 */  
private m_className;  
  
/**  
 * The properties of the reporter listener  
 */  
private m_properties;`
{% endhint %}

## Place dependent functions vertically close

If one function calls another, they should be vertically close, and the caller should be above the callee.



## Place similar functions vertically close

If a group of functions perform a similar operation, they should be vertically close.

## Declare variables vertically close to their usage

Good functions are very short and local variables may appear at the top of each function.

## Use horizontal separation and density so that code reads nicely

The factors have no white space between them because they are high precedence. The terms are separated by white space because addition and subtraction are lower precedence.

{% hint style="success" %}
`return (-b + Math.sqrt(determinant)) / (2*a);`
{% endhint %}

{% hint style="danger" %}
`return (-b+Math.sqrt(determinant))/(2*a);`
{% endhint %}

{% hint style="danger" %}
`return (-b + Math.sqrt(determinant)) / (2 * a);`
{% endhint %}

