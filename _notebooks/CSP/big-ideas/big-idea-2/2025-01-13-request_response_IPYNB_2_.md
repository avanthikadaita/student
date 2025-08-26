---
toc: True
comments: False
layout: post
categories: ['CSP Big Idea 2']
title: API | Request | Response | Database
description: Frontend coding and backend design will always be related.  In building Views, frontend designs will work with the backend APIs, those APIs can work with databases.
courses: {'csp': {'week': 18}}
type: ccc
---

## View/CRUD Concepts

### Frontend Data Capture

HTML to obtain input is a key step in a frontend system that works with APIs that support database operations.

![Screen Concept]({{site.baseurl}}/images/crud/crud.png)

### HTML5 table is a way to organize input
- th labels
- td input types
- onclick starts an action

```html
<table>
    <tr>
        <th><label for="name">Name</label></th>
        <th><label for="email">Email</label></th>
        <th><label for="password">Password</label></th>
        <th><label for="phone">Phone</label></th>
    </tr>
    <tr>
        <td><input type="text" name="name" id="name" required></td>
        <td><input type="email" name="email" id="email" placeholder="abc@xyz.org" required></td>
        <td><input type="password" name="password" id="password" required></td>
        <td><input type="tel" name="phone_num" id="phone_num"
            pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}"
            placeholder="999-999-9999"></td>
        <td ><button onclick="create_User()" class="medium primary">Create</button></td>
    </tr>
</table>
```

<h3>Table</h3>
<table>
    <tr>
        <th><label for="name">Name</label></th>
        <th><label for="email">Email</label></th>
        <th><label for="password">Password</label></th>
        <th><label for="phone">Phone</label></th>
    </tr>
    <tr>
        <td><input type="text" name="name" id="name" required></td>
        <td><input type="email" name="email" id="email" placeholder="abc@xyz.org" required></td>
        <td><input type="password" name="password" id="password" required></td>
        <td><input type="tel" name="phone_num" id="phone_num"
            pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}"
            placeholder="999-999-9999"></td>
        <td ><button onclick="create_User()" class="medium primary">Create</button></td>
    </tr>
</table>

### An HTML5 form is another way to organize input
- form action vs onclick
- p tags
- labels
- input

```html
<form action="create_User()">
    <p><label>
        Name:
        <input type="text" name="name" id="name" required>
    </label></p>
    <p><label>
        User ID:
        <input type="text" name="uid" id="uid" required>
    </label></p>
    <p><label>
        Password:
        <input type="password" name="password" id="password" required>
        Verify Password:
        <input type="password" name="passwordV" id="passwordV" required>
    </label></p>
    <p><label>
        Phone:
        <input type="tel" name="phone_num" id="phone_num"
            pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}"
            placeholder="999-999-9999">
    </label></p>
    <p><label>
        Birthday:
        <input type="date" name="dob" id="dob">
    </label></p>
    <p>
        <button class="medium primary">Create</button>
    </p>
</form>
```

<h3>Form</h3>
<form>
    <p><label>
        Name:
        <input type="text" name="name" id="name" required>
    </label></p>
    <p><label>
        User ID:
        <input type="text" name="uid" id="uid" required>
    </label></p>
    <p><label>
        Password:
        <input type="password" name="password" id="password" required>
        Verify Password:
        <input type="password" name="passwordV" id="passwordV" required>
    </label></p>
    <p><label>
        Phone:
        <input type="tel" name="phone_num" id="phone_num"
            pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}"
            placeholder="999-999-9999">
    </label></p>
    <p><label>
        Birthday:
        <input type="date" name="dob" id="dob">
    </label></p>
    <p>
        <button class="medium primary">Create</button>
    </p>
</form>

## API Request and Response

> In an API database project, the key idea is to build a system for information exchange. For instance, ***user information*** exchange would require login, create, read, update, and delete operations in a Backend Database.

- **Login to existing account**: The request requires credentials, and the response returns a token.

![Login Concept]({{site.baseurl}}/images/crud/login.png)

- **Create new user records**: The request requires a token, body, and the response returns the created JSON object of the user from the database.

![Create Concept]({{site.baseurl}}/images/crud/create.png)

- **Read logged in user**: The request requires a valid token, and the response returns a the logged in JSON user object from the database.

![Read Concept]({{site.baseurl}}/images/crud/read.png)

- **Update user data**: The request requires a valid token, body, and the response returns the updated JSON object of the user from the database.

![Update Concept]({{site.baseurl}}/images/crud/update.png)

- **Delete a user record**: The request requires a valid Admin token, and the response only returns a 204 success status.

![Delete Concept]({{site.baseurl}}/images/crud/delete.png)

## Backend API

