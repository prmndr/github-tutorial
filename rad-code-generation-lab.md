# Practical Lab: Rapid Application Development (RAD) and Code Generation

## Objective
In this lab, you will learn about Rapid Application Development (RAD) principles and how to implement code generation techniques. You'll create a simple CRUD (Create, Read, Update, Delete) application using Python and Flask, demonstrating how RAD and code generation can speed up the development process.

## Prerequisites
- Basic understanding of Python programming
- Familiarity with web concepts (HTTP, APIs)
- Python 3.7+ installed on your local machine
- Basic command-line operations

## Estimated Time
2 hours

## Use Case
You're a software developer tasked with quickly creating a prototype for a user management system. The system should allow creating, reading, updating, and deleting user records. You need to develop this rapidly, showcasing the principles of RAD and utilizing code generation techniques.

## Steps

### 1. Set up the project structure

1. Create a new directory for your project:
   ```
   mkdir rad-code-gen-lab
   cd rad-code-gen-lab
   ```

2. Create a virtual environment and activate it:
   ```
   python -m venv venv
   source venv/bin/activate  # On Windows, use: venv\Scripts\activate
   ```

3. Install required packages:
   ```
   pip install flask flask-sqlalchemy
   ```

4. Create the following file structure:
   ```
   rad-code-gen-lab/
   ├── app.py
   ├── models.py
   └── templates/
       └── index.html
   ```

### 2. Define the data model

Create `models.py` with the following content:

```python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
```

### 3. Implement code generation for CRUD operations

Create `app.py` with the following content:

```python
from flask import Flask, request, jsonify, render_template
from models import db, User

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///users.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db.init_app(app)

def generate_crud_routes(model):
    name = model.__name__.lower()

    @app.route(f'/api/{name}', methods=['POST'])
    def create():
        data = request.json
        new_item = model(**data)
        db.session.add(new_item)
        db.session.commit()
        return jsonify({f'{name}_id': new_item.id}), 201

    @app.route(f'/api/{name}/<int:id>', methods=['GET'])
    def read(id):
        item = model.query.get_or_404(id)
        return jsonify({name: {column.name: getattr(item, column.name) for column in item.__table__.columns}})

    @app.route(f'/api/{name}/<int:id>', methods=['PUT'])
    def update(id):
        item = model.query.get_or_404(id)
        data = request.json
        for key, value in data.items():
            setattr(item, key, value)
        db.session.commit()
        return jsonify({name: {column.name: getattr(item, column.name) for column in item.__table__.columns}})

    @app.route(f'/api/{name}/<int:id>', methods=['DELETE'])
    def delete(id):
        item = model.query.get_or_404(id)
        db.session.delete(item)
        db.session.commit()
        return '', 204

    @app.route('/api/user', methods=['GET'])
    def list_users():
        users = User.query.all()
        return jsonify([{column.name: getattr(user, column.name) for column in user.__table__.columns} for user in users])
# Generate CRUD routes for User model
generate_crud_routes(User)

@app.route('/')
def index():
    return render_template('index.html')

if __name__ == '__main__':
    with app.app_context():
        db.create_all()
    app.run(debug=True)
```

### 4. Create a simple front-end

Create `templates/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Management</title>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
</head>
<body>
    <h1>User Management</h1>
    <form id="userForm">
        <input type="text" id="name" placeholder="Name" required>
        <input type="email" id="email" placeholder="Email" required>
        <button type="submit">Add User</button>
    </form>
    <div id="userList"></div>

    <script>
        const API_URL = '/api/user';

        async function addUser(event) {
            event.preventDefault();
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            try {
                await axios.post(API_URL, { name, email });
                document.getElementById('userForm').reset();
                fetchUsers();
            } catch (error) {
                console.error('Error adding user:', error);
            }
        }

        async function fetchUsers() {
        const userList = document.getElementById('userList');
        userList.innerHTML = 'Loading...';
        try {
        const response = await axios.get('/api/user');
        userList.innerHTML = '<h2>Users:</h2><ul>' + 
            response.data.map(user => `<li>${user.name} (${user.email})</li>`).join('') +
            '</ul>';
        } catch (error) {
        console.error('Error fetching users:', error);
        userList.innerHTML = 'Error fetching users';
        }
    }

        async function fetchUsers() {
            const userList = document.getElementById('userList');
            userList.innerHTML = 'Loading...';
            try {
                const response = await axios.get(API_URL);
                userList.innerHTML = JSON.stringify(response.data, null, 2);
            } catch (error) {
                console.error('Error fetching users:', error);
                userList.innerHTML = 'Error fetching users';
            }
        }

        document.getElementById('userForm').addEventListener('submit', addUser);
        fetchUsers();
    </script>
</body>
</html>
```

### 5. Run and test the application

1. Start the Flask application:
   ```
   python app.py
   ```

2. Open a web browser and navigate to `http://localhost:5000`

3. Use the form to add users and see the list update.

4. Test the API endpoints using cURL or a tool like Postman:
   - Create: `curl -X POST -H "Content-Type: application/json" -d "{\"name\":\"CD\",\"email\":\"cd@example.com\"}" http://localhost:5000/api/user`
   - Read: `curl http://localhost:5000/api/user/1`
   - Update: `curl -X PUT -H "Content-Type: application/json" -d '{"name":"Jane Doe"}' http://localhost:5000/api/user/1`
   - Delete: `curl -X DELETE http://localhost:5000/api/user/1`

### 6. Analyze RAD and Code Generation aspects

1. RAD Principles demonstrated:
   - Rapid prototyping: We quickly created a functioning CRUD application.
   - Iterative development: The structure allows for easy additions and modifications.
   - Reusability: The `generate_crud_routes` function can be used for multiple models.

2. Code Generation aspects:
   - The `generate_crud_routes` function dynamically creates CRUD routes for any given model.
   - This approach reduces repetitive code and allows for rapid development of new features.

## Conclusion
In this lab, you've learned how to apply Rapid Application Development principles and implement a simple code generation technique. By using these approaches, you were able to quickly create a functional CRUD application with minimal boilerplate code.

## Additional Exercises
1. Add more fields to the User model and observe how the generated CRUD operations adapt.
2. Create a new model (e.g., Product) and use the same code generation function to create its CRUD routes.
3. Implement input validation and error handling in the generated routes.
4. Extend the front-end to support updating and deleting users.
5. Implement pagination for the user list API.

Remember to explore Flask, SQLAlchemy, and Python documentation for more advanced features and best practices in web development and code generation techniques.

