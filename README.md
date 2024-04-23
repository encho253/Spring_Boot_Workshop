# Spring Boot Workshop - Technical University Plovdiv, 23.04.2024

## Assignment - Registration form for Student System web application
Create a SPA (Single Page Application) website with the following registration form:

![LoginForm](https://github.com/encho253/Spring_Boot_Workshop/assets/13778374/68e10cff-485b-4205-a666-1ef56ba55433)

## Requirements
### Backend technologies:
- Java 17
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
- Add Student Repository class (locating it in a package .repository) which will be used from Spring Data to translate Java to SQL
- Add Student Service interface and Student Service class to implement it (locating it in a package .service).
- Create a new database schema in MySql called 'studentsystem'

### Frontend
- Create a new ReactJs application
- Add a well-looking registration form
- Call the POST API from the backend for creating a new student

## 01. Setting up the development environment
- Install Git https://git-scm.com/
- Install TortoiseGit https://tortoisegit.org/download/
- Install JAVA JDK 17 https://www.oracle.com/java/technologies/downloads/#jdk17-windows
- Install IDE Eclipse/IntelliJ
- Install Node js https://nodejs.org/en

## 02. Generate a new Spring Boot application
- Use Spring Initializr https://start.spring.io/
- Add these dependencies: **Spring Boot Dev Tools, Spring Web, Spring Data JPA, MySQL Driver**
![SpringBoot_Initializr](https://github.com/encho253/Spring_Boot_Workshop/assets/13778374/39b3ee90-2e08-433b-bea8-1dc09321dc29)

## 03. Create the project structure of the application
- Add a new package controller with a new class called **StudentController.java**
- Add a new package model with a new class called **Student.java**
- Add a new package repository with a new interface called **StudentRepository.java**
- Add a new package service with a new class **StudentServiceImpl.java** and implements an interface called **StudentService.java**
![ProjectStructure](https://github.com/encho253/Spring_Boot_Workshop/assets/13778374/f95f70c4-d563-41ff-b291-3fb24d787ffb)

## 04. Create a new POST API in StudentController.java
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


