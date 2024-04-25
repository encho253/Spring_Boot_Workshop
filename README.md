# Spring Boot Workshop - Technical University Plovdiv, 23.04.2024

## Assignment - Registration form for Student System web application
Create a SPA (Single Page Application) website with the following registration form:

![LoginForm](https://github.com/encho253/Spring_Boot_Workshop/assets/13778374/68e10cff-485b-4205-a666-1ef56ba55433)

## Requirements
### Backend technologies:
- Java 17 or higher required
- Spring Boot
- MySQL Database
- Hibernate

### Frontend technologies:
- Javascript
- CSS
- HTML
- ReactJS

### Backend
- Create a new Spring Boot application with Spring Data and connector for MySql database
- Add Rest Controller class for catching HTTP requests, locating it in a package .controller
- Add Entity Model class Student (locating it in a package .model)
- Student entity will be used for creating a new MySQL table, called Student in the Database
- Add Student Repository class (locating it in a package .repository) which will be used from Spring Data to translate Java classes to SQL tables
- Add Student Service interface and Student Service class to implement it (locating it in a package .service).
- Create a new database schema in MySql called 'studentsystem'

### Frontend
- Create a new `ReactJS` application
- Add a well-looking registration form
- Call the POST API from the backend for creating a new student

## 01. Setting up the development environment
- Install Git https://git-scm.com/
- Install TortoiseGit https://tortoisegit.org/download/
- Install JAVA JDK 17 https://www.oracle.com/java/technologies/downloads/#jdk17-windows
- Install IDE Eclipse/IntelliJ
- Install Node js https://nodejs.org/en
- Install Visual Studio Code
- Install MySql Server https://dev.mysql.com/downloads/mysql/
- Install MySql Workbench https://dev.mysql.com/downloads/workbench/
- Install TCPView https://learn.microsoft.com/en-us/sysinternals/downloads/tcpview

## 02. Generate a new Spring Boot application called StudentSystem
- Use Spring Initializr https://start.spring.io/
- Add these dependencies: **Spring Boot Dev Tools, Spring Web, Spring Data JPA, MySQL Driver**
![SpringBoot_Initializr](https://github.com/encho253/Spring_Boot_Workshop/assets/13778374/39b3ee90-2e08-433b-bea8-1dc09321dc29)

## 03. Create the project structure of the application
- Add a new package controller with a new class called **StudentController.java**
- Add a new package model with a new class called **Student.java**
- Add a new package repository with a new interface called **StudentRepository.java**
- Add a new package service with a new class **StudentServiceImpl.java** and implements an interface called **StudentService.java**
![ProjectStructure](https://github.com/encho253/Spring_Boot_Workshop/assets/13778374/f95f70c4-d563-41ff-b291-3fb24d787ffb)

## 04. Create a new POST API in `StudentController.java`
```
@RestController
@RequestMapping("/student")
@CrossOrigin
public class StudentController {
	
	@Autowired
	private StudentService studentService;
	
	@PostMapping("/add")
	public String add(@RequestBody Student student) {
		studentService.saveStudent(student);
		return "New student is added";
	}
}
```
### 05. Transform `Student.java` class into an entity model class
```
@Entity
public class Student {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private int id;
	
	private String firstName;
	
	private String lastName;
	
	private String email;
	
	public Student() {		
	}
	
	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getLastName() {
		return lastName;
	}

	public void setLastName(String lastName) {
		this.lastName = lastName;
	}

	public String getFirstName() {
		return firstName;
	}

	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}
}
```

### 06. Extend and annotate the interface `StudentRepository.java`
```
@Repository
public interface StudentRepository extends JpaRepository<Student, Integer> {
}
```

### 07. Add a new method for creating students in the interface `StudentService.java` and `StudentServiceImpl.java`
```
public interface StudentService {
	public Student saveStudent(Student student);
}
```

### 08. Implement the interface `StudentService.java` by the class `StudentServiceImpl.java`
```
@Service
public class StudentServiceImpl implements StudentService{
    
	@Autowired
	private StudentRepository studentRepository;
	
	@Override
	public Student saveStudent(Student student) {
		return studentRepository.save(student);
	}

}
```

### 09. Create a new database schema called 'studentsystem' in MySQL Workbench
![image](https://github.com/encho253/Spring_Boot_Workshop/assets/13778374/9a900c18-c861-4740-9187-5e3b2895c149)

### 10. Create a database connection configuration in the Spring Boot Application. Add the following configuration to the application.properties file

```
#configuration
spring.application.name=studentsystem

spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect

spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://localhost:3306/studentsystem

spring.datasource.username=appraisal_admin
spring.datasource.password=appraisal_admin

spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

### 11. Run the application and use Postman to send an HTTP POST request to the controller http://localhost:8080/student/add
with this JSON body as a request body
```
{
    "firstName": "Encho",
    "lastName": "Enevski",
    "email": "encho.enevski@gmail.com"
}
```

### 12. Create an empty React project in Visual Studio Code
- Create somewhere a new folder
- Open Visual Studio Code, Navigate to File -> Open Folder -> Select the folder that you have recently created
- From the menu **Terminal** open a new Terminal and type the command:
             ```npx create-react-app studentfrontend```
- The generated project contains two files with the important logic, everything else is just a boilerplate
![React_Project_Structure](https://github.com/encho253/Spring_Boot_Workshop/assets/13778374/4889e133-d6b0-496f-a41a-9a018b16333d)

### 13. Replace the code in App.js with the following Registration form code
```
import "./styles.css";
import React, { useState } from "react";

export default function App() {
  const [values, setValues] = useState({
    firstName: "",
    lastName: "",
    email: "",
  });

  const handleInputChange = (event) => {
    event.preventDefault();

    const { name, value } = event.target;
    setValues((values) => ({
      ...values,
      [name]: value,
    }));
  };

  const [submitted, setSubmitted] = useState(false);
  const [valid, setValid] = useState(false);

  const handleSubmit = (e) => {
    e.preventDefault();
    if (values.firstName && values.lastName && values.email) {
      setValid(true);
    }
     setSubmitted(true);
  };

  return (
    <div className="form-container">
      <form className="register-form" onSubmit={handleSubmit}>
        {submitted && valid && (
          <div className="success-message">
            <h3>
              {" "}
              Welcome {values.firstName} {values.lastName}{" "}
            </h3>
            <div> Your registration was successful! </div>
          </div>
        )}
        {!valid && (
          <input
            class="form-field"
            type="text"
            placeholder="First Name"
            name="firstName"
            value={values.firstName}
            onChange={handleInputChange}
          />
        )}

        {submitted && !values.firstName && (
          <span id="first-name-error">Please enter a first name</span>
        )}

        {!valid && (
          <input
            class="form-field"
            type="text"
            placeholder="Last Name"
            name="lastName"
            value={values.lastName}
            onChange={handleInputChange}
          />
        )}

        {submitted && !values.lastName && (
          <span id="last-name-error">Please enter a last name</span>
        )}

        {!valid && (
          <input
            class="form-field"
            type="email"
            placeholder="Email"
            name="email"
            value={values.email}
            onChange={handleInputChange}
          />
        )}

        {submitted && !values.email && (
          <span id="email-error">Please enter an email address</span>
        )}
        {!valid && (
          <button class="form-field" type="submit">
            Register
          </button>
        )}
      </form>
    </div>
  );
}

```
### 14. Add the CSS for our registration form - create a new file called styles.css
```
body {
  background: #76b852;
  display: flex;
  min-height: 100vh;
  justify-content: center;
  align-items: center;
}

.form-container {
  width: 360px;
  background-color: white;
  margin: auto;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
  padding: 10px;
}

.register-form {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  padding: 10px;
}

.success-message {
  font-family: "Roboto", sans-serif;
  background-color: #3f89f8;
  padding: 15px;
  color: white;
  text-align: center;
}

.form-field {
  margin: 10px 0 10px 0;
  padding: 15px;
  font-size: 16px;
  border: 0;
  font-family: "Roboto", sans-serif;
}

span {
  font-family: "Roboto", sans-serif;
  font-size: 14px;
  color: red;
  margin-bottom: 15px;
}

input {
  background: #f2f2f2;
}

.error {
  border-style: solid;
  border: 2px solid #ffa4a4;
}

button {
  background: #4caf50;
  color: white;
  cursor: pointer;
}

button:disabled {
  cursor: default;
}

```

### 15. Add `fetch` method to send HTTP requests to the backend, replace the `const handleSubmit = (e) => {...` with this code:
```
const handleSubmit = (e) => {
    e.preventDefault();
    if (values.firstName && values.lastName && values.email) {
      setValid(true);
    }
    fetch("http://localhost:8080/student/add", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(values),
    }).then(() => {
      setSubmitted(true);
      console.log("New Student Added");
    });
  };
```

### 16. Run the React application with the command: 
		```npm start```
- your frontend application is started on http://localhost:3000/
- your backend application is started on http://localhost:8080/



