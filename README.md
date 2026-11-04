# 🚆 Train Ticket Booking System (TrainBook)

## 📌 Project Description
TrainBook is a Java-based Train Ticket Booking System developed using Maven. It allows users to book train tickets, manage passenger details, and check booking information.

This project is designed to simplify the railway ticket reservation process.

## 🎯 Features
- User Registration and Login
- Search Available Trains
- Check Train Details
- Book Train Tickets
- Enter Passenger Details
- View Booking Details
- Manage Ticket Reservations

## 🛠️ Technologies Used
- **Programming Language:** Java
- **Build Tool:** Apache Maven
- **Frontend:** HTML, CSS, JavaScript
- **Backend:** Java / JSP / Servlets (as applicable)
- **Server:** Apache Tomcat
- **Version Control:** Git & GitHub

## 📂 Project Structure

```text
TrainBook/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   ├── resources/
│   │   └── webapp/
│   │
│   └── test/
│
├── pom.xml
├── README.md
└── target/
```

## ⚙️ Prerequisites
Install the following software:

1. Java JDK 8 or higher
2. Apache Maven
3. Apache Tomcat
4. Git
5. IDE (Eclipse / IntelliJ IDEA)

## 🚀 How to Run the Project

### Step 1: Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/TrainBook.git
```

### Step 2: Navigate to Project Directory

```bash
cd TrainBook
```

### Step 3: Build the Project

```bash
mvn clean package
```

### Step 4: Deploy to Tomcat

Copy the generated WAR file from the target folder into the Tomcat webapps directory.

### Step 5: Start Tomcat

```bash
./startup.sh
```

For Windows:

```bash
startup.bat
```

### Step 6: Open the Application

Open your browser and visit:

```text
http://localhost:8080/TrainBook
```

## 🔨 Maven Commands

| Command | Description |
|---|---|
| `mvn clean` | Removes previous build files |
| `mvn compile` | Compiles Java source code |
| `mvn test` | Executes test cases |
| `mvn package` | Generates WAR/JAR file |
| `mvn clean package` | Cleans and builds the project |

## 🔄 CI/CD Integration
This project can be integrated with a CI/CD pipeline using:

- GitHub – Source Code Management
- GitHub Actions – Continuous Integration
- Maven – Build Automation
- Apache Tomcat – Application Deployment

## 👨‍💻 Developer
**Project Name:** TrainBook – Train Ticket Booking System

**Developed By:** Your Name

## 📄 License
This project is created for learning and educational purposes.
