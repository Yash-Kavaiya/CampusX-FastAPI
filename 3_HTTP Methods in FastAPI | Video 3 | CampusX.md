# HTTP Methods in FastAPI | Video 3 | CampusX

## Project Overview

### Problem Statement
Traditional doctor-patient record management faces several challenges:
- **Physical Documentation**: Doctors provide handwritten prescriptions on clinic letterheads
- **Manual Storage**: Patients must maintain physical copies of all medical records
- **Risk of Loss**: Papers can be misplaced or damaged over time
- **Inefficient Retrieval**: Difficult to access historical medical data quickly
- **Doctor's Copy Issues**: Clinic copies can also be misplaced

### Solution: Digital Patient Management System
Create a **startup solution** that digitizes the entire patient record management process through a web application and API.

---

## Solution Architecture

### Core Components
1. **Web Application** (Frontend - not covered in this tutorial)
2. **FastAPI Backend** (Our focus)
3. **Data Storage** (JSON file for basic implementation, database for production)

### Patient Profile Structure
```json
{
  "id": "P001",
  "name": "Patient Name",
  "city": "City Name",
  "age": 30,
  "gender": "male/female",
  "height": 175,
  "weight": 70,
  "bmi": 22.86,
  "medical_info": "Additional medical details"
}
```

### Key Features
- **Create**: Add new patient records
- **Read**: View all patients or specific patient details
- **Update**: Modify existing patient information
- **Delete**: Remove patient records

---

## Software Classification

