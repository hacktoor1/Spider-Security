---
description: Advanced Attack
---

# Insecure Deserialization

<figure><img src="../../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>

### Serialization&#x20;

>
>
> **Serialization** is the process by which some <mark style="color:red;">**bit**</mark> if data in programming lan-guage gets converted into a format that allows it to be saved in a DB ir trasferred  over a Network&#x20;

### Deserialization

> is the process of reconstructing data that has been serialized (converted into a specific format, often a sequence of bytes) back into its original form, such as objects or data structures. It involves taking serialized data and converting it back into an object or data structure that can be used in the program. Deserialization is crucial for retrieving stored or transmitted data and restoring it to its original state, allowing applications to work with the data as it was originally structured before serialization.

### Risks of Insecure Deserialization

Insecure deserialization can lead to significant risks:

* **Remote Code Execution (RCE):** Attackers can execute arbitrary code on the server, compromising its integrity.
* **Data Tampering:** Serialized data can be manipulated to alter application logic or compromise data integrity.
* **Authentication Bypass:** Allows attackers to bypass authentication mechanisms by tampering with session or credential data.
* **Denial of Service (DoS):** Exploits can cause crashes or performance issues, disrupting service availability.
* **Privilege Escalation:** Malicious objects may escalate privileges or gain unauthorized access.
* **Financial Loss and Reputation Damage:** Resulting from breaches and loss of customer trust.

## Serialization In Programming Language

### PHP Serialization&#x20;

When an application needs to store or transfer a PHP object over the network, it calls the PHP function <mark style="color:red;">**serialize()**</mark> to pack it up. when the application needs to use that data, it calls <mark style="color:red;">**unserialize( )**</mark> unpack and get the underlying object&#x20;

```php
<?php 
class User{
    public $username;
    public $status;
}
$user = new User;
$user->username = "test";
$user->status = "not admin";
echo serialize($user);
?>
//O:4:"User":2:{s:8:"username";s:4:"test";s:6:"status";s:9:"not admin";}
```

<figure><img src="../../.gitbook/assets/image (236).png" alt=""><figcaption><p>serialize() data</p></figcaption></figure>

example how to add serialize in cookie

```php
<?php
// Step 1: Define the class and create an object
class User {
    public $username;
    public $status;
}

// Step 2: Create a new User object and set its properties
$user = new User;
$user->username = "test";
$user->status = "not admin";

// Step 3: Serialize the object
$serializedData = serialize($user);

// Step 4: Base64 encode the serialized data
$encodedData = base64_encode($serializedData);

// Step 5: Set the cookie (name: user_data, value: base64 encoded serialized data, expires in 1 hour)
setcookie('user_data', $encodedData, time() + 3600, "/");

// Output the encoded data (for testing purposes)
echo "Serialized and encoded cookie value: " . base64_decode($encodedData);
?>
```

<figure><img src="../../.gitbook/assets/image (237).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
b: The\_Boolean;

i: The\_interger;

d: The\_float;

s: Length\_OF\_String:"string";

a: Number\_OF\_Elements:{Elements};

O: Length\_OF\_Name:"Class\_name":{properties}
{% endhint %}

<pre class="language-php"><code class="lang-php">&#x3C;?php
// Define a sample class
<strong>class User { 
</strong>    public $name;
    public $email;

    public function __construct($name, $email) { //class construct
        $this->name = $name;
        $this->email = $email;
    }
}

// Create an instance of the User class
$user = new User('John Doe', 'john@example.com');

// Serialize the object to a string
$serializedUser = serialize($user);

// Echo the serialized data
echo "Serialized User Data:\n";
echo $serializedUser . "\n";

// Deserialize the string back into an object
$deserializedUser = unserialize($serializedUser);

// Echo the deserialized object's properties
echo "\nDeserialized User Data:\n";
echo "Name: " . $deserializedUser->name . "\n";
echo "Email: " . $deserializedUser->email . "\n";
?>
</code></pre>

