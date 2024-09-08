# End-User Monitoring Types: Practical Guide for B.Tech Students

## Objective
To understand and implement basic versions of different types of end-user monitoring: Real User Monitoring (RUM), Synthetic Monitoring, and Session Replay using Python and create a simple web application to demonstrate these monitoring capabilities.

## Files to Create
1. `app.py`: Main Flask application
2. `rum.py`: Real User Monitoring module
3. `synthetic.py`: Synthetic Monitoring module
4. `session_replay.py`: Session Replay module
5. `templates/index.html`: HTML template for the web interface
6. `static/style.css`: CSS file for styling
7. `requirements.txt`: List of required Python packages

## Step-by-Step Guide

### 1. Set up the project structure
Create a new directory for your project and navigate into it:

```
mkdir end_user_monitoring_types
cd end_user_monitoring_types
```

Create the necessary subdirectories:

```
mkdir templates static
```

### 2. Create `requirements.txt`
Create a file named `requirements.txt` with the following content:

```
Flask==2.0.1
requests==2.26.0
selenium==3.141.0
```

Install the requirements:

```
pip install -r requirements.txt
```

### 3. Create `app.py`
Create a file named `app.py` with the following content:

```python
from flask import Flask, render_template, jsonify, request
from rum import capture_rum_data
from synthetic import run_synthetic_test
from session_replay import capture_session_data

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/rum', methods=['POST'])
def rum():
    data = request.json
    result = capture_rum_data(data)
    return jsonify(result)

@app.route('/synthetic')
def synthetic():
    result = run_synthetic_test()
    return jsonify(result)

@app.route('/session_replay', methods=['POST'])
def session_replay():
    data = request.json
    result = capture_session_data(data)
    return jsonify(result)

if __name__ == '__main__':
    app.run(debug=True)
```

### 4. Create `rum.py`
Create a file named `rum.py` with the following content:

```python
import time

def capture_rum_data(data):
    # In a real-world scenario, you would store this data in a database
    print(f"RUM Data: {data}")
    return {
        "timestamp": time.time(),
        "status": "captured"
    }
```

### 5. Create `synthetic.py`
Create a file named `synthetic.py` with the following content:

```python
import requests
import time

def run_synthetic_test():
    start_time = time.time()
    response = requests.get("http://example.com")
    end_time = time.time()
    
    return {
        "url": "http://example.com",
        "status_code": response.status_code,
        "response_time": end_time - start_time
    }
```

### 6. Create `session_replay.py`
Create a file named `session_replay.py` with the following content:

```python
import time

def capture_session_data(data):
    # In a real-world scenario, you would store this data in a database
    print(f"Session Data: {data}")
    return {
        "timestamp": time.time(),
        "status": "captured"
    }
```

### 7. Create `templates/index.html`
Create a file named `index.html` in the `templates` directory with the following content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>End-User Monitoring Types</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h1>End-User Monitoring Types Dashboard</h1>
    
    <div id="rum">
        <h2>Real User Monitoring</h2>
        <button onclick="captureRUM()">Simulate RUM Data</button>
        <p id="rum-result"></p>
    </div>
    
    <div id="synthetic">
        <h2>Synthetic Monitoring</h2>
        <button onclick="runSyntheticTest()">Run Synthetic Test</button>
        <p id="synthetic-result"></p>
    </div>
    
    <div id="session-replay">
        <h2>Session Replay</h2>
        <button onclick="captureSessionReplay()">Simulate Session Capture</button>
        <p id="session-replay-result"></p>
    </div>

    <script>
        function captureRUM() {
            const data = {
                pageLoadTime: Math.random() * 1000,
                userAgent: navigator.userAgent
            };
            
            fetch('/rum', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(data)
            })
            .then(response => response.json())
            .then(result => {
                document.getElementById('rum-result').textContent = JSON.stringify(result);
            });
        }

        function runSyntheticTest() {
            fetch('/synthetic')
            .then(response => response.json())
            .then(result => {
                document.getElementById('synthetic-result').textContent = JSON.stringify(result);
            });
        }

        function captureSessionReplay() {
            const data = {
                events: [
                    { type: 'click', target: 'button', timestamp: Date.now() },
                    { type: 'input', target: 'textfield', value: 'example', timestamp: Date.now() }
                ]
            };
            
            fetch('/session_replay', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(data)
            })
            .then(response => response.json())
            .then(result => {
                document.getElementById('session-replay-result').textContent = JSON.stringify(result);
            });
        }
    </script>
</body>
</html>
```

### 8. Create `static/style.css`
Create a file named `style.css` in the `static` directory with the following content:

```css
body {
    font-family: Arial, sans-serif;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
}

h1, h2 {
    color: #333;
}

button {
    background-color: #4CAF50;
    border: none;
    color: white;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
}

#rum, #synthetic, #session-replay {
    background-color: #f0f0f0;
    padding: 20px;
    border-radius: 5px;
    margin-bottom: 20px;
}
```

### 9. Run the application
To run the application, execute the following command in your terminal:

```
python app.py
```

Open a web browser and navigate to `http://localhost:5000` to see the end-user monitoring types dashboard.

## Explanation of End-User Monitoring Types

### 1. Real User Monitoring (RUM)
RUM collects data from actual users interacting with your website or application. It provides insights into real-world performance and user experience.

In our example:
- `rum.py` simulates capturing RUM data such as page load time and user agent.
- The web interface allows you to simulate sending RUM data to the server.

### 2. Synthetic Monitoring
Synthetic monitoring involves simulating user interactions with your website or application to proactively identify issues and measure performance.

In our example:
- `synthetic.py` performs a simple synthetic test by making a request to example.com and measuring the response time.
- The web interface allows you to trigger a synthetic test and view the results.

### 3. Session Replay
Session replay captures and recreates user sessions, allowing you to see how users interact with your application and identify usability issues or bugs.

In our example:
- `session_replay.py` simulates capturing session data, including user events and their timestamps.
- The web interface allows you to simulate capturing a session with some example events.

## Tasks for Students
1. Run the application and experiment with each type of monitoring.
2. Enhance the RUM module to capture more real user data (e.g., time spent on page, number of clicks).
3. Improve the synthetic monitoring module to test multiple URLs and compare their performance.
4. Extend the session replay module to capture more detailed user interactions and visualize them in the web interface.
5. Implement basic storage for the captured data (e.g., using SQLite) and create a page to view historical data.
6. Add error handling and logging to make the application more robust.

## Conclusion
This practical guide introduces students to different types of end-user monitoring: Real User Monitoring, Synthetic Monitoring, and Session Replay. By implementing basic versions of these monitoring types, students can gain a practical understanding of how they work and how they can be used to improve web application performance and user experience.