### 1. Static vs Dynamic Software
![image](https://github.com/user-attachments/assets/2d839488-2004-4db8-baa0-9851576916cb)
#### Static Software
- **Definition**: Software with minimal user interaction
- **Purpose**: Primarily for information consumption
- **Examples**: 
  - Calendar applications
  - Clock applications
  - Basic information displays
- **Interaction**: One-way communication (software → user)

#### Dynamic Software
- **Definition**: Software with extensive user interaction
- **Purpose**: Two-way communication and data manipulation
- **Examples**:
  - Microsoft Excel
  - Microsoft Word
  - PowerPoint
  - Photo editing software
- **Interaction**: Two-way communication (user ↔ software)

### 2. Static vs Dynamic Websites



#### Static Websites
- **Characteristics**: Limited user interaction
- **Examples**: Blogs, government websites, information portals
- **Usage**: Primarily for content consumption

#### Dynamic Websites
- **Characteristics**: Extensive user interaction
- **Examples**: Instagram, Zomato, Facebook
- **Features**: User registration, content creation, real-time updates

---

## HTTP Methods & CRUD Operations

### The Four Fundamental Operations (CRUD)

All dynamic software and websites support only **4 types of interactions**:

1. **C**reate
2. **R**ead (Retrieve)
3. **U**pdate
4. **D**elete


![image](https://github.com/user-attachments/assets/f2436ac9-2ddb-4172-bee5-189dbf1216e0)

### HTTP Methods Mapping

| CRUD Operation | HTTP Method | Purpose | Example |
|---------------|-------------|---------|---------|
| **Create** | POST | Add new data | Register new user, create post |
| **Read** | GET | Retrieve data | View profile, load page |
| **Update** | PUT | Modify existing data | Edit profile, update post |
| **Delete** | DELETE | Remove data | Delete account, remove post |

### Real-World Examples

#### Instagram Operations
- **Create**: Upload new photo, write comment
- **Read**: Scroll feed, view profile
- **Update**: Edit profile, modify caption
- **Delete**: Remove post, delete comment

#### Zomato Operations
- **Create**: Place new order
- **Read**: View past orders, browse restaurants
- **Update**: Change delivery address
- **Delete**: Remove saved address

---

## Project Implementation

### Project Structure
```
patient-management-api/
├── main.py                 # FastAPI application
├── patients.json          # Data storage (JSON file)
├── requirements.txt       # Dependencies
└── .env/                 # Virtual environment
```

### Dependencies
- **FastAPI**: Web framework
- **Uvicorn**: ASGI server
- **Pydantic**: Data validation
- **Python JSON**: Data handling

### Setup Commands
```bash
# Create virtual environment
python -m venv env

# Activate virtual environment
env\Scripts\activate

# Install dependencies
pip install fastapi uvicorn pydantic

# Run server
uvicorn main:app --reload
```

---

## Code Examples

### 1. Basic FastAPI Setup
```python
from fastapi import FastAPI
import json

app = FastAPI()

# Helper function to load data from JSON file
def load_data():
    with open('patients.json', 'r') as f:
        data = json.load(f)
    return data

# Basic endpoints
@app.get("/")
def read_root():
    return {"message": "Patient Management System API"}

@app.get("/about")
def about():
    return {"message": "A fully functional API to manage your patient records"}
```

### 2. Patient Data Structure (patients.json)
```json
[
  {
    "id": "P001",
    "name": "Aranya Sharma",
    "city": "Gujarat",
    "age": 32,
    "gender": "female",
    "height": 165,
    "weight": 65,
    "bmi": 23.87,
    "verdict": "Obese"
  },
  {
    "id": "P002",
    "name": "Ravi Mehta",
    "city": "Mumbai",
    "age": 35,
    "gender": "male",
    "height": 175,
    "weight": 85,
    "bmi": 27.76,
    "verdict": "Overweight"
  }
]
```

### 3. View All Patients Endpoint
```python
@app.get("/view")
def view_patients():
    """
    Retrieve all patient records
    HTTP Method: GET
    Purpose: Display all patients in the system
    """
    data = load_data()
    return data
```

---

## API Endpoints

### Planned Endpoints

| Endpoint | HTTP Method | Purpose | Status |
|----------|-------------|---------|--------|
| `/view` | GET | View all patients | ✅ Implemented |
| `/view/{patient_id}` | GET | View specific patient | 🔄 Next video |
| `/create` | POST | Add new patient | 🔄 Future |
| `/update/{patient_id}` | PUT | Update patient info | 🔄 Future |
| `/delete/{patient_id}` | DELETE | Remove patient | 🔄 Future |

### Current Implementation: View All Patients

**URL**: `http://localhost:8000/view`
**Method**: GET
**Response**: JSON array of all patient records

**Example Response**:
```json
[
  {
    "id": "P001",
    "name": "Aranya Sharma",
    "city": "Gujarat",
    "age": 32,
    "gender": "female",
    "height": 165,
    "weight": 65,
    "bmi": 23.87,
    "verdict": "Obese"
  }
]
```

![image](https://github.com/user-attachments/assets/c698e94f-49c9-4bf1-8c38-5e6eebe73d9a)


## Testing the API

### 1. Browser Testing
- Navigate to: `http://localhost:8000/view`
- View JSON response directly in browser

### 2. Interactive Documentation
- FastAPI automatically generates documentation
- Access at: `http://localhost:8000/docs`
- Try endpoints directly from the documentation

### 3. Network Tab Verification
- Open browser developer tools
- Go to Network tab
- Make request to `/view` endpoint
- Verify HTTP method is GET

---

## Key Concepts Learned

### 1. HTTP Methods
- Understanding the four fundamental HTTP methods
- Mapping CRUD operations to HTTP methods
- Real-world examples of each method

### 2. Software Architecture
- Difference between static and dynamic software
- Client-server communication patterns
- JSON as a data exchange format

### 3. FastAPI Features
- Automatic API documentation
- Easy endpoint creation
- Built-in validation and serialization

### 4. Development Workflow
- Virtual environment setup
- Dependency management
- Local server development

---

## Next Steps

### Upcoming Features
1. **Specific Patient Retrieval**: Get individual patient by ID
2. **Data Filtering**: Sort patients by parameters
3. **Patient Creation**: Add new patient records
4. **Patient Updates**: Modify existing records
5. **Patient Deletion**: Remove patient records

### Advanced Topics
- Database integration (PostgreSQL/MongoDB)
- Authentication and authorization
- Input validation with Pydantic models
- Error handling and status codes
- API testing with pytest

---

## Best Practices

### 1. Code Organization
- Separate data access logic into helper functions
- Use meaningful function and variable names
- Implement proper error handling

### 2. API Design
- Follow RESTful conventions
- Use appropriate HTTP status codes
- Provide clear API documentation

### 3. Data Management
- Validate input data
- Handle edge cases
- Implement proper data persistence

---

## Conclusion

This tutorial introduces the fundamentals of building a Patient Management System API using FastAPI. We covered:

- **Problem identification** and solution design
- **Software classification** and interaction patterns
- **HTTP methods** and CRUD operations
- **Basic FastAPI implementation**
- **JSON data handling**

The project provides a practical foundation for understanding web API development and will be expanded with additional features in subsequent tutorials.