The PHP code defines a <mark style="color:red;">**User**</mark> class with <mark style="color:red;">**name**</mark> and <mark style="color:red;">**email**</mark> properties. It demonstrates serialization by converting an instance of <mark style="color:red;">**User**</mark> into a string using <mark style="color:red;">**serialize()**</mark>, then deserializes it back into an object using <mark style="color:red;">**unserialize()**</mark>, displaying the reconstructed user's data.

<figure><img src="../../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

#### PHP Magic Methods

PHP provides several magic methods that are crucial for customizing the serialization and deserialization processes:

* <mark style="color:red;">**`__sleep()`**</mark>: This method is invoked before serialization. It allows you to perform cleanup tasks, such as closing database connections and should return an array of property names that should be serialized.
* <mark style="color:red;">**`__wakeup()`**</mark>: This method is called deserialization. It is used to reinitialize any resources or connections that were serialized along with the object, ensuring it operates correctly after deserialization.
* <mark style="color:red;">**`__serialize()`**</mark>: Introduced in PHP 7.4, this method allows you to customize the serialization data by returning an array representing the object's serialized form. It provides fine-grained control over what data gets serialized.
* <mark style="color:red;">**`__unserialize()`**</mark>: This method is the counterpart to <mark style="color:red;">**`__serialize()`**</mark>. It allows you to customize the restoration of an object from its serialized data. This can include initializing properties or performing any necessary post-processing tasks.
* <mark style="color:red;">**`__destruct()`**</mark> : method in PHP is a magic method that is automatically called when an object is no longer referenced or its script ends. It's used for cleanup tasks, such as closing files or database connections, releasing resources, or performing other actions that should be done when the object is no longer needed.

### Java Serialization

Serialization in Java converts an object into a sequence of bytes, which can be easily stored or transmitted. It allows objects to be saved to files or sent over networks. By implementing the <mark style="color:red;">**Serializable**</mark> interface and using <mark style="color:red;">**ObjectOutputStream**</mark>, Java can serialize an object's state, which can later be deserialized back into its original form using <mark style="color:red;">**ObjectInputStream**</mark>.

```java
import java.io.*;

// Example class to serialize and deserialize
class MyClass implements Serializable {
    private String name;
    private int age;

    // Constructor
    public MyClass(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getter methods
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public static void main(String[] args) {
        // Create an object of MyClass
        MyClass obj = new MyClass("John Doe", 30);

        // Serialization
        try {
            // Serialize the object
            FileOutputStream fileOut = new FileOutputStream("serialized_object.ser");
            ObjectOutputStream out = new ObjectOutputStream(fileOut);
            out.writeObject(obj);
            out.close();
            fileOut.close();
            System.out.println("Object serialized and saved as 'serialized_object.ser'");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Deserialization
        try {
            // Deserialize the object
            FileInputStream fileIn = new FileInputStream("serialized_object.ser");
            ObjectInputStream in = new ObjectInputStream(fileIn);
            MyClass deserializedObj = (MyClass) in.readObject();
            in.close();
            fileIn.close();

            // Print deserialized object
            System.out.println("\\nDeserialized Object:");
            System.out.println("Name: " + deserializedObj.getName());
            System.out.println("Age: " + deserializedObj.getAge());
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

**Explanation:**

* **MyClass Definition:** Defines a simple <mark style="color:red;">**`MyClass`**</mark> that implements <mark style="color:red;">**`Serializable`**</mark> interface with `name` and `age` attributes.
* **Serialization (main method):**
  * Creates an instance <mark style="color:red;">**`obj`**</mark> of <mark style="color:red;">**`MyClass`**</mark>.
  * Uses <mark style="color:red;">**`ObjectOutputStream`**</mark> to serialize `obj` and write it to <mark style="color:red;">**`serialized_object.ser`**</mark>.
* **Deserialization (main method):**
  * Reads the serialized object from <mark style="color:red;">**`serialized_object.ser`**</mark> using <mark style="color:red;">**`ObjectInputStream`**</mark>.
  * Casts the read object to <mark style="color:red;">**`MyClass`**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**(**</mark><mark style="color:red;">**`(MyClass) in.readObject()`**</mark><mark style="color:red;">**)**</mark> for type safety.
  * Prints the deserialized object's attributes (<mark style="color:red;">**`name`**</mark> and <mark style="color:red;">**`age`**</mark>) to verify successful deserialization.
* **Output:**
  * The program outputs messages indicating the serialization and deserialization operations.
  * After deserialization, it prints the attributes of the deserialized object (<mark style="color:red;">**`Name`**</mark> and <mark style="color:red;">**`Age`**</mark>).

This example demonstrates the complete process of serializing (<mark style="color:red;">**`ObjectOutputStream`**</mark>) and deserializing (<mark style="color:red;">**`ObjectInputStream`**</mark>) a Java object using Java's built-in serialization mechanism.



We will create Tow Files:&#x20;

**item.java** -> hold code for a class name

**Serialize.java** -> which will contain the serialization logic

{% code title="item.java" %}
```java
import java.io.*;
public class item implements Serializable {
    int id;
    String name;
    public item(int id,String name){
        this.id = id;
        this.name = name;
    }
}
```
{% endcode %}

**Item.java is a simple class that  has two fields: **<mark style="color:red;">**id**</mark>** and **<mark style="color:red;">**name**</mark>

Now, we will use another file (in Java one class should be contained in one file)

{% code title="Serialize.java" %}
```java
import java.io.*;
class serialize{
 public static void main(String[] argv){
  try{
    //creating the object
    item s1 = new item(123,"book");
    //Creating stream and writing the object
    FileOutputStream fout = new FileOutputStream("data.ser");
    ObjectOutputStream out = new ObjectOutputStream(fout);
    out.writeObject(s1);
    out.flush();
    //closing the stream 
    out.close();
    System.out.println("Serialized data saved to data.ser");
  }catch (Exception e){System.out.println(e);}
 }
}
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>

