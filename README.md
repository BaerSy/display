


Develop Simple Web Application
with Node.js and MySQL



Backend
Express, Node.js, MySql

Frontend
HTML, Javascript, Angular






Develop a Simple Web Application with Node.js and MySql
Purpose: Understand how to develop web application using node.js, mysql, express, html etc.
Backend:  Express, Node.js, MySql
Frontend: HTML, Javascript, Angular

Build a development environment
Install Node.js
https://nodejs.org/en/download/
 
          After install, you can try “npm version”
           

2.	Install MySql
It recommends to install MySql 5.7.x community Edition.
https://downloads.mysql.com/archives/get/p/25/file/mysql-installer-community-5.7.30.0.msi
When installing it, make sure the password for root is default as “123456”.  
You can install MySql Workbench to view data from  your MySQL
https://downloads.mysql.com/archives/get/p/8/file/mysql-workbench-community-6.3.10-winx64.msi
You can use “Command Prompt” cmd to test your installation:
>mysql –u root –p 
   
Enter password “123456”, then you will enter the system.
mysql> show database;
 
If you want to see inside of which database, just use command “use”.
 
How many tables in the database “mysql”: 
               
 What’s structure of table “func”:

 

3.	Install Express
https://codingstatus.com/how-to-install-express-application-using-express-generator-tool/
         Create a project folder, for example “myapp”
        Enter this folder
npm install -g express-generator
npx express --view=ejs 
 
         
You can test your installation:
Run “npm start”
Then in the browser http://localhost:3000. You will see
 



Practice 1
How to Insert Form Data Into the Table Using Node.js and MySQL
https://codingstatus.com/how-to-insert-form-data-into-the-table-using-node-js-and-mysql/
Create a database – “NodeApp” with a table of “Users”
Create a txt file – createDBTable.txt 
          drop database if exists NodeApp;
 create database NodeApp CHARACTER SET utf8 COLLATE utf8_general_ci;
 use NodeApp;
  CREATE TABLE IF NOT EXISTS USERS (id int(10) UNSIGNED PRIMARY KEY NOT NULL  AUTO_INCREMENT, 
             fullName varchar(255) DEFAULT NULL,
             emailAddress varchar(255) DEFAULT NULL,
             city varchar(255) DEFAULT NULL,
            country varchar(50) DEFAULT NULL
                                 
) DEFAULT CHARSET=utf8;


(b)	 Login to mysql
           >>  mysql –u root –p < createDBTable.txt
             Enter password: 123456
You can view database and table. This time you should not see any data inside table.
            
	
            

2.	Create database.js to connect databse.  Database.js will be located in the folder of “myapp”.
 var mysql = require('mysql');
 var conn = mysql.createConnection({
  host: 'localhost', // Replace with your host name
  user: 'root',      // Replace with your database username
  password: '123456',      // Replace with your database password
  database: 'nodeapp' // // Replace with your database Name
  }); 
  conn.connect(function(err) {
  if (err) throw err;
     console.log('Database is connected successfully !');
  });
  module.exports = conn;

3.	Create an HTML Form in Node.js App
Now, Use the following HTML code in the views/user.ejs to insert data using Node.js & MySQL.
File Name: user.ejs
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
* {
  box-sizing: border-box;}
.user-detail {
    height: 100vh;
    border: 2px solid #f1f1f1;
    padding: 16px;
    background-color: white;
    width: 30%;}
