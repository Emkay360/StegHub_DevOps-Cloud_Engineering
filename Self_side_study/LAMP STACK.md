## What is the LAMP stack?
The LAMP stack is a popular open-source software stack for building and deploying web applications. LAMP is an acronym for the components in the stack: Linux (operating system), Apache (HTTP server), MySQL (database), and PHP, Perl or Python (programming language).

**LAMP stack components**

**Linux:** The operating system. Linux is a free and open-source operating system (OS) that has been around since the mid-1990s. Today, it has an extensive worldwide user base that extends across industries. Linux is popular in part because it offers more flexibility and configuration options than some other operating systems.

**Apache:** The web server. The Apache web server processes requests and serves up web assets via HTTP so that the application is accessible to anyone in the public domain over a simple web URL. Developed and maintained by an open community, Apache is a mature, feature-rich server that runs a large share of the websites currently on the internet. 

**MySQL:** The database. MySQL is an open-source relational database management system for storing application data. With My SQL, you can store all your information in a format that is easily queried with the SQL language. SQL is a great choice if you are dealing with a business domain that is well structured, and you want to translate that structure into the backend. MySQL is suitable for running even large and complex sites. See "SQL vs. NoSQL Databases: What's the Difference?" for more information on SQL and NoSQL databases.

**PHP:** The programming language. The PHP open-source scripting language works with Apache to help you create dynamic web pages. You cannot use HTML to perform dynamic processes such as pulling data out of a database. To provide this type of functionality, you simply drop PHP code into the parts of a page that you want to be dynamic. 

## How LAMP stack elements work together 

A high-level look at the LAMP stack order of execution shows how the elements interoperate. The process starts when the Apache web server receives requests for web pages from a user’s browser. If the request is for a PHP file, Apache passes the request to PHP, which loads the file and executes the code contained in the file. PHP also communicates with MySQL to fetch any data referenced in the code. 

PHP then uses the code in the file and the data from the database to create the HTML that browsers require to display web pages. The LAMP stack is efficient at handling not only static web pages but also dynamic pages where the content may change each time it is loaded depending on the date, time, user identity, and other factors. 

After running the file code, PHP then passes the resulting data back to the Apache web server to send to the browser. It can also store this new data in MySQL. And of course, all of these operations are enabled by the Linux operating system running at the base of the stack.


What is the LAMP stack?
The LAMP stack is a popular open-source software stack for building and deploying web applications. LAMP is an acronym for the components in the stack: Linux (operating system), Apache (HTTP server), MySQL (database), and PHP, Perl or Python (programming language).

You've probably heard about the LAMP stack. Some of today’s most popular open-source web applications—for example, WordPress and Drupal—all run on LAMP. 
But LAMP has a lot more to its credit. It was one of the first open-source software stacks for the web and remains one of the most common ways to deliver web applications. It is so widely used that you are likely to encounter it frequently during your career as you update or host existing applications. It is considered by many to be the platform of choice for developing new custom web apps. 

Stable, simple, powerful—these are words most often used to describe LAMP. All of this makes it well worth learning about and a valuable addition to any developer’s resume.

Guide
Realize the full value of your hybrid cloud
Connect and integrate your systems to prepare your infrastructure for AI.

Related content
Register for the guide on app modernization

LAMP stack components
LAMP stands for Linux, Apache, MySQL, and PHP. Together, they provide a proven set of software for delivering high-performance web applications. Each component contributes essential capabilities to the stack:

Linux: The operating system. Linux is a free and open-source operating system (OS) that has been around since the mid-1990s. Today, it has an extensive worldwide user base that extends across industries. Linux is popular in part because it offers more flexibility and configuration options than some other operating systems.

Apache: The web server. The Apache web server processes requests and serves up web assets via HTTP so that the application is accessible to anyone in the public domain over a simple web URL. Developed and maintained by an open community, Apache is a mature, feature-rich server that runs a large share of the websites currently on the internet. 

MySQL: The database. MySQL is an open-source relational database management system for storing application data. With My SQL, you can store all your information in a format that is easily queried with the SQL language. SQL is a great choice if you are dealing with a business domain that is well structured, and you want to translate that structure into the backend. MySQL is suitable for running even large and complex sites. See "SQL vs. NoSQL Databases: What's the Difference?" for more information on SQL and NoSQL databases.

PHP: The programming language. The PHP open-source scripting language works with Apache to help you create dynamic web pages. You cannot use HTML to perform dynamic processes such as pulling data out of a database. To provide this type of functionality, you simply drop PHP code into the parts of a page that you want to be dynamic. 
PHP is designed for efficiency. It makes programming easier—and a bit more fun—by allowing you to write new code, hit refresh, and immediately see the resulting changes without the need for compiling. If you prefer, you can swap out PHP in favor of Perl or the increasingly popular Python language.

LAMP has a classic layered architecture, with Linux at the lowest level. The next layer is Apache and MySQL, followed by PHP. Although PHP is nominally at the top or presentation layer, the PHP component sits inside Apache.

How LAMP stack elements work together 
A high-level look at the LAMP stack order of execution shows how the elements interoperate. The process starts when the Apache web server receives requests for web pages from a user’s browser. If the request is for a PHP file, Apache passes the request to PHP, which loads the file and executes the code contained in the file. PHP also communicates with MySQL to fetch any data referenced in the code. 

PHP then uses the code in the file and the data from the database to create the HTML that browsers require to display web pages. The LAMP stack is efficient at handling not only static web pages but also dynamic pages where the content may change each time it is loaded depending on the date, time, user identity, and other factors. 

After running the file code, PHP then passes the resulting data back to the Apache web server to send to the browser. It can also store this new data in MySQL. And of course, all of these operations are enabled by the Linux operating system running at the base of the stack.

**LAMP stack flexibility**
Although LAMP uses Linux as the OS, you can use the other components with an alternative OS to meet your specific needs. For example, there is a WAMP stack, which uses Microsoft Windows; MAMP with the Mac OS; and even WIMP, using Windows and the Internet Information Services webserver from Microsoft. 

Because LAMP is all open source and non-proprietary, you can avoid lock-in. You have the flexibility to select the right components for specific projects or business requirements.

LAMP offers flexibility in other ways as well. Apache is modular in design, and you will find there are existing, customizable modules available for many different extensions. These modules range from support for other languages to authentication capabilities. 

Another advantage of LAMP is its secure architecture and well-established encryption practices that have been proven in the enterprise.

