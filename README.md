# WordPress + MySQL Docker Setup

This guide helps you quickly set up a new WordPress project with MySQL using Docker Compose.

---

## Prerequisites

- Docker installed and running
- Basic knowledge of terminal commands
- Docker Compose installed (comes bundled with recent Docker versions)

---

## Step-by-Step Setup

### 1. Create a New Project Directory

```bash
mkdir ~/dev/personal-projects/my-new-wordpress
cd ~/dev/personal-projects/my-new-wordpress
```

### 2. Create docker-compose.yml
Create a file named docker-compose.yml with the following content:

```bash
services:
  wordpress:
    image: wordpress:latest
    ports:
      - "8000:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppassword
      WORDPRESS_DB_NAME: wpdb
    volumes:
      - ./wp-content:/var/www/html/wp-content
    restart: always
    depends_on:
      - db
    command: sh -c "sleep 20 && docker-entrypoint.sh apache2-foreground"

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword    # Change this password
      MYSQL_DATABASE: wpdb                  # Database name for this project
      MYSQL_USER: wpuser                    # Database user
      MYSQL_PASSWORD: wppassword            # Database user password
    volumes:
      - db_data:/var/lib/mysql
    restart: always

volumes:
  db_data:
```
Tip: Change the passwords and database names to something unique per project.

### 3. Start the Containers
Run:
```bash
docker compose up -d
```
This command downloads the necessary images and starts your WordPress and MySQL containers.

### 4. Verify MySQL Initialization
Check the logs to ensure the database and user are created correctly:
```bash
docker compose logs db
```
Look for lines confirming the creation of the database and users.


### 5. Access WordPress
Open your browser and go to:
```bash
http://localhost:8000
```
Complete the WordPress installation wizard using these database details:

    Database Name: wpdb

    Username: wpuser

    Password: wppassword

    Database Host: db:3306

### 6. Stop Containers
To stop the running containers, use:
```bash
docker compose down
```
### 7. Remove Containers and Volumes (Clean Slate)
To remove containers and volumes (data will be lost):
```bash
docker compose down -v
```

## Notes

    If you run multiple projects, change the ports to avoid conflicts (e.g., "8001:80", "8002:80").

    Use .env files for managing sensitive data in production.

    Volumes persist your database and WordPress files between restarts.

    For more automation, consider scripting or using environment variable substitution in docker-compose.yml.

## License

This setup guide is free to use and modify.