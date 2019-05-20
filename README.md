# gettingStartedWithJSFBootstrap
How to create a bootstrap web application with JSF

This article originally published here with much better formatting:  https://www.codementor.io/howardungar/getting-started-with-bootstrap-and-jsf-6bofntdov

Getting Started with Bootstrap and JSF

This tutorial will give you a great starting point for building any web application using Java Server, Faces, and Bootstrap. It assumes you have Java 8 or later installed.

Environment
In this tutorial, we'll be using NetBeans 8.2 with JBoss Wildfly 10.1.

Download and install Java EE version of Netbeans.
Download the Java EE7 Full & Web Distribution of JBoss Wildfly and unzip it to a folder on your machine.
Create the JSF Project
After launching NetBeans, select "File" --> "New Project" and choose the "Java Web" - "Web Application" template and click "Next."
Name the project "HelloJSF" and click "Next."
On the Server and Settings screen click the "Add..." button and select "Wildfly Application Server" and browse the server location to the directory that you unzipped the Wildfly server. Click "Finish."
On the Frameworks screen, choose "JavaServer Faces". Click "Finish."
Run the project and you should see the following screen in your browser:

Install Bootstrap
Download Bootstrap here and unzip it into your project's web folder under a subdirectory called resources/bootstrap-3.3.7-dist so it looks like this:

├── resources
│   ├── bootstrap-3.3.7-dist
│   │   ├── css
│   │   │   ├── bootstrap-theme.css
│   │   │   ├── bootstrap-theme.css.map
│   │   │   ├── bootstrap-theme.min.css
│   │   │   ├── bootstrap.css
│   │   │   ├── bootstrap.css.map
│   │   │   └── bootstrap.min.css
│   │   ├── fonts
│   │   │   ├── glyphicons-halflings-regular.eot
│   │   │   ├── glyphicons-halflings-regular.svg
│   │   │   ├── glyphicons-halflings-regular.ttf
│   │   │   └── glyphicons-halflings-regular.woff
│   │   └── js
│   │       ├── bootstrap.js
│   │       └── bootstrap.min.js

Because bootstrap.css uses url references they do not work in a vanilla jsf installation.


@font-face {
  font-family: 'Glyphicons Halflings';
 
  src: url('../fonts/glyphicons-halflings-regular.eot');
  src: url('../fonts/glyphicons-halflings-regular.eot?#iefix') format('embedded-opentype'),
      url('../fonts/glyphicons-halflings-regular.woff') format('woff'),
      url('../fonts/glyphicons-halflings-regular.ttf') format('truetype'),
      url('../fonts/glyphicons-halflings-regular.svg#glyphicons_halflingsregular') format('svg');
}

Rather than replacing every relative path with a JSF expression to these resources:

url("#{resource['bootstrap/fonts/glyphicons-halflings-regular.eot']}");
I prefer to use omnifaces to add a resource handler:

