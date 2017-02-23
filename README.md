# Introduction

The main theme of this book is writing web applications using PHP and MySQL, but we have not yet defined what exactly we mean by this. It is important to understand that when we say web application, we are talking about something very different from a simple web site that serves up files such as HTML, XML, and media.



# Terminology

We will define a web application as a dynamic program that uses a web-based interface and a client-server architecture. This is not to say that web applications have to be complicated and difficult to implementwe will demonstrate some extremely simple ones in later chaptersbut they definitely have a more dynamic and code-oriented nature than simple sites.

When we talk about the server, we are primarily referring to the machine or group of machines that acts as the web server and executes PHP code. It is important to note that "the server" does not have to be one machine. Any number of components that the server uses to execute and serve the application might reside on different machines, such as the application databases, web services used for credit card processing, and so on. The web servers might reside on multiple machines to help handle large demand.

As for the client, we are referring to a computer that accesses the web application via HTTP by using a web browser. As we write user interfaces for our web applications, we will resist the urge to use features specific to individual browsers, ensuring the largest possible audience for our products. Some of these clients will have high-speed connections to the Internet measured in Mb/s and will be able to transfer large amounts of data, while others will be restricted to mobile-based connections with a maximum bandwidth measured in Kb/s.

