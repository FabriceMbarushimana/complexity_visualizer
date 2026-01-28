# Algorithm Complexity Visualizer

A Flask-based API that analyzes and visualizes the time complexity of various algorithms. This tool measures execution times across different input sizes and returns the results along with visual graphs encoded in Base64 format.

  
## Installation

### Prerequisites
- Python 3.x installed on your system

### Step 1: Create a Virtual Environment

In project directory, create a virtual environment:

```bash
python -m venv .venv
```

### Step 2: Activate the Virtual Environment

**On Windows (PowerShell):**
```powershell
.\.venv\Scripts\Activate.ps1
```

**On Windows (Command Prompt):**
```cmd
.\.venv\Scripts\activate.bat
```

**On Linux/Mac:**
```bash
source .venv/bin/activate
```

### Step 3: Install Dependencies

With the virtual environment activated, install the required packages:

```bash
pip install flask numpy matplotlib
```

### Step 4: Verify Installation

Verify that Flask is installed correctly:

```bash
pip show flask
```

Or test with Python:

```bash
python -c "import flask; print(flask.__version__)"
```

### Running the Application

Once everything is installed, you can run the Flask app with:

```bash
python app.py
```

The server will be available at `http://localhost:3000`

---

## API Documentation

### Base URL

```
http://localhost:3000
```

### Endpoints

#### 1. Home

**GET** `/`

Returns basic API information and available options.

**Response Example:**

```json
{
  "available_algorithms": ["bubble", "linear", "binary", "nested"],
  "example": "/analyze?algo=bubble&n=1000&steps=10"
}
```

---

#### 2. Analyze Algorithm

**GET** `/analyze`

Analyzes the time complexity of a specified algorithm with given parameters.

**Query Parameters:**

| Parameter | Type    | Required | Description                                    |
|-----------|---------|----------|------------------------------------------------|
| `algo`    | string  | Yes      | Algorithm key: `bubble`, `linear`, `binary`, `nested` |
| `n`       | integer | Yes      | Maximum input size (must be positive)          |
| `steps`   | integer | Yes      | Step increment (must be positive, ≤ n)         |

**Available Algorithms:**

| Key      | Algorithm Name | Time Complexity |
|----------|----------------|-----------------|
| `bubble` | Bubble Sort    | O(n²)           |
| `linear` | Linear Search  | O(n)            |
| `binary` | Binary Search  | O(log n)        |
| `nested` | Nested Loops   | O(n²)           |

**Request Example:**

```
GET /analyze?algo=bubble&n=1000&steps=10
```

This will:
- Run Bubble Sort with input sizes: 10, 20, 30, ..., 1000
- Measure execution time for each input size
- Generate a time complexity graph
- Return all data in JSON format

**Response Example:**

```json
{
  "algo": "Bubble Sort",
  "items": "1000",
  "steps": "10",
  "start_time": 1706450400000,
  "end_time": 1706450403500,
  "total_time_ms": 3500,
  "time_complexity": "O(n²)",
  "data_points": 100,
  "path_to_graph": "C:/Users/.../graphs/bubble_sort_20260128_143025_a1b2c3d4.png",
  "download_url": "/download/bubble_sort_20260128_143025_a1b2c3d4.png",
  "graph_base64": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAA..."
}
```

**Response Fields:**

| Field            | Type    | Description                                              |
|------------------|---------|----------------------------------------------------------|
| `algo`           | string  | Full name of the analyzed algorithm                      |
| `items`          | string  | Maximum input size (n value)                             |
| `steps`          | string  | Step increment used                                      |
| `start_time`     | integer | Unix timestamp in milliseconds when analysis started     |
| `end_time`       | integer | Unix timestamp in milliseconds when analysis completed   |
| `total_time_ms`  | integer | Total analysis duration in milliseconds                  |
| `time_complexity`| string  | Theoretical time complexity (Big O notation)             |
| `data_points`    | integer | Number of data points collected                          |
| `path_to_graph`  | string  | Absolute file path where the graph image was saved       |
| `download_url`   | string  | URL endpoint to download the saved graph                 |
| `graph_base64`   | string  | Base64 encoded PNG image of the complexity graph         |

**Error Responses:**

**Missing Algorithm Parameter (400)**
```json
{
  "error": "Missing required parameter: algo"
}
```

**Unknown Algorithm (400)**
```json
{
  "error": "Unknown algorithm: quicksort",
  "available_algorithms": ["bubble", "linear", "binary", "nested"]
}
```

**Invalid N Parameter (400)**
```json
{
  "error": "Missing or invalid parameter: n (must be positive integer)"
}
```

**Invalid Steps Parameter (400)**
```json
{
  "error": "Missing or invalid parameter: steps (must be positive integer)"
}
```

**Steps Greater Than N (400)**
```json
{
  "error": "steps cannot be greater than n"
}
```

---

#### 3. List Algorithms

**GET** `/algorithms`

Returns a list of all available algorithms with their details.

**Response Example:**

```json
{
  "algorithms": [
    {
      "key": "bubble",
      "name": "Bubble Sort",
      "complexity": "O(n²)"
    },
    {
      "key": "linear",
      "name": "Linear Search",
      "complexity": "O(n)"
    },
    {
      "key": "binary",
      "name": "Binary Search",
      "complexity": "O(log n)"
    },
    {
      "key": "nested",
      "name": "Nested Loops",
      "complexity": "O(n²)"
    }
  ]
}
```

