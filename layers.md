# Layer 1 - User Interface

The layer with which the end user interacts most directly is the user-interface portion of our application, or the front end. This module acts as the main driving force behind our web applicaton and can be implemented any way we want. We could write a client for Windows or the Apple Macintosh or come up with a number of ways to interact with the application.

However, since the purpose of this book is to demonstrate web applications, we will focus our efforts on HTMLspecifically XHTML, an updated version of HTML that is fully compliant with XML and is generally cleaner and easier to parse than HTML. XML is a highly organized markup language in which tags must be followed by closing tags, and the rules for how the tags are placed are more clearly specifiedin particular, no overlapping is allowed.

Thus, the following HTML code

`<br>`

`<br>`

`<B>This is an <em>example of </B>overlapping tags</em>`

is not valid in XHTML, since it is not valid XML. \(See Chapter 23 for more detail.\) The &lt;br&gt; tags need to be closed either by an accompanying &lt;/br&gt; tag or by replacing them with the empty tag, &lt;br/&gt;. Similarly, the &lt;b&gt; and &lt;em&gt; tags are not allowed to overlap. To write this code in XHTML, we simply need to change it to

`<br/>`

`<br/>`

`<b>This is an <em>example of</em></b><em>overlapping tags</em>`

For those who are unfamiliar with XHTML, we will provide more details on it in Chapter 23. For now, it is worth noting that it is not very different from regular HTML, barring the exceptions we mentioned previously. Throughout this book, we will use HTML and XTHML interchangeablywhen we mention the former, chances are we are writing about the latter.

When designing the user interface for our application, it is important to think of how we want the users to interact with it. If we have a highly functional web application with all the features we could possibly want, but it is completely counterintuitive and indecipherable to the end user, we have failed in our goal of providing a useful web application.

As we will see in Chapter 14, it is very important to plan the interface to our application in advance, have a few people review it, and even prototype it in simple HTML \(without any of the logic behind it hooked up\) to see how people react to it. More time spent planning at the beginning of the project translates into less time spent on painful rewrites later on.

# Layer 2 - Business Logic

As we mentioned in the "Basic Layout" section, if our user interface code were to talk to all of the backend components in our system, such as databases and web services, we would quickly end up with a mess of "spaghetti code." We would find ourselves in serious trouble if we wanted to remove one of those components and replace it with something completely different.

## Abstracting Functionality

To avoid this problem, we are going to create a middle tier in our application, often referred to as the "business logic" or "biz-logic" part of the program. In this, we can create and implement an abstraction of the critical elements in our system. Any complicated logic for rules, requirements, or relationships is managed here, and our user interface code does not have to worry about it.

The middle tier is more of a logical abstraction than a separate system in our program. Given that our options for abstracting functionality into different processes or services are limited in PHP, we will implement our business logic by putting it into separate classes, separate directories, and otherwise keep the code separate but still operating from the same PHP scripts. However, we will be sure that the implementation maintains these abstractionsthe user interface code will only talk to the business logic, and the business logic will be the code that manages the databases and auxiliary files necessary for implementation.

For example, our business logic might want to have a "user" object. As we design the application, we might come up with a User and a UserManager object. For both objects, we would define a set of operations and properties for them that our user interface code might need.

User:

{

Properties:

* user id

* name

* address, city, province/state, zip/postal code, country

* phone number\(s\)

* age

* gender

* account number

Operations:

* Verify Account is valid

* Verify Old Enough to create Account

* Verify Account has enough funds for Requested Transactions

* Get Purchase History for User

* Purchase Goods

}

UserManager:

{

Properties:

* number of users

  Operations:

* Add new user

* Delete User

* Update User Information

* Find User

* List All Users

}

Given these crude specifications for our objects, we could then implement them and create a bizlogic directory and a number of .inc files that implement the various classes needed for this functionality. If our UserManager and User objects were sufficiently complex, we could create a separate userman directory for them and put other middle-tier functionality, such as payment systems, order tracking systems, or product catalogues into their own directories. Referring back to Chapter 3, "Code Organization and Reuse," we might choose a layout similar to the following:

