# 1. Introduction To JSP
Previously we have seen how implement servlets to build web pages. Next we will dive into JSP.

Java Server Pages (JSP) is a technology which is used to develop web pages by inserting Java code into the HTML pages by making special JSP tags.

It can be used as HTML page, which can be used in forms and registration pages with the dynamic content into it. JSP can be used for separation of the view layer with the business logic in the web application.
    
JSP is an extension of servlets and every JSP page first gets converted into servlet by JSP container before processing the client’s request.

---
# 2. How to write JSP code 

Let us see how to create a JSP project and how to work with JSP files.

* First thing we need to do is create a dynamic web page.
![image](screenshots/Jsp/jsp_1.png)
* Name your project and  Don't forget to Generate web.xml deployment descriptor.
* Next thing we need to do is create a JSP file.
     *  To create a jsp file, Right click on Your project -> new -> JSP file.
    ![image](screenshots/Jsp/jsp_2.png) 
    * Name  your file and click on finish
![image](screenshots/Jsp/jsp_3.png)    
* As we can see, our Eclipse ide is not recognizing the jsp tag on the top of our page. To fix this We need to import some files from our apache tomcat download. To do that follow this steps.
    * Right click on our project -> build path -> Configure build path -> Libraries -> Classpath -> Add External JARs
    ![image](screenshots/Jsp/jsp_4.png) 
    * Next go to the extracted Tomcat folder and click on the lib folder. In that folder pick a jar file named servlet-api and click on open then apply and close.
* Now our error is gone and we can proceed with JSP.
* Lets move to the web.xml file and make our home.jsp file the default page.

## web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>jspdemo</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>home.jsp</welcome-file> <!-- Adding our jsp file here-->
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
</web-app>
```
* You can test your project with a simple text out put. Sava all and run your project on a server.
## home.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
  This from JSP
</body>
</html>
```
   ![image](screenshots/Jsp/jsp_6.png)

  ## **JSP Tags**
JSP tags are an essential part of Java Server Pages. Tags in JSP create a container for Java code, insulating and providing separation of dynamic content from static design elements in your site. Here are some of the most popular JSP tags.

  **Declaration Tags** 

Declaration tags in JSP function as identification containers for the functions, methods and variables in JSP pages.

```jsp
<%! jsp declaration %>
```
  **Expression Tags**

  Expression tags signal JSP to convert a Java statement into a string and display the output.

  ```jsp
  <%= Java statement %>
  ```

  **JSP Scriptlet Tags**
  
Scriptlet tags allow you to embed any valid Java source code in JSP server pages. The code within the tags executes in consecutive order on the server side and is available for client access through a Web browser.  
```jsp 
 <% Java code %> 
```

* We can see some of this tags in action using a simple program that adds two numbers.

 ## home.jsp
 ```jsp
 <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
  <h1>This from JSP <br></h1>   <!--  Html header -->
  
  <%! int x=3; int y = 5;   int sum = 0; %>  <!-- Declaration Tag  -->
  
  <% sum = x+y; %> <!-- Scriptlet Tag -->
  
  The Sum of <%= x  %> and <%= y %> is  <%=sum %> <!-- Expression Tag -->
</body>

</html>
 ```

 Save and refresh our page.

 ![image](screenshots/Jsp/jsp_7.png)

 With this simple program we have seen how to use Jsp tags in a jsp file for the static html part and for the dynamic java part. 
 
 We Can see further implementation of the dynamic nature of our program by adding more logic to the program where our output changes colors to green and red when the sums are even and odd respectively. Here we are making sure that the website is changing based on the users action.

 ## home.jsp

 ```jsp
 <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
  <h1>This from JSP <br></h1>   <!--  Html header -->
  
  <%! int x=3; int y = 5;   int sum = 0; %>  <!-- Declaration Tag  -->
  
  <% sum = x+y; %> <!-- Scriptlet Tag -->
  
   <%
   if (sum%2 ==0){
   %>
  <p style = "color:green">
  <%
   }
  else{
   %>
   <p style = "color:red">
   <%}%>
   The Sum of <%= x  %> and <%= y %> is  <%=sum %> <!-- Expression Tag -->
   </p>
</body>

</html>
```
* Save and refresh our page
 ![image](screenshots/Jsp/jsp_8.png)
