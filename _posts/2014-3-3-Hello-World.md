title: Docker Three Tier Architecture 
---

# Three-Tier Application Deployment using Dockerfile


## Prerequisites

Before you begin, ensure that you have the following installed:

- [Docker](https://www.docker.com/get-started)
  
## Project Structure

- *backend*: Node.js application serving as the backend.
- *frontend*: React.js application for the frontend.
- *mysql*: Dockerfile and configurations for the MySQL database.

## Dockerfile(Database)
![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/database%20dockerfile.png)


## Dockerfile(Backend)
![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/backend%20dockerfile.png)
## Dockerfile(Frontend)
![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/frontend%20dockerfile.png)
## Deployment Steps
0. *Create Network*
   - Navigate to the project directory
   - bash
     docker network create my-network
     
     ![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/network%20create%20.png)
1. *MySQL Database:*

   - Navigate to the mysql directory.
   - Build the MySQL Docker image:
     bash
     docker build -t mysql-image .
     
     
     ![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/database%20image%20build1.png)
     ![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/database%20image%20build2.png)

     
   - Run the MySQL container:
     bash
     docker run --name mysql-container --network=three-tier-network -p 3306:3306 -v mysql-data:/var/lib/mysql -d mysql-image
     
     ![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/database%20container%20creation.png)
   - Access the MySQL container:
     bash
     docker exec -it mysql-container /bin/bash
     
   - Inside the container, create tables for the database:
     sql
     ![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/databse%20table%20creation.png)
     USE school;
     CREATE TABLE student (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(40), roll_number INT, class VARCHAR(16));
     CREATE TABLE teacher (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(40), subject VARCHAR(40), class VARCHAR(16));
     
2. *Backend Application:*

   - Navigate to the backend directory.
   - Build the backend Docker image:
     bash
     docker build -t backend .
     
     ![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/backend%20image%20build.png)
     ![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/backend%20image%20build2.png)

   - Run the backend container:
     bash
     docker run -d -p 3500:3500 --name backend-container --network=three-tier-network backend
     
     ![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/backend%20container%20created.png)
3. *Frontend Application:*

   - Navigate to the frontend directory.
   - Build the frontend Docker image:
     bash
     docker build -t frontend .
     
     ![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/frontend%20image%20build1.png)
     ![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/frontend%20image%20build2.png)

   - Run the frontend container:
     bash
     docker run -d --name frontend-container --network=three-tier-network -p 80:80 frontend
     
4. *Access the Application:*

   Open your favorite browser and visit [http://localhost:80](http://localhost:80). Enjoy exploring the MERN stack application!
   ![Alt Text](https://raw.githubusercontent.com/Being-Reprobate/being-reprobate.github.io/blob/master/images/webpage%20running.png)

    
## Data Persistence

Data persistence is ensured by using Docker volumes. If the MySQL container is deleted, data remains available and is automatically added to a new Docker container by providing the same Docker volume.

Feel free to explore and modify the Dockerfiles to enhance your understanding of containerization and deployment! Happy coding! 🚀
