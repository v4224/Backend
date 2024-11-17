# Backend

### Installation

1. Clone the repo
   ```sh
   git clone https://github.com/v4224/Backend.git
   ```

2. Config:
   * Configure your database connection details in an `app.config` file.

3. Run the project manually:
   * Install NetCore 6
   * Restore all the dependencies
     ```sh
     dotnet restore
     ```
    * build and run project
     ```sh
     dotnet run
     ```
Open [http://192.168.189.99:5214/swagger/index.html](http://192.168.189.99:5214/swagger/index.html) to view it in your browser.

4. Run the project by using docker manually:
  * Build docker image
     ```sh
     docker build -t onlineshop-backend .
     ```
  * Run container
     ```sh
     docker run --name onlineshop-backend -dp 5214:5214 onlineshop-backend
     ```

5. Run the project by using docker with Jenkins CI/CD pipeline:
  * Set jenkins agent as lab-server
  * Run pipeline in Jenkinsfile on Jenkins server
---
