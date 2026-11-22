# Developing with Docker: Node.js + MongoDB User Profile App

A hands-on project demonstrating how a Node.js/Express application connects to MongoDB, with Mongo Express used to inspect and manage the database.

The application displays a user profile, allows the user to edit it, saves the changes to MongoDB, and loads the saved profile when the page is refreshed.

## Project goals

- Run MongoDB and Mongo Express as Docker containers.
- Create a dedicated Docker network so the database tools can communicate.
- Connect a Node.js/Express application to MongoDB.
- Store and retrieve profile information from the `user-account` database and `users` collection.
- Verify saved documents through Mongo Express.

## Technology stack

- **Docker** – container runtime and networking
- **MongoDB** – document database
- **Mongo Express** – browser-based MongoDB administration interface
- **Node.js** – JavaScript runtime
- **Express.js** – web application backend
- **HTML, CSS and JavaScript** – user interface

## Architecture

```text
Browser
  |
  | http://localhost:3000
  v
Node.js + Express application
  |
  | MongoDB connection
  v
MongoDB container
  |
  | inspected through
  v
Mongo Express (http://localhost:8081)
```

The application uses the MongoDB database named `user-account` and the `users` collection.

## Screenshots

### Docker images

![Docker images](docker%20images.png)

### Docker network

![Docker network](docker%20network.png)

### Pulling the MongoDB image

![Pull MongoDB image](docker%20pull%20mongo.png)

### Running containers

![Docker containers](docker%20run%20and%20docker%20ps.png)

### Mongo Express

![Mongo Express](mongo%20express.png)

### Application in the browser

![Application running on localhost:3000](localhost%203000.png)

### Database and collection

![Mongo Express user-account database and users collection](mongo%20express%20after%20adding%20user%20account.png)

### Saved user document

![User document saved in MongoDB](user%20added.png)

![Updated user document saved in MongoDB](user%20added%202.png)

## Run the project locally

### 1. Clone the repository

```bash
git clone https://github.com/Acnweze/developing-with-docker.git
cd developing-with-docker
```

### 2. Create a Docker network

```bash
docker network create mongo-network
```

If the network already exists, Docker will report that; you can continue using the existing network.

### 3. Start MongoDB

```bash
docker run -d \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  --name mongodb \
  --network mongo-network \
  mongo
```

### 4. Start Mongo Express

```bash
docker run -d \
  -p 8081:8081 \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
  -e ME_CONFIG_MONGODB_SERVER=mongodb \
  --network mongo-network \
  --name mongo-express \
  mongo-express
```

These credentials are example credentials for a local learning environment only. Do not use them for a publicly accessible deployment.

### 5. Create the database and collection

Open [http://localhost:8081](http://localhost:8081) in your browser.

In Mongo Express, create:

- Database: `user-account`
- Collection: `users`

### 6. Install Node.js dependencies and start the application

Open another terminal, navigate to the application's `app` directory, and run:

```bash
cd app
npm install
npm start
```

Open [http://localhost:3000](http://localhost:3000) to use the profile application.

> The Node.js application source is maintained in the `app/` directory. Make sure its MongoDB connection settings match the MongoDB container and your local environment.

## Verify the saved data

1. Open the application at `http://localhost:3000`.
2. Select **Edit Profile**, update the name, email and interests, then select **Save Profile**.
3. Open Mongo Express at `http://localhost:8081`.
4. Browse to `user-account` → `users` and confirm that the document was saved.
5. Refresh the application page and confirm that the saved profile is loaded again.

## Security notes

- The credentials in the Docker commands above are for local demonstration only.
- Keep real credentials and connection strings out of source code and Git history.
- Use environment variables or a local, untracked `.env` file for secrets.
- Add `.env` to `.gitignore` before pushing project files.
- Do not expose MongoDB or Mongo Express ports publicly without appropriate access controls.

## What I practised

- Pulling and running Docker images.
- Creating and inspecting a Docker network.
- Running and checking multiple containers.
- Connecting a Node.js application to MongoDB.
- Saving and retrieving documents.
- Using Mongo Express to verify database records.

---

**Project type:** Hands-on Docker and application integration lab.
