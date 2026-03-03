
> Allows an attacker to circumvent the [[SOP - Same Origin Policy]] and masquerade as a victim user, to carry out any actions that the user is able to perform, and to access any of the user's data.
> 
> XSS works by manipulating a vulnerable web site so that it returns malicious JavaScript to users, usually by injecting a payload that causes the browser to execute some arbitrary JavaScript.

# Impact
The actual impact of an XSS attack generally depends on the nature of the application, its functionality and data, and the status of the compromised user. For example:

- If all users are anonymous and all information is public, the impact will often be minimal.
- In an application holding sensitive data, the impact will usually be serious.
- If the compromised user has elevated privileges within the application, then the impact will generally be critical.

# Types of XSS:

## Reflected XSS:
It arises when an application receives data in an HTTP request and includes that data within the immediate response in an unsafe way.

The need for an external delivery mechanism for the attack (crafted URL) means that the impact is generally less severe. The attack must be fortuitously timed: a user who is induced to make the attacker's request at a time when they are not logged in will not be compromised.

It is different to **Self-XSS**, in which the vulnerability is only triggered if the victim themselves submits the XSS payload from their browser. This is considered a low-impact issue.

## Stored XSS:
It arises when an application receives data from an untrusted source and includes that data within its later HTTP responses in an unsafe way. The script supplied by the attacker will then execute in the victim user's browser, in the context of their session with the application.

The attacker does not need to find an external way of inducing other users to make a particular request containing their exploit. Rather, the attacker places their exploit into the application itself and simply waits for users to encounter it.

## DOM-based XSS:
It arises when JavaScript takes data from an attacker-controllable source, such as the URL, and passes it to a sink that supports dynamic code execution, such as `eval()` or `innerHTML`. To deliver a DOM-based XSS attack, you need to place data into a source so that it is propagated to a sink and causes execution of arbitrary JavaScript.

The most common source for [[DOM - Document Object Model]] XSS is the URL, which is typically accessed with the `window.location` object.

In principle, a website is vulnerable to DOM-based cross-site scripting if there is an executable path via which data can propagate from source to sink.
# Methodology:

## PoC
Usually `alert(1)` is used in Proofs of Concept for XSS, but they are not the most reliable form, because in advanced XSS attack scenarios `alert(1)` fails to show the impact of the XSS and sometimes the function is prevented from being called. 

For that reason, `alert(document.domain)`, `alert(window.origin)` or `console.log(window.origin)` are better alternatives for providing in which domain the JavaScript is being executed. This happens because many websites use embedded **sandbox iframes domains** to isolate the user and make XSS harmless. That said, it is still possible to get an XSS on **sandbox iframes**, but you will need it to chain with an **Sandbox Escape**.

## Reflected XSS:
The biggest issue is proper **character encoding**. In some cases, the web application could not be filtering correctly some encodings of characters.

### Objectives:
- Identify variables that are **reflected** in **responses**.
- Assess the input they accept and the **encoding** that gets **applied** on return (if any).

### White-Box Testing:
Analyze any sanitization procedures implemented to decide if these can be circumvented.

### Black-Box Testing:
1. `Detect Input Vectors:` must determine all the user-defined variables, for each web page, and how to input them. This include: [[HTTP parameters]], POST data, hidden form values and predefined values.

2. `Analyze Input Vectors:` use specially crafted input data with each input vector. See [XSS FIlter Evasion Cheat Sheet](https://owasp.org/www-community/xss-filter-evasion-cheatsheet) and XSS Filters Bypass.

3. `Check Impact:` for each test input test you must analyze the result by examining the resulting web page HTML and searching for the test input. Once found, the tester identifies any special characters that were not properly encoded, replaced, or filtered out. Ideally all [[HTML special characters]] will be replaced with [[HTML entities]].

### Bypass XSS Filters:
(...)


#### Useful Tools:
- [PHP Charset Encoder(PCE)](https://cybersecurity.wtf/encoder/) helps you encode arbitrary texts to and from 65 kinds of character sets that you can use in your customized payloads.
- [Hackvertor](https://hackvertor.co.uk/public) is an online tool which allows many types of encoding and obfuscation of JavaScript (or any string input).
- [XSS-Proxy](http://xss-proxy.sourceforge.net/) is an advanced Cross-Site-Scripting (XSS) attack tool.
- [ratproxy](https://code.google.com/archive/p/ratproxy/) is a semi-automated, largely passive web application security audit tool, optimized for an accurate and sensitive detection, and automatic annotation, of potential problems and security-relevant design patterns based on the observation of existing, user-initiated traffic in complex web 2.0 environments.

## Stored XSS:


## DOM-based XSS:




# References:
[PortSwigger's Academy XSS](https://portswigger.net/web-security/cross-site-scripting)
[OWASP Reflected XSS](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting)
[OWASP Stored XSS](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting)
[OWASP Dom-based XSS](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting)

