# two-tier-flask-app

## Overview

This project demonstrates a simple two-tier application architecture using Flask (Python) as the backend and MySQL as the database. The app allows users to submit messages that are stored in the database and displayed on the frontend. The setup is containerized using Docker and Docker Compose, and Kubernetes deployment manifests are also provided for orchestration in a cluster environment.

---

## Prerequisites

- Docker
- Docker Compose
- Git (for cloning the repository)
- (Optional) Kubernetes cluster for k8s deployment

---

## Getting Started with Docker Compose

1. **Clone the repository:**
   ```bash
   git clone https://github.com/saifeezibrahim/two-tier-flask-app.git
   cd two-tier-flask-app
   ```

2. **Create a `.env` file** in the project root to store your MySQL environment variables:
   ```bash
   touch .env
   ```
   Add your MySQL configuration to `.env`:
   ```
   MYSQL_HOST=mysql
   MYSQL_USER=your_username
   MYSQL_PASSWORD=your_password
   MYSQL_DB=your_database
   ```
   _Replace placeholders with your actual MySQL information._

3. **Start the application using Docker Compose:**
   ```bash
   docker-compose up --build
   ```

4. **Access the application:**
   - Frontend: [http://localhost](http://localhost)
   - Backend API: [http://localhost:5000](http://localhost:5000)

5. **Initialize the database:**
   - Use a MySQL client (or phpMyAdmin) to execute:
     ```sql
     CREATE TABLE messages (
         id INT AUTO_INCREMENT PRIMARY KEY,
         message TEXT
     );
     ```

6. **Interacting with the app:**
   - Submit messages via the frontend at [http://localhost](http://localhost)
   - Direct SQL insert example: [http://localhost:5000/insert_sql](http://localhost:5000/insert_sql)

---

## Running Without Docker Compose

1. **Build the Flask app Docker image:**
   ```bash
   docker build -t flaskapp .
   ```

2. **Create a Docker network:**
   ```bash
   docker network create twotier
   ```

3. **Run the MySQL container:**
   ```bash
   docker run -d --name mysql -v mysql-data:/var/lib/mysql -v ./message.sql:/docker-entrypoint-initdb.d/message.sql --network=twotier -e MYSQL_DATABASE=mydb -e MYSQL_USER=root -e MYSQL_ROOT_PASSWORD=your_password mysql
   ```

4. **Run the Flask backend container:**
   ```bash
   docker run -d --name flaskapp --network=twotier -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=your_password -e MYSQL_DB=mydb flaskapp
   ```

---

## Kubernetes Deployment

See [`k8s/README.md`](two-tier-flask-app-main/two-tier-flask-app-master/k8s/README.md) for detailed steps to deploy this two-tier application on a Kubernetes cluster. In summary:

1. Set up your Kubernetes cluster (see [kubestarter](https://github.com/saifeezibrahim/kubestarter-main/Kubeadm_Installation_Scripts_and_Documentation/README.md))
2. Apply manifests in the `k8s` directory:
   ```
   kubectl apply -f twotier-deployment.yml
   kubectl apply -f twotier-deployment-svc.yml
   kubectl apply -f mysql-deployment.yml
   kubectl apply -f mysql-deployment-svc.yml
   kubectl apply -f persistent-volume.yml
   kubectl apply -f persistent-volume-claim.yml
   ```

---

## Cleaning Up

To stop and remove Docker containers:

- In the terminal running Docker Compose, press <kbd>Ctrl+C</kbd>
- Or run:
  ```bash
  docker-compose down
  ```

---

## Notes

- Replace all placeholder values (e.g., `your_username`, `your_password`, `your_database`) with your actual credentials.
- This setup is for development and demonstration purposes. For production, follow best practices for security and scalability.
- Always validate and sanitize user inputs to prevent SQL injection and other vulnerabilities.
- For troubleshooting, check Docker/Kubernetes logs and error messages.

---
## 🤝 Contributing

Contributions, suggestions, and corrections are welcome!  
Feel free to fork and submit a pull request!

## 📄 License

MIT  Free for personal and educational use.

## 🙋‍♂️ Maintained by Saifeez Ibrahim
If you found this helpful, star ⭐ the repo or share it to help more people learn DevOps!

Need help or want to connect? [Reach out on GitHub!](https://github.com/saifeezibrahim)

Happy learning!