---
 # 3. Writing Java and Servlet syntax in JSP
 As we have seen earlier we can write java syntax in a jsp file using a tag. servlet is nothing but a java class so we can also write servlets in jsp files. 
 * Let us create a jsp file using the steps we followed earlier. It will be named "syntax".
 * We need to make sure the syntax page is the default page by updating our web.xml file.
 * In the  previous code that adds to numbers we outputted our result in the html part of our code outside of the java code. 
    * ![image](screenshots/Jsp/jsp_9.png)
* There is another way we can print our output inside the java tag using out.print. We can see that by using the following simple code.

## syntax.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<%! int i = 5; %>

<%
	if (i%2 == 0){
		out.print("<p style = \"color:red; \">this is even number</p>"); // We used a skipping character / as we are telling the compiler not to confuse the " characters.
	}else{
		out.print("<p style = \"color:blue; \">this is odd number</p>");
	}

%>

</body>
</html>
```
* Run the project and see the out put
 ![image](screenshots/Jsp/jsp_10.png)
* We can also see how to use loop syntax in jsp and how to insert an html tag inside the loop. 
## synax.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<%! int i = 5; %>
<%
	if (i%2 == 0){
		out.print("<p style = \"color:red; \">this is even number</p>");
	}else{
		out.print("<p style = \"color:blue; \">this is odd number</p>");
	}
%>
<%
int x=3;
for(int i = 0; i<x; i++){
 %>
<p style="font-size: <%= (i+1)*10%>px"> This is text -  <%=(i+1) %> </p>
// An html tag can be used in for loop using this logic by separating the java tags
<%	
}
%>
</body>
</html>
```
* Save and refresh the syntax page 
![image](screenshots/Jsp/jsp_11.png)

* We can use other loops like while and do while using the same logic of separating the static html part and the dynamic java part.
 ---
# 4. How to use JSP directives 
JSP directives are the elements of a JSP source code that guide the web container on how to translate the JSP page into it’s respective servlet. They are special instruction to Web Container at the time of page translation. Directive tags are of three types: **Page, Include and Taglib.**

Page: Defines page dependent properties such as language, session, errorPage.

Include: Defines file to be included.

Taglib: Declares tag library used in the page

* Let us create another jsp file to see directives in action. Make sure to make this new page the default page of our project.
* When we create any jsp file the page directive will be on top of our page by default.
## Page directive
## directive.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>   <!-- This is A page directive --> 
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>

</body>
</html>
```
The ***Page directive*** defines a number of page dependent properties which communicates with the Web Container at the time of translation. Basic syntax of using the page directive is **<%@ page attribute="value" %>** where attributes can be one of the following :

* **Import attribute** : The import attribute defines the set of classes and packages that must be imported in servlet class definition.
* **Language attribute**:  defines scripting language to be used in the page.
* **Extends attribute**: defines the class name of the superclass of the servlet class that is generated from the JSP page.
* **Session attribute**: defines whether the JSP page is participating in an HTTP session. The value is either true or false.
* **IsThreadSafe attribute** : declares whether the JSP is thread-safe. The value is either true or false
* **IsErrorPage attribute** : declares whether the current JSP Page represents another JSP's error page.
* **ErrorPage attribute** : indicates another JSP page that will handle all the run time exceptions thrown by current JSP page. It specifies the URL path of another page to which a request is to be dispatched to handle run time exceptions thrown by current JSP page.
* **ContentType attribute** : defines the MIME type for the JSP response.
* **AutoFlush attribute** : defines whether the buffered output is flushed automatically. The default value is "true".
* **Buffer attribute** : defines how buffering is handled by the implicit out object.

Let us see an example of using the import attribute in a jsp file. When we try to use List and arrayList in the java tag it shows an error. This is because we need to import their classes to our project. For that we simply need to import the classes in the page directive as shown below.
![image](screenshots/Jsp/jsp_12.png)

## directive.jsp
```jsp
   <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1" 
    import="java.util.List,java.util.ArrayList;" 
    %> <!-- This is how we import classes and packages in the page directive -->
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
 <%
 List<String>favoriteFoods = new ArrayList<String>();
 favoriteFoods.add("Shiro");
 favoriteFoods.add("Doro");
 favoriteFoods.add("Sigawet");
 
 for(String food : favoriteFoods){
	 %>
	 <h3><%= food %></h3>
<%	 
 }
 %>