Using Some commands in Linux such as **Strings or haxdump**

&#x20;

<figure><img src="../../.gitbook/assets/image (241).png" alt=""><figcaption></figcaption></figure>

the application  For Java serialized Objects, you should also look for base64 strings starting with <mark style="color:red;">**`rO0AB`**</mark>

```bash
echo -en "\xac\xed\x00\x05" | base64

#output
rO0ABQ==
```

deserializtion

```java
import java.io.*;
class deserialize{
 public static void main(String[] argv){
  try{
    //Creating stream and writing the object
    ObjectInputStream in = new ObjectInputStream(new FileInputStream("data.ser"));
    item s = (item) in.readObject();
    System.out.println(s.id+" "+s.name);
    //closing the stream 
    in.close();
  }catch (Exception e){System.out.println(e);}
 }
}
```



<figure><img src="../../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

insecure deserialization Conditions

Executing OS commands in Java could be done

Example:

```java
java.lang.Runtime.getRuntime.exec("whoami")
```

Ysoserial

{% embed url="https://github.com/frohoff/ysoserial/releases/tag/v0.0.6" %}

Requerd:

* java 8
* **`update-alternatives --config java`** -> switch version

```bash
java -jar ysoserial-all.jar CommonsCollections1  "whomai" | base64 
```

<figure><img src="../../.gitbook/assets/image (243).png" alt=""><figcaption></figcaption></figure>

## .NET Serialization

Saving the states of objects  using serialization in .NET can be done using various methods \
Example:

* BinaryFormatter
* DataContractSerializer
* NetDataContractSerializer
* XML Serialization

In .NET, there are several serialization techniques available, including **binary serialization**, **XML serialization**, **JSON serialization**, and **custom serialization**.

to make demo&#x20;

requred:

windows

in search Turn window featuers on or off

### &#x20;**Binary Serialization**

**How to Do Binary Serialization:**

1. **Mark the class as serializable** by adding the `[`<mark style="color:red;">**`Serializable`**</mark>`]` attribute.
2. **Use the **<mark style="color:red;">**`BinaryFormatter`**</mark> class to perform serialization and deserialization.

