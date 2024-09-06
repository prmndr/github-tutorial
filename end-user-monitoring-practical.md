# End-User Monitoring Practical Guide

## Objective
To set up a basic end-user monitoring system using Python and create a simple web application to demonstrate monitoring capabilities.

## Files to Create
1. `app.py`: Main Flask application
2. `monitor.py`: Monitoring module
3. `templates/index.html`: HTML template for the web interface
4. `static/style.css`: CSS file for styling
5. `requirements.txt`: List of required Python packages

## Step-by-Step Guide

### 1. Set up the project structure
Create a new directory for your project and navigate into it:

```
mkdir end_user_monitoring
cd end_user_monitoring
```

Create the necessary subdirectories:

```
mkdir templates static
```

### 2. Create `requirements.txt`
Create a file named `requirements.txt` with the following content:

```
Flask==2.0.1
psutil==5.8.0
```

Install the requirements:

```
pip install -r requirements.txt
```

### 3. Create `app.py`
Create a file named `app.py` with the following content:

```python
from flask import Flask, render_template, jsonify
from monitor import get_system_stats

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/stats')
def stats():
    return jsonify(get_system_stats())

if __name__ == '__main__':
    app.run(debug=True)
```

### 4. Create `monitor.py`
Create a file named `monitor.py` with the following content:

```python
import psutil

def get_system_stats():
    cpu_percent = psutil.cpu_percent(interval=1)
    memory = psutil.virtual_memory()
    disk = psutil.disk_usage('/')
    
    return {
        'cpu': cpu_percent,
        'memory': memory.percent,
        'disk': disk.percent
    }
```

### 5. Create `templates/index.html`
Create a file named `index.html` in the `templates` directory with the following content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>End-User Monitoring</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h1>End-User Monitoring Dashboard</h1>
    <div id="stats">
        <p>CPU Usage: <span id="cpu">-</span>%</p>
        <p>Memory Usage: <span id="memory">-</span>%</p>
        <p>Disk Usage: <span id="disk">-</span>%</p>
    </div>
    <script>
        function updateStats() {
            fetch('/stats')
                .then(response => response.json())
                .then(data => {
                    document.getElementById('cpu').textContent = data.cpu.toFixed(2);
                    document.getElementById('memory').textContent = data.memory.toFixed(2);
                    document.getElementById('disk').textContent = data.disk.toFixed(2);
                });
        }
        setInterval(updateStats, 5000);
        updateStats();
    </script>
</body>
</html>
```

### 6. Create `static/style.css`
Create a file named `style.css` in the `static` directory with the following content:

```css
body {
    font-family: Arial, sans-serif;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
}

h1 {
    text-align: center;
}

#stats {
    background-color: #f0f0f0;
    padding: 20px;
    border-radius: 5px;
}
```

### 7. Run the application
To run the application, execute the following command in your terminal:

```
python app.py
```

Open a web browser and navigate to `http://localhost:5000` to see the end-user monitoring dashboard.

## Explanation
- `app.py`: This is the main Flask application that serves the web interface and provides an API endpoint for system stats.
- `monitor.py`: This module uses the `psutil` library to gather system information such as CPU, memory, and disk usage.
- `index.html`: This is the HTML template for the web interface. It includes JavaScript to fetch and display real-time system stats.
- `style.css`: This file contains basic styling for the web interface.

## Tasks for Students
1. Run the application and observe the real-time system stats.
2. Modify the `monitor.py` file to include additional system information (e.g., network usage, battery status for laptops).
3. Update the web interface to display the new information you've added.
4. Implement error handling for cases where system information might not be available.
5. Add a feature to log the system stats to a file at regular intervals.

## Conclusion
This practical introduces students to the basics of end-user monitoring using Python. It demonstrates how to gather system information and present it in a web interface, providing a foundation for more advanced monitoring and analysis tasks.