> In a Python Flask database project, the API endpoint serves as a middle layer between the frontend request and the backend database. The backend **receives requests** and **prepares responses**.

This is a list of common operations in these API endpoints:

- **Validates Input**: An API stops the "Garbage In" and sends error responses in case of garbage.
- **Query of Database**: A User SQLAlchemy command will query data according to the method requirement and the body of the request.
- **Class Method Call**: A User method will perform a CRUD operation on the database according to the request.
- **RESTful Response**: The return statement sends data back to the request.

### CRUD Handling

Observe and study this API in detail. Each endpoint contains comments explaining its functionality. A key element of this lesson is to learn the interactions between the API endpoint and the User table in the database.

```python
    class _CRUD(Resource):  # Users API operation for Create, Read, Update, Delete 
        def post(self): # Create method
            """
            Create a new user.

            Reads data from the JSON body of the request, validates the input, and creates a new user in the database.

            Returns:
                JSON response with the created user details or an error message.
            """
            
            # Read data for json body
            body = request.get_json()
            
            ''' Avoid garbage in, error checking '''
            # validate name
            name = body.get('name')
            password = body.get('password')
            if name is None or len(name) < 2:
                return {'message': f'Name is missing, or is less than 2 characters'}, 400
            
            # validate uid
            uid = body.get('uid')
            if uid is None or len(uid) < 2:
                return {'message': f'User ID is missing, or is less than 2 characters'}, 400
          
            # check if uid is a GitHub account
            _, status = GitHubUser().get(uid)
            if status != 200:
                return {'message': f'User ID {uid} not a valid GitHub account' }, 404
            
            ''' User object creation '''
            #1: Setup minimal User object using __init__ method
            user_obj = User(name=name, uid=uid)
            
            #2: Save the User object to the database using custom create method
            user = user_obj.create(body) # pass the body elements to be saved in the database
            if not user: # failure returns error message
                return {'message': f'Processed {name}, either a format error or User ID {uid} is duplicate'}, 400
            
            # return response, the created user details as a JSON object
            return jsonify(user.read())

        @token_required()
        def get(self):
            """
            Retrieve all users.

            Retrieves a list of all users in the database.

            Returns:
                JSON response with a list of user dictionaries.
            """
            # retrieve the current user from the token_required authentication check  
            current_user = g.current_user
            
            """ User SQLAlchemy query returning list of all users """
            users = User.query.all() # extract all users from the database
             
            # prepare a json list of user dictionaries
            json_ready = []  
            for user in users:
                user_data = user.read()
                if current_user.role == 'Admin' or current_user.id == user.id:
                    user_data['access'] = ['rw'] # read-write access control 
                else:
                    user_data['access'] = ['ro'] # read-only access control 
                json_ready.append(user_data)
            
            # return response, a list of user dictionaries in JSON format
            return jsonify(json_ready)
        
        @token_required()
        def put(self):
            """
            Update user details.

            Retrieves the current user from the token_required authentication check and updates the user details based on the JSON body of the request.

            Returns:
                JSON response with the updated user details or an error message.
            """
            
            # Retrieve the current user from the token_required authentication check
            current_user = g.current_user
            # Read data from the JSON body of the request
            body = request.get_json()

            ''' Admin-specific update handling '''
            if current_user.role == 'Admin':
                uid = body.get('uid')
                # Admin is updating themself
                if uid is None or uid == current_user.uid:
                    user = current_user 
                else: # Admin is updating another user
                    """ User SQLAlchemy query returning a single user """
                    user = User.query.filter_by(_uid=uid).first()
                    if user is None:
                        return {'message': f'User {uid} not found'}, 404
            else:
                # Non-admin can only update themselves
                user = current_user
                
            # Accounts are desired to be GitHub accounts, change must be validated 
            if body.get('uid') and body.get('uid') != user._uid:
                _, status = GitHubUser().get(body.get('uid'))
                if status != 200:
                    return {'message': f'User ID {body.get("uid")} not a valid GitHub account' }, 404
            
            # Update the User object to the database using custom update method
            user.update(body)
            
            # return response, the updated user details as a JSON object
            return jsonify(user.read())
        
        @token_required("Admin")
        def delete(self):
            """
            Delete a user.

            Deletes a user from the database based on the JSON body of the request. Only accessible by Admin users.

            Returns:
                JSON response with a success message or an error message.
            """
            body = request.get_json()
            uid = body.get('uid')
            
            """ User SQLAlchemy query returning a single user """
            user = User.query.filter_by(_uid=uid).first()
            
            # bad request
            if user is None:
                return {'message': f'User {uid} not found'}, 404
           
            # Read and then Delete the User object using custom methods
            user_json = user.read()
            user.delete()
            
            # 204 is the status code for delete with no json response
            return f"Deleted user: {user_json}", 204 # use 200 to test with Postman
```