```csharp
using System;
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;

[Serializable]
public class MyClass
{
    public int Id { get; set; }
    public string Name { get; set; }
}

class Program
{
    static void Main()
    {
        MyClass obj = new MyClass { Id = 1, Name = "Example" };

        // Serialize
        FileStream fileStream = new FileStream("data.bin", FileMode.Create);
        BinaryFormatter formatter = new BinaryFormatter();
        formatter.Serialize(fileStream, obj);
        fileStream.Close();

        // Deserialize
        fileStream = new FileStream("data.bin", FileMode.Open);
        MyClass deserializedObj = (MyClass)formatter.Deserialize(fileStream);
        fileStream.Close();

        Console.WriteLine($"Id: {deserializedObj.Id}, Name: {deserializedObj.Name}");
    }
}
```

### **XML Serialization**

XML serialization converts an object into XML format. It is useful for interoperability between different platforms and languages

```csharp
using System;
using System.IO;
using System.Xml.Serialization;

public class MyClass
{
    public int Id { get; set; }
    public string Name { get; set; }
}

class Program
{
    static void Main()
    {
        MyClass obj = new MyClass { Id = 1, Name = "Example" };

        // Serialize
        XmlSerializer xmlSerializer = new XmlSerializer(typeof(MyClass));
        using (StreamWriter writer = new StreamWriter("data.xml"))
        {
            xmlSerializer.Serialize(writer, obj);
        }

        // Deserialize
        using (StreamReader reader = new StreamReader("data.xml"))
        {
            MyClass deserializedObj = (MyClass)xmlSerializer.Deserialize(reader);
            Console.WriteLine($"Id: {deserializedObj.Id}, Name: {deserializedObj.Name}");
        }
    }
}
```

### **JSON Serialization**

JSON serialization converts an object into JSON format, which is both human-readable and efficient for web-based applications. The [<mark style="color:red;">**`System.Text.Json`**</mark> ](#user-content-fn-1)[^1]namespace is commonly used for JSON serialization in .NET Core and .NET 5/6+.

```csharp
using System;
using System.Text.Json;
using System.IO;

public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

class Program
{
    static void Main(string[] args)
    {
        Person person = new Person { Name = "John Doe", Age = 30 };
        
        // JSON serialization
        string jsonString = JsonSerializer.Serialize(person);
        File.WriteAllText("person.json", jsonString);
        
        // JSON deserialization
        string readJson = File.ReadAllText("person.json");
        Person deserializedPerson = JsonSerializer.Deserialize<Person>(readJson);
        
        Console.WriteLine($"Name: {deserializedPerson.Name}, Age: {deserializedPerson.Age}");
    }
}

```

### Custom Serialization (Using <mark style="color:red;">**`ISerializable`**</mark>)

By implementing the interface, custom serialization allows you to control how the object is serialized and deserialized. This method is used when you need to serialize non-serializable objects or need fine control over the process.

**Example:**

```csharp
using System;
using System.Runtime.Serialization;
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;

[Serializable]
public class Person : ISerializable
{
    public string Name { get; set; }
    public int Age { get; set; }

    // Default constructor is required for deserialization
    public Person() {}

    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }

    // Custom serialization
    public void GetObjectData(SerializationInfo info, StreamingContext context)
    {
        info.AddValue("PersonName", Name);
        info.AddValue("PersonAge", Age);
    }

    // Custom deserialization
    protected Person(SerializationInfo info, StreamingContext context)
    {
        Name = info.GetString("PersonName");
        Age = info.GetInt32("PersonAge");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Person person = new Person("John Doe", 30);
        
        // Custom binary serialization
        BinaryFormatter formatter = new BinaryFormatter();
        using (FileStream fs = new FileStream("person_custom.dat", FileMode.Create))
        {
            formatter.Serialize(fs, person);
        }
        
        // Custom binary deserialization
        using (FileStream fs = new FileStream("person_custom.dat", FileMode.Open))
        {
            Person deserializedPerson = (Person)formatter.Deserialize(fs);
            Console.WriteLine($"Name: {deserializedPerson.Name}, Age: {deserializedPerson.Age}");
        }
    }
}

```

[^1]: 