</body>
</html>
```
* Now the error is gone 
* (Another error might occur depending on the tomcat server you are using Try removing the ';' from your import)
   ```jsp
      <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1" 

    import="java.util.List, java.util.ArrayList " 

    %>
  ```
* Now, we can run our project to see the output.

![image](screenshots/Jsp/jsp_13.png)


## Include directive
The ***include directive*** tells the Web Container to copy everything in the included file and paste it into current JSP file. The syntax of include directive is :

    ***<%@ include file="file.jsp" %>***

There are many ways in which this directive proves to be quite useful in giving a structure to your web application code. 

For exampleWhenever we are building a web application, with webpages, all of which have the same top header and bottom footer . We make them as separate jsp files and include them using the include directive in all the pages. Hence whenever we have to update something in the top header or footer, we just have to do it at one place.  We can see the implementation below.

* Let us create two jsp files called "header" and "footer".
* 

## directives.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1" 
    import="java.util.List, java.util.ArrayList" 
    %>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<%@ include file="header.jsp" %> <!-- Header File -->

<hr>

 <%
 List<String>favoriteFoods = new ArrayList<String>();
 favoriteFoods.add("Shiro");
 favoriteFoods.add("Doro");
 favoriteFoods.add("Sigawet");
 
 for(String food : favoriteFoods){
	 %>
	 <h3><%= food %></h3>
<%	 
 }
 %>
 
 <hr>
 
 <%@ include file="footer.jsp" %><!-- Footer file -->
</body>
</html>
```
## header.jsp
```jsp
 <div>

<a href="#">Home</a>  | <a href="#">About-us</a>  |  <a href="#">Contact-us</a>

</div>
```
## footer.jsp
```jsp
<div>

<h4>This is the footer</h4>

</div>
```

* Save and refresh our page and here is the output
![image](screenshots/Jsp/jsp_14.png)

## Taglib Directive
The ***taglib directive*** is used to define tag library that the current JSP page uses. A JSP page might include several tag library. JavaServer Pages Standard Tag Library (JSTL), is a collection of useful JSP tags, which provides many commonly used core functionalities. It has support for many general, structural tasks such as iteration and conditionals, ready made tags for manipulating XML documents, internationalization tags, and for performing SQL operations. 
Syntax of taglib directive is:
```jsp
<%@ taglib prefix="prefixOfTag" uri="uriOfTagLibrary" %>
```
The prefix is used to distinguish the custom tag from other library custom tag. Prefix is prepended to the custom tag name. Every custom tag must have a prefix.

# 5. Implicit objects

JSP implicit objects are created during the translation phase of JSP to the servlet. These objects can be directly used in scriplets that goes in the service method. They are created by the container automatically, and they can be accessed using objects. JSP Implicit Objects are also called pre-defined variables.JSP provide you Total 9 implicit objects which are as follows 

**request:** This is the object of HttpServletRequest class associated with the request.

**response:** This is the object of  HttpServletResponse class associated with the response to the client.

**config:** This is the object of ServletConfig class associated with the page.

**application:** This is the object of ServletContext class associated with the application context.

**session:** This is the object of HttpSession class associated with the request.

**page context:** This is the object of PageContext class that encapsulates the use of server-specific features. This object can be used to find, get or remove an attribute.

**page object:** The manner we use the keyword this for current object, page object is used to refer to the current translated servlet class.

**exception:** The exception object represents all errors and exceptions which is accessed by the respective jsp. The exception implicit object is of type java.lang.Throwable.

**out:** This is the PrintWriter object where methods like print and println help for displaying the content to the client.


* Lets create another jsp file called "implicitObj.jsp" to see implicit objects in action.
* Lets Make it our default page in the web.xml file.
* We also need to create another jsp file called "requestTest.jsp" to test out redirection in jsp.

## implicitObj.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<a href="requestTest.jsp"> Go to request Test !</a>
</body>
</html>
```
## requestTest.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
Request test is Working !!!
</body>
</html>
```

