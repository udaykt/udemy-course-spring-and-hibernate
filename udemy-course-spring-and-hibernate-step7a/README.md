# udemy-course-spring-and-hibernate

Step 7 : Spring MVC : creating a spring home controller and view
---

I. Install
---

Download and import this this seed : https://github.com/jvanhouteghem/spring-seed-mvc

II. Our first home controller
---

NB : This steps is already done by default in the seed.

Development process : 
- create controller class
- define controller method
- add request mapping to controller method
- return view name
- develop view page

1) Create new package named com.jvanhouteghem.springdemo

2) Create controller class

```java
@Controller
public class HomeController {
	
	// You can use any method name
	@RequestMapping("/")
	public String showPage(){
		// Return view name in WEB-INF/view
		return "main-menu";
	}
	
}
```

3) Create view page named main-menu.jsp

```html
<!DOCTYPE html>
<html>

    <body>

        <h2>Spring MVC Home Page</h2>

        <p>Hello world</p>

    </body>

</html>
```

Run on server and connect to : http://localhost:8080/spring-mvc-demo/

III. Reading HTML form data
---

Remember : if you use annotation mapping, the method name don't matters.

Development process : 
- create controller class
- create controller method to show html form
- create view page for html form
- create controller method to process html form
- develop view page for confirmation

1) Create new class HelloWorldController

```java
@Controller
public class HelloWorldController {

	// need a controller method to show the initial HTML form
	@RequestMapping("/showForm")
	public String showForm(){
		return "helloworld-form";
	}
	
	// need a controller method to process the HTML form
	@RequestMapping("/processForm")
	public String processForm(){
		return "helloworld";
	}
}
```

2) Create new view file named helloworld-form.jsp

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
	<title>Hello World - Input Form</title>
</head>
<body>

	<form action="processForm" method="GET">
		<input type="text" name="studentName" placeholder="What's your name" /> 
		<input type="submit" />
	</form>

</body>
</html>
```

3) Try it : http://localhost:8080/spring-mvc-demo/processForm?studentName=zzz

4) Create new view helloworld.jsp

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
	<head>
	</head>
	<body>
		<p> Hello World of Spring ! </p>
		
		<p>Student name : ${param.studentName}
		
	</body>
</html>
```

5) Lets add a link to main-menu.jsp

```html
<!DOCTYPE html>
<html>

	<body>
	
		<h2>Spring MVC Home Page</h2>
	
		<a href="showForm">Hello world form</a>
	
	</body>

</html>
```

IV. Adding data to the spring model
---

The model is a container for your application data.

In your controller :
- you can put anything in the model
- strings, objects, info from database, etc...

Your view page (JSP) can access data from the model

In this example we want to :
- create a new method to process form data
- read the form data : student's name
- convert the name to upper case
- add the uppercase version to the model

1) Update HelloWorldController

```java
@Controller
public class HelloWorldController {

	// need a controller method to show the initial HTML form
	@RequestMapping("/showForm")
	public String showForm(){
		return "helloworld-form";
	}
	
	// need a controller method to process the HTML form
	@RequestMapping("/processForm")
	public String processForm(){
		return "helloworld";
	}
	
    //(new)
	// need a controller method to read form data 
	// and add data to the model
	@RequestMapping("/processFormVersionTwo")
	public String letsShoutDude(HttpServletRequest request, Model model){
		
		// read the request parameter from the HTML form
		String theName = request.getParameter("studentName");
		
		// convert the data to all caps
		theName = theName.toUpperCase();
		
		// create the message
		String result = "Yo! " + theName;
		
		// add message to model
		model.addAttribute("message", result);
		
		return "helloworld";
	}
}
```

2) Update view page helloworld.jsp to access model data

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
	<head>
	</head>
	<body>
		<p> Hello World of Spring ! </p>
		
		<p>Student name : ${param.studentName}
		
		<p>The message : ${message}</p>
		
	</body>
</html>
```

3) Update helloworld-form.jsp

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
	<title>Hello World - Input Form</title>
</head>
<body>

	<form action="processForm" method="GET">
		<input type="text" name="studentName" placeholder="What's your name" /> 
		<input type="submit" />
	</form>
	
		<form action="processFormVersionTwo" method="GET">
		<input type="text" name="studentName" placeholder="What's your name LOUDER" /> 
		<input type="submit" />
	</form>

</body>
</html>
```

V. What about css, img and js ?
---

1) Update spring-mvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans
    	http://www.springframework.org/schema/beans/spring-beans.xsd
    	http://www.springframework.org/schema/context
    	http://www.springframework.org/schema/context/spring-context.xsd
    	http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

	<!-- Step 3: Add support for component scanning -->
	<context:component-scan base-package="com.luv2code.springdemo" />

	<!-- Step 4: Add support for conversion, formatting and validation support -->
	<mvc:annotation-driven/>

	<!-- Step 5: Define Spring MVC view resolver -->
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/view/" />
		<property name="suffix" value=".jsp" />
	</bean>

    <!-- Step 6 : load img, css, js ... (new) -->
	<mvc:resources mapping="/resources/**" location="/resources/" />

</beans>
```

2) Create "ressources" folder in WebContent

3) In ressources folder add 3 folders : css, img, js

4) In css folder create new main.css file

```css
body {
    background-color: lightblue;
}

h2 {
    color: white;
    text-align: center;
}

p {
    font-family: verdana;
    font-size: 20px;
}
```

5) In js folder add new main.js file

```js
function doSomeWork() {
	
	alert("I'm doing some work!!!");
	
}
```

6) Update main-menu.jsp

```html

```