input{
  width: 100%;
  padding: 15px;
  margin: 5px 0 22px 0;
  display: inline-block;
  border: none;
  background: #f1f1f1;}
input[type=text]:focus, input[type=password]:focus {
    background-color: #ddd;
    outline: none;}
button[type=submit] {
    background-color: #434140;
    color: #ffffff;
    padding: 10px 20px;
    margin: 8px 0;
    border: none;
    cursor: pointer;
    width: 100%;
    opacity: 0.9;
    font-size: 20px;}
label{
  font-size: 18px;;}
button[type=submit]:hover {
  background-color:#3d3c3c;}
</style>
</head>
<body>
<!--====form section start====-->
<div class="user-detail">
    <h2>Create User Data</h2>
    <form action="/user/create" method="POST">
          <label>Full Name</label>
          <input type="text" placeholder="Enter Full Name" name="fullName" required>
          <label>Email Address</label>
          <input type="email" placeholder="Enter Email Address" name="emailAddress" required>
          <label>City</label>
          <input type="city" placeholder="Enter Full City" name="city" required>
          <label>Country</label>
          <input type="text" placeholder="Enter Full Country" name="country" required>
          <button type="submit">Submit</button>
        </div>
</div>
<!--====form section start====-->
</body>
</html>


This form page will open through http://localhost:3000/user URL.
Use the following script in the routes/user.js file and lo to load the HTML form.
router.get('/', function(req, res, next) {
res.render('user');
});
You must declare the following points:
Form method must be POST like method="POST"
Form action contains user/create/ ( It is created in routes/user.js to post form data ) like action="user/create".
You should declare field name the same as the column name of the users table.
 Form Field	Field Name
Full Name	name=”fullName”
Email Address	name=”emailAddress”
City	name=”city”
Country	name=”country”
4.	Write MySQL Query in Node.js to Insert Data
	
First of all, Include the database connection file in the routes/user.js file
var db=require('../database');

Complete Script: user.js
var express = require('express');
var router = express.Router();
var db=require('../database');

router.get('/', function(req, res, next) { 
res.render('user'); 

});

router.post('/create', function(req, res, next) {
  
  // store all the user input data
  const userDetails=req.body;
 
  // insert user data into users table
  var sql = 'INSERT INTO users SET ?';
  db.query(sql, userDetails,function (err, data) { 
      if (err) throw err;
         console.log("User dat is inserted successfully "); 
  });
 res.redirect('/user');  // redirect to user form page after inserting the data

}); 
module.exports = router;

Include the user.js file in app.js root file
You have to include user.js route file in  app.js the root file as.
var userRouter = require('./routes/user');
app.use('/user',userRouter);

5.	Start nodejs server
>>npm start
 
Apply Web Browser 

 
Practice 2
How to display Data from MySQL database table in Node.js
This practice will display all data added into the database in Practice 1.

 
  
Create the folder of nodejs_display
cd nodejs_display
Install Express as described in Step 3 of “build development environment”
Create database.js to connect databse.  Database.js will be located in the folder of “nodejs_display”.
var mysql = require('mysql');
 var conn = mysql.createConnection({
  host: 'localhost', // Replace with your host name
  user: 'root',      // Replace with your database username
  password: '123456',      // Replace with your database password
  database: 'nodeapp' // // Replace with your database Name
  }); 
  conn.connect(function(err) {
  if (err) throw err;
     console.log('Database is connected successfully !');
  });
 module.exports = conn;

5.	Write MySQL Query in Node.js to Fetch Data
Before Displaying MySQL data, You have to do the following things
Include the database connection file database.js in the routes/user.js file.
var db=require('../database');
Write the following script in the routes/user.js file to fetch data from the MySQL users table.
router.get('/user-list', function(req, res, next) {
var sql='SELECT * FROM users';
db.query(sql, function (err, data, fields) {
if (err) throw err;
res.render('user-list', { title: 'User List', userData: data});
});
});
Include routes/user.js file in the root file app.js
var userRouter = require('./routes/user');
app.use('/user',userRouter);

Complete Script: routes/user.js
var express = require('express');
var router = express.Router();
var db=require('../database');
// another routes also appear here
// this script to fetch data from MySQL databse table
router.get('/user-list', function(req, res, next) {
var sql='SELECT * FROM users';
db.query(sql, function (err, data, fields) {
if (err) throw err;
res.render('user-list', { title: 'User List', userData: data});
});
});
module.exports = router;

6.	Create HTML Table & Display data using Node.js
Create a file user-list.ejs in the views folder.
Now,  Use the following  HTML code and ejs Script to display data in the HTML table.

 
Complete HTML Table Code: views/user-list.ejs

<!DOCTYPE html>
<html lang="en">
<head>
  <title>Fetch using MySQL and Node.js</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    table, td, th {  
      border: 1px solid #ddd;
      text-align: left;
    }
    
    table {
      border-collapse: collapse;
      width: 50%;
     

    }
    .table-data{
        position: relative;
      left:150px;
      top:100px;
    }
    th, td {
      padding: 15px;
    }

    </style>
</head>
<body>
    <div class="table-data">
<h2>Display Data using Node.js & MySQL</h2>
    <table border="1">
        <tr>
            <th>S.N</th>
            <th>Full Name</th>
            <th>Email Address</th>
            <th>City</th>
            <th>Country</th>
            <th>Edit</th>
            <th>Delete</th>
        </tr>
        
        <%
        if(userData.length!=0){
        var i=1;
        userData.forEach(function(data){
        %>
        <tr>
            <td><%=i; %></td>
            <td><%=data.fullName %></td>
            <td><%=data.emailAddress %></td>
            <td><%=data.city %></td>
            <td><%=data.country %></td>
            <td><a href="/user/edit/<%=data.id%>">Edit</a></td>
            <td><a href="/user/delete/<%=data.id%>">Delete</a></td>
        </tr>
        <%  i++; }) %>
        <% } else{ %>
            <tr>
                <td colspan="7">No Data Found</td>
            </tr>
        <% } %>
    </table>
    </div>
</body>
</html>

You can see the above table with displaying data from users table by entering the following URL in the browser
http://localhost:3000/user/user-list

 
Reference
https://codingstatus.com/how-to-insert-form-data-into-the-table-using-node-js-and-mysql/
https://codingstatus.com/how-to-display-data-from-mysql-database-table-in-node-js/
https://github.com/codethereforam/express-mysql-demo
https://codingstatus.com/how-to-insert-data-into-mongodb-using-mongoose-and-node-js/
https://codingstatus.com/delete-mongodb-data-using-mongoose/
https://codingstatus.com/update-mongodb-data-using-mongoose/
https://www.youtube.com/watch?v=YYEC7ydDj4k
https://www.simplilearn.com/nodejs-tutorial-article