Web Site Directory Layout:

`www/`

`generatepage.php`

`uigeneration.inc`

`/images/015/`

`homepage.png`

`bizlogic/`

`userman/`

```
user.inc

usermanager.inc
```

`payments/`

```
payment.inc

paymentmanager.inc
```

`catalogues/`

```
item.inc

catalogue.inc

cataloguemanager.inc
```

`orders/`

```
order.inc

ordermanager.inc
```

If we ever completely changed how we wanted to store the information in the database or how we implemented our various routines \(perhaps our "Find User" method had poor performance\), our user interface code would not need to change. However, we should try not to let the database implementation and middle tier diverge too much. If our middle tier is spending large amounts of time creating its object model on top of a database that is now completely different, we are unlikely to have an efficient application. In this case, we might need to think about whether we want to change the object model exposed by the business logic \(and therefore also modify the front end\) to match this, or think more about making the back end match its usage better.

## What Goes into the Business Logic

Finally, in the middle tier of our web applications, we should change how we consider certain pieces of functionality. For example, if we had a certain set of rules to which something must comply, there would be an instinctive temptation to add PHP code to verify and conform to those rules in the implementation of the middle tier. However, rules changeoften frequently. By putting these rules in our scripts, we have made it harder for ourselves to find and change them and increased the likelihood of introducing bugs and problems into our web application.

We would be better served by storing as many of these rules in our database as possible and implementing a more generic system that knows how to process these rules from tables. This helps reduce the risk and cost \(testing, development, and deployment\) whenever any of the rules change. For example, if we were implementing an airline frequent flyer miles management system, we might initially write code to see how miles can be used:

`<?php`

`if ($user_miles < 50000)`

`{`

```
if \($desired\_ticket\_cat = "Domestic"\)

{

  if \($destination == "NYC" or $destination == "LAX"\)

  {

    $too\_far = TRUE;

  }

  else if \(in\_range\($desired\_date, "Dec-15", "Dec-31"\)\)

  {

    $too\_far = TRUE;

  }

  else \($full\_moon == TRUE and is\_equinox\($desired\_date\)\)

  {

    $too\_far = FALSE;  // it's ok to buy this ticket

  }

}

else

{

  $too\_far = TRUE;

}
```

`else`

`{`

```
// etc.
```

`}`

`?>`

Any changes to our rules for using frequent flyer miles would require changing this code, with a very high likelihood of introducing bugs. However, if we were to codify the rules into the database, we could implement a system for processing these rules.

MILES\_REQUIRED   DESTINATION    VALID\_START\_DATE   VALID\_END\_DATE

55000            LAX            Jan-1              Dec-14

65000            LAX            Dec-15             Dec-31

55000            NYC            Jan-1              Dec-14

65000            NYC            Dec-15             Dec-31

45000            DOMESTIC       Jan-1              Dec-14

55000            DOMESTIC       Dec-15             Dec-31

etc...

We will see more about how we organize our middle tier in later chapters, when we introduce new functionality that our business logic might want to handle for us.

# Layer 3 - Data Layer / Database Server

Our final tier is the place without which none of our other tiers would have anything to do. It is where we store the data for the system, validate the information we are sending and receiving, and manage the existing information. For most of the web applications we will be writing, it is our database engine and any additional information we store on the file system.

Aa fair amount of thought should go into the exact layout of your data. If improperly designed, it can end up being a bottleneck in your application that slows things down considerably. Many people fall into the trap of assuming that once the data is put in a database, accessing it is always going to be fast and easy.

While there are no hard and fast rules for organizing your data and other back end information, there are some general principles we will try to follow \(performance, maintainability, and scalability\).

While we have chosen to use MySQL as the database for the back-end tier of our application, this is not a hard requirement of all web applications. There are a number of database products available that are excellent in their own regard and suited to certain scenarios. We have chosen to use MySQL due to its popularity, familiarity to programmers, and ease of setup.

## n-Tier Architectures

For complicated web applications that require many different pieces of functionality and many different technologies to implement, we can take the abstraction of tiers a bit further and abstract other major blocks of functionality into different modules.