## Response Handling

> Response handling goes back to the requester. This example illustrates data JavaScript may receive on a read and how it would construct a table that, in turn, could activate other Update and Delete actions.

- **JSON response is required**: It is hardcoded in this example. Typically, JSON will come from a JavaScript fetch.
- **JSON object is required**: In this case, a list of JSON objects. Each object allows access to elements in JSON using JavaScript dot notation (e.g., `user._name`).
- **DOM editing**: This is a significant part of the remainder of this example. DOM elements often nest inside other DOM elements. For instance, each `td` is nested in a `tr`. ***Find examples*** of DOM create and append in the code below.
- Notice the definition of ***table*** and build your own map or visual of how these elements are put together.

```html

<table>
  <thead>
  <tr>
    <th>Name</th>
    <th>ID</th>
    <th>Actions</th>
  </tr>
  </thead>
  <tbody id="table">
    <!-- javascript generated data -->
  </tbody>
</table>


<script>
// Static json, this can be used to test data prior to API and Model being ready
const json = '[{"_name": "Thomas Edison", "_uid": "toby"}, {"_name": "Nicholas Tesla", "_uid": "nick"}, {"_name": "John Mortensen", "_uid": "jm1021"}, {"_name": "Eli Whitney", "_uid": "eli"}, {"_name": "Hedy Lemarr", "_uid": "hedy"}]';

// Convert JSON string to JSON object
const data = JSON.parse(json);

// prepare HTML result container for new output
const table = document.getElementById("table");
data.forEach(user => {
    // build a row for each user
    const tr = document.createElement("tr");

    // td's to build out each column of data
    const name = document.createElement("td");
    const id = document.createElement("td");
    const action = document.createElement("td");
           
    // add content from user data          
    name.innerHTML = user._name; 
    id.innerHTML = user._uid; 

    // add action for update button
    var updateBtn = document.createElement('input');
    updateBtn.type = "button";
    updateBtn.className = "button";
    updateBtn.value = "Update";
    updateBtn.style = "margin-right:16px";
    updateBtn.onclick = function () {
      alert("Update: " + user._uid);
    };
    action.appendChild(updateBtn);

    // add action for delete button
    var deleteBtn = document.createElement('input');
    deleteBtn.type = "button";
    deleteBtn.className = "button";
    deleteBtn.value = "Delete";
    deleteBtn.style = "margin-right:16px"
    deleteBtn.onclick = function () {
      alert("Delete: " + user._uid);
    };
    action.appendChild(deleteBtn);  

    // add data to row
    tr.appendChild(name);
    tr.appendChild(id);
    tr.appendChild(action);

    // add row to table
    table.appendChild(tr);
});
</script>

```


<table>
  <thead>
  <tr>
    <th>Name</th>
    <th>ID</th>
    <th>Actions</th>
  </tr>
  </thead>
  <tbody id="table">
    <!-- javascript generated data -->
  </tbody>
</table>


<script>
// Static json, this can be used to test data prior to API and Model being ready
const json = '[{"_name": "Thomas Edison", "_uid": "toby"}, {"_name": "Nicholas Tesla", "_uid": "nick"}, {"_name": "John Mortensen", "_uid": "jm1021"}, {"_name": "Eli Whitney", "_uid": "eli"}, {"_name": "Hedy Lemarr", "_uid": "hedy"}]';

// Convert JSON string to JSON object
const data = JSON.parse(json);

// prepare HTML result container for new output
const table = document.getElementById("table");
data.forEach(user => {
    // build a row for each user
    const tr = document.createElement("tr");

    // td's to build out each column of data
    const name = document.createElement("td");
    const id = document.createElement("td");
    const action = document.createElement("td");
           
    // add content from user data          
    name.innerHTML = user._name; 
    id.innerHTML = user._uid; 

    // add action for update button
    var updateBtn = document.createElement('input');
    updateBtn.type = "button";
    updateBtn.className = "button";
    updateBtn.value = "Update";
    updateBtn.style = "margin-right:16px";
    updateBtn.onclick = function () {
      alert("Update: " + user._uid);
    };
    action.appendChild(updateBtn);

    // add action for delete button
    var deleteBtn = document.createElement('input');
    deleteBtn.type = "button";
    deleteBtn.className = "button";
    deleteBtn.value = "Delete";
    deleteBtn.style = "margin-right:16px"
    deleteBtn.onclick = function () {
      alert("Delete: " + user._uid);
    };
    action.appendChild(deleteBtn);  

    // add data to row
    tr.appendChild(name);
    tr.appendChild(id);
    tr.appendChild(action);

    // add row to table
    table.appendChild(tr);
});
</script>