Edit your WEB-INF/web.xml file to add the javax.faces.resource URL to the Faces Servlet-Mapping as shown here:
    <servlet-mapping>
        <servlet-name>Faces Servlet</servlet-name>
        <url-pattern>/faces/*</url-pattern>
         <url-pattern>/javax.faces.resource/*</url-pattern>
    </servlet-mapping>

Add the following mime-mapping entries to the web.xml

    <mime-mapping>
      <extension>ttf</extension>
      <mime-type>css/fonts</mime-type>
    </mime-mapping>
    <mime-mapping>
      <extension>otf</extension>
      <mime-type>font/opentype</mime-type>
    </mime-mapping>
    <mime-mapping>
      <extension>woff2</extension>
      <mime-type>font/woff2</mime-type>
    </mime-mapping>  
    <mime-mapping>
      <extension>woff</extension>
      <mime-type>font/woff</mime-type>
    </mime-mapping>  
    <mime-mapping>
      <extension>eot</extension>
      <mime-type>application/vnd.ms-fontobject</mime-type>
    </mime-mapping>
    
Download the omnifaces jar file here and place it in your web application's WEB-INF/lib folder.
Create a faces-config.xml file in the WEB-INF folder with the following resource-handler entry:
<?xml version='1.0' encoding='UTF-8'?>
<faces-config version="2.2"
              xmlns="http://xmlns.jcp.org/xml/ns/javaee"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-facesconfig_2_2.xsd">
<application>  
  <!-- This supports resources referenced from within css files e.g. url(../fonts/glyphicons) -->
    <resource-handler>org.omnifaces.resourcehandler.UnmappedResourceHandler</resource-handler>
</application>

</faces-config>

Create a JSF Template for Bootstrap
Bootstrap pages require a css and two javascript libraries with some important meta tags. The sample bootstrap starter template will be our starting point:

Create a WEB-INF/include folder in your web application and create a new jsf file called template.xhtml in that folder. Edit the file and add the following code:
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:ui="http://java.sun.com/jsf/facelets"
      xmlns:f="http://xmlns.jcp.org/jsf/core"
      xmlns:h="http://xmlns.jcp.org/jsf/html">
  <h:head>
    <meta charset="utf-8"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
    <meta name="viewport" content="width=device-width, initial-scale=1"/>
    <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->
    <meta name="description" content=""/>
    <meta name="author" content=""/>

    <title>JSF Bootstrap Tutorial</title>

    <!-- Bootstrap core CSS -->
    <h:outputStylesheet name="bootstrap-3.3.7-dist/css/bootstrap.min.css"/>
    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
    <f:verbatim>
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->    
    </f:verbatim>
    <ui:insert name="head"/>
  </h:head>
  <h:body>
    <ui:insert name="body"/>
    <!-- Bootstrap core JavaScript
    ================================================== -->
    <!-- Placed at the end of the document so the pages load faster -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
    <h:outputScript name="bootstrap-3.3.7-dist/js/bootstrap.min.js"/>
  </h:body>
</html>

Create a new welcome bootstrap page
Create a new jsf page in the root of your web application, called helloBootstrap.xhtml, to use the above template.xhtml:

<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:ui="http://java.sun.com/jsf/facelets"
      xmlns:h="http://xmlns.jcp.org/jsf/html">
  <ui:composition template="/WEB-INF/include/template.xhtml">
    <ui:define name="head">
        <title>Hello JSF Bootstrap</title>
        <!-- Custom styles for this template -->
        <h:outputStylesheet name="css/starter-template.css"/>
    </ui:define>
    <ui:define name="body">
    <nav class="navbar navbar-inverse navbar-fixed-top">
      <div class="container">
        <div class="navbar-header">
          <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
            <span class="sr-only">Toggle navigation</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
          </button>
          <a class="navbar-brand" href="#">Project name</a>
        </div>
        <div id="navbar" class="collapse navbar-collapse">
          <ul class="nav navbar-nav">
            <li class="active"><a href="#">Home</a></li>
            <li><a href="#about">About</a></li>
            <li><a href="#contact">Contact</a></li>
          </ul>
        </div><!--/.nav-collapse -->
      </div>
    </nav>
    <div class="container">
      <div class="starter-template">
        <h1>Bootstrap starter template</h1>
        <p class="lead">Use this document as a way to quickly start any new project.<br/> All you get is this text and a mostly barebones HTML document.</p>
      </div>
    </div><!-- /.container -->
    </ui:define>
  </ui:composition> 
</html>
Create your own stylesheet
Create a new folder in your web application called resources/css and create a new css stylesheet file called starter-template.css with the following content:

body {
    padding-top: 50px;
}

.starter-template {
    padding: 40px 15px;
    text-align: center;
}
Run and test your new page
Run the project and browse to your newly created page:

http://localhost:8080/HelloJSF/faces/helloBootstrap.xhtml

Download the entire project here:

https://github.com/HowardUngar/gettingStartedWithJSFBootstrap

Next steps
Now you have a template and starting point for creating sophisticated bootstrap based jsf pages. Try adding a backing bean using the @Named and @Dependent annotations. Here's a link to a tutorial for that:

https://netbeans.org/kb/docs/javaee/cdi-intro.html

With JSF 2 you can pass through various html5 attributes needed for bootstrap. Here's a description of how that works:

https://docs.oracle.com/javaee/7/tutorial/jsf-facelets009.htm

