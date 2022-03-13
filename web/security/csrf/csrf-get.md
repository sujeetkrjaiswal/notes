# CSRF - GET



### Code

#### Incorrect code implementation

```markup
<a
    href="/ideas2/delete_all?scope=all"
    title="" class="button redB" id="delete_button">
    <span>Delete All</span>
</a>
```

#### Malcius site&#x20;

```markup
<html>
    <body>
        <p>We are experiencing some technical problems. Our website is expected to be back online shortly. We apologize for the inconvenience.</p>
        <img src="https://tradeidea2.codebashing.com/ideas2/delete_all?scope=all" width="0" height="0" border="0">
    </body>
</html>
```



It is a bad practice to implement Update, Create, and Delete operations that rely on user-supplied input - via HTTP GET requests. This is the example of misuse of HTTP methods.

In case of TradeIDEAS2 application, the developers have wrongly assigned GET method to invoke the Delete All function. However, best practice suggests that GET method be only used for retrieving data, while POST, PUT, PATCH, and/or DELETE methods are used for all actions that modify application state.

Many web development frameworks also enforce the use of correctly assigned HTTP methods by default. In general, for a framework-agnostic approach, the following model is in line with the best practice for CRUD operations in a database-centric application:

1. Use HTTP GET for Read operations (SQL SELECT)
2. Use HTTP PUT for Update operations (SQL UPDATE)
3. Use HTTP POST for Create operations (SQL INSERT)
4. Use HTTP DELETE for Delete operations (SQL DELETE)