* Now  we can save all files and run the project.
![image](screenshots/Jsp/jsp_15.png)
* We will be on our default page which is "implicitObj.jsp" and on that page there is a link to redirect us to the requestTest page. click on it.
![image](screenshots/Jsp/jsp_16.png)
* We have successfully linked two jsp files together, Now we can see some implicit object implementations.
* We can send data from one page to another using the request parameter. we can use a form to manipulate the input but we can test this parameter automatically without a form. Lets update the Jsp files to accomplish this task.


## implicitObj.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<a href="requestTest.jsp?msg=secret&secretKey=123456"> Go to request Test !</a>
</body>
</html>
```
Notice the change in the a tag 
```jsp
<a href="requestTest.jsp?msg=secret&secretKey=123456"> Go to request Test !</a>
```
Lets save and refresh our page.

![image](screenshots/Jsp/jsp_17.png)
![image](screenshots/Jsp/jsp_18.png)
* Our data is sent to our new page and it can be seen in the URL bar because by default get method is used.
* To view our sent data on our page we must use implicit objects. Specifically the request object,
* Lets update the requestTest.jsp file




## requestTest.jsp
```jsp

<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<h2>Request test is Working !!!</h2> <br>

Msg Value is : <%= request.getParameter("msg") %> <br>
Secret Key Value is : <%= request.getParameter("secretKey") %>
</body>
</html>
```
Lets save and refresh to see the changes.
![image](screenshots/Jsp/jsp_17.png)
![image](screenshots/Jsp/jsp_19.png)
We can continue with other implicit objects but lets create another jsp file called "error.jsp". With this file we can create an error page that is triggered when the value of "msg" is null. With this example we can see how to use two other implicit objects, respond and session objects.
* Lets write some code in our new jsp file and update the previous files.

## error.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<h3>There is an error !!!</h3> 
 Please return to main page <a href = "implicitObj.jsp">Go to main page</a>
</body>
</html>
```
* After an error occurs we will redirect our page back to our default page (implicitObj.jsp). But first we need to create a session to pass an error parameter on or page.

## requestTest.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<h2>Request test is Working !!!</h2> <br>
<%
String msg = request.getParameter("msg");
String key = request.getParameter("secretKey");
if (msg == null){
	session.setAttribute("error", "error is msg not found");
	response.sendRedirect("error.jsp");
}
%>
</body>
</html>
```
* Lets update our "implicitObj" too.

## implicitObj
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<a href="requestTest.jsp?secretKey=123456"> Go to request Test !</a> <br>

Error msg: <%= session.getAttribute("error") %>
</body>
</html>
```
```jsp
<a href="requestTest.jsp?secretKey=123456"> Go to request Test !</a>
```
Notice here that msg is not passed to the requestTest page.
* Now Lets save and refresh our page.
![image](screenshots/Jsp/jsp_20.png)
![image](screenshots/Jsp/jsp_21.png)
It redirected to the error page instead of the intended requestTest page because we didn't pass the "msg" variable. there is an option to go back to our main page by clicking the link provided.
![image](screenshots/Jsp/jsp_21.png)
![image](screenshots/Jsp/jsp_22.png)
* The error message we set in a session is passed to our main page.

 As we discussed earlier HTTP is a "stateless" protocol which means each time a client retrieves a Webpage, the client opens a separate connection to the Web server and the server automatically does not keep any record of previous client request. There are few options to maintain the session between the Web Client and the Web ServerIn JSP, the session is the most regularly used implicit object of type HttpSession. It is mainly used to approach all data of the user until the user session is active. 

 We can also see two other commonly used implicit objects, out and application Implicit objects.
* By updating the implicitObj.jsp file we can see their implementation.
 ## implicitObj
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<a href="requestTest.jsp?secretKey=123456"> Go to request Test !</a> <br>
Error msg: <%= session.getAttribute("error") %> <br>

<%
Integer mycount = (Integer)application.getAttribute("count");
if(mycount == null || mycount==0){
	mycount =1;	
}else{
	mycount+=1;}

	out.println("Number of times accessed:"+mycount); // This is The out implicit object
	application.setAttribute("count",mycount);// Thid id The application object
%>
</body>
</html>
```
This simple application will count how many times our page is accessed by a user. It used the application implicit object.
![image](screenshots/Jsp/jsp_23.png)
![image](screenshots/Jsp/jsp_24.png)
![image](screenshots/Jsp/jsp_25.png)

---
# 6. Forms in JSP