---

#### 4. Download Graph

**GET** `/download/<filename>`

Downloads a saved graph image directly.

**Request Example:**

```
GET /download/bubble_sort_20260128_143025_a1b2c3d4.png
```

**Response:**

Returns the PNG file as a downloadable attachment.

**Error Response (404):**

```json
{
  "error": "File not found"
}
```

---

#### 5. List Saved Graphs

**GET** `/graphs`

Returns a list of all saved graph images.

**Response Example:**

```json
{
  "graphs": [
    {
      "filename": "bubble_sort_20260128_143025_a1b2c3d4.png",
      "download_url": "/download/bubble_sort_20260128_143025_a1b2c3d4.png"
    },
    {
      "filename": "linear_search_20260128_143100_e5f6g7h8.png",
      "download_url": "/download/linear_search_20260128_143100_e5f6g7h8.png"
    }
  ],
  "total": 2
}
```

---

## Usage Examples

### Using cURL

**Analyze Bubble Sort:**
```bash
curl "http://localhost:3000/analyze?algo=bubble&n=1000&steps=10"
```

**Analyze Linear Search:**
```bash
curl "http://localhost:3000/analyze?algo=linear&n=5000&steps=100"
```

**Analyze Binary Search:**
```bash
curl "http://localhost:3000/analyze?algo=binary&n=10000&steps=500"
```

**Analyze Nested Loops:**
```bash
curl "http://localhost:3000/analyze?algo=nested&n=500&steps=10"
```

### Using Python

```python
import requests
import base64

# Make API request
response = requests.get(
    'http://localhost:3000/analyze',
    params={'algo': 'bubble', 'n': 1000, 'steps': 10}
)

data = response.json()

# Print results
print(f"Algorithm: {data['algo']}")
print(f"Time Complexity: {data['time_complexity']}")
print(f"Total Analysis Time: {data['total_time_ms']}ms")

# Save the graph image
if 'graph_base64' in data:
    # Remove the data URL prefix
    image_data = data['graph_base64'].split(',')[1]
    with open('complexity_graph.png', 'wb') as f:
        f.write(base64.b64decode(image_data))
    print("Graph saved as complexity_graph.png")
```

### Using JavaScript (Browser)

```javascript
fetch('http://localhost:3000/analyze?algo=bubble&n=1000&steps=10')
  .then(response => response.json())
  .then(data => {
    console.log('Algorithm:', data.algo);
    console.log('Time Complexity:', data.time_complexity);
    console.log('Total Time:', data.total_time_ms, 'ms');
    
    // Display the graph in an img element
    const img = document.createElement('img');
    img.src = data.graph_base64;
    document.body.appendChild(img);
  });
```

---

## How It Works

### Analysis Process

1. **Parameter Validation**: The API validates all query parameters
2. **Input Size Generation**: Creates a list of input sizes from `steps` to `n` with `steps` increment
3. **Time Measurement**: For each input size:
   - Records start time
   - Executes the algorithm
   - Records end time
   - Calculates execution duration
4. **Graph Generation**: Creates a matplotlib plot showing input size vs. execution time
5. **Response Building**: Packages all data including the Base64-encoded graph

### Algorithm Implementations

#### Bubble Sort - O(n²)
- Compares adjacent elements and swaps if out of order
- Repeats until no swaps needed
- Worst case: n² comparisons

#### Linear Search - O(n)
- Iterates through each element sequentially
- Returns when target is found
- Worst case: n iterations

#### Binary Search - O(log n)
- Works on sorted arrays
- Divides search space in half each iteration
- Very efficient for large datasets

#### Nested Loops - O(n²)
- Two nested loops, each iterating n times
- Demonstrates quadratic time complexity
- Similar behavior to matrix operations

---

## Graph Output

The generated graph includes:
- **X-axis**: Input size (n)
- **Y-axis**: Running time in seconds
- **Title**: Algorithm name and theoretical complexity
- **Data Points**: Actual measured times (connected by lines)
- **Shaded Area**: Visual representation of time growth
- **Statistics Box**: Average and maximum execution times

---

## Performance Considerations

| Algorithm | Recommended Max N | Notes                              |
|-----------|-------------------|------------------------------------|
| `bubble`  | 5000              | Very slow for large inputs         |
| `linear`  | 100000            | Scales linearly                    |
| `binary`  | 1000000           | Very fast, logarithmic growth      |
| `nested`  | 2000              | Quadratic, avoid large inputs      |

> ⚠️ **Warning**: Using very large values of `n` with O(n²) algorithms may cause long response times or timeouts.

---

## Project Structure

```
complexity_visualizer/
│
├── app.py                    # Main Flask application
├── factorial.py              # Algorithm implementations
├── graphs/                   # Generated graph images
├── API_documentation.md      # Detailed API documentation
├── install.md               # Installation guide
├── README.md                # This file
└── .venv/                   # Virtual environment (created during setup)
```

---

## Dependencies

- **Flask** - Web framework for the API
- **NumPy** - Numerical operations
- **Matplotlib** - Graph generation

Install all dependencies with:
```bash
pip install flask numpy matplotlib
```

---

## License

This project is available for educational purposes.

---

## Contributing

Feel free to submit issues or pull requests to improve the project.

---

## Support

For questions or issues, please refer to the API documentation or check the application logs.
