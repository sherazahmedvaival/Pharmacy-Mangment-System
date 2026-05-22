# Pharmacy Manegement System

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 8.3.6.

Updates and bug fixes are done daily :100:.

Star :star:  the repo to help the developers :innocent:


## 🦄 Product Features and Screen Shots



<table>
  <tr>
    <td>Login</td>
     <td>SignUp</td>
     
  </tr>
  <tr>
    <td><img src="https://i.ibb.co/0BCPCQB/Screenshot-2020-08-30-at-00-02-41.png" width="600"></td>
    <td><img src="https://i.ibb.co/fv4F5jR/Screenshot-2020-08-30-at-00-02-54.png" width="600"></td>
  </tr>
 </table>
 
<img src="https://i.ibb.co/W0FKBk1/Screenshot-2020-08-30-at-00-03-31.png" > 

<table>
  <tr>
    <td>Doctor Oders</td>
     <td>Verified Doctor Oders</td>
     
  </tr>
  <tr>
    <td><img src="https://i.ibb.co/Dk4GP77/Screenshot-2020-08-30-at-00-05-09.png" width="600"></td>
    <td><img src="https://i.ibb.co/HNB2B9D/Screenshot-2020-08-30-at-00-05-20.png" width="600"></td>
  </tr>
 </table>
 
 <table>
  <tr>
    <td>Point Of Sales</td> 
  </tr>
  <tr>
    <td><img src="https://i.ibb.co/1vCrYKk/Screenshot-2020-08-30-at-00-06-11.png"></td>
  </tr>
 </table>
 
 <table>
  <tr>
    <td>Checking out drugs from Point Of Sales</td> 
  </tr>
  <tr>
    <td><img src="https://i.ibb.co/wS7T14K/Screenshot-2020-08-30-at-00-06-49.png"></td>
  </tr>
 </table>
 
<table>
  <tr>
    <td>Supplier Table </td>
     <td>Supplier Form</td>
     
  </tr>
  <tr>
    <td><img src="https://i.ibb.co/0jCfM54/Screenshot-2020-08-30-at-00-07-19.png" width="600"></td>
    <td><img src="https://i.ibb.co/Wy2j4HV/Screenshot-2020-08-30-at-00-07-07.png" width="600"></td>
  </tr>
 </table>
 
 <table>
  <tr>
    <td>Sales Charts generated</td> 
  </tr>
  <tr>
    <td><img src="https://i.ibb.co/zNxh1pD/Screenshot-2020-08-30-at-00-07-32.png"></td>
  </tr>
 </table>
 
 <table>
  <tr>
    <td>Sends Email requests to suppliers when drugs expire </td>
     <td>Expired & about to expire table</td>
     
  </tr>
  <tr>
    <td><img src="https://i.ibb.co/s6ZB4ny/Screenshot-2020-08-30-at-00-08-22.png" width="600"></td>
    <td><img src="https://i.ibb.co/F77KhWJ/Screenshot-2020-08-30-at-00-08-01.png" width="600"></td>
  </tr>
 </table>
 
 
 <table>
  <tr>
    <td>Preferences or Settings </td>
     <td>Out of Stock & About to get out of stock</td>
     
  </tr>
  <tr>
    <td><img src="https://i.ibb.co/4YmKk4Y/Screenshot-2020-08-30-at-00-08-49.png" width="600"></td>
    <td><img src="https://i.ibb.co/0Z3qbrh/Screenshot-2020-08-30-at-00-08-32.png" width="600"></td>
  </tr>
 </table>


## 🚀 Build Instructions / How to start the project 

1) Downloard/clone the Contributor branch of the repository
2) Open terminal/command prompt 
3) cd (change directory) in to the project folder
4) Run `npm install` in your terminal
5) Run `ng serve` to run the Angular frontend
6) Run `npm run start:server` to run the backend Node server
7) Open your browser and navigate to `http://localhost:4200/`


## 🐳 Run with Docker (recommended)

This fork ships a Docker Compose setup that runs the Angular frontend,
the Node/Express backend, and a MongoDB database in three containers.
No local Node, npm, or MongoDB install required — just Docker.

### Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (or
  Docker Engine + Compose v2)
- Host ports `4200`, `3001`, and `27017` free

### Quick start

```bash
# 1. Clone the repository
git clone https://github.com/sherazahmedvaival/Pharmacy-Mangment-System.git
cd Pharmacy-Mangment-System

# 2. Copy the env template (defaults work out-of-the-box)
cp .env.example .env

# 3. Build and start everything (first build ~3–5 min)
docker compose up --build -d

# 4. Watch the logs until you see "Compiled successfully."
docker compose logs -f frontend
```

When the Angular dev server reports `Compiled successfully.`, open:

| URL                                            | What it is                       |
| ---------------------------------------------- | -------------------------------- |
| http://localhost:4200                          | Pharmacy app (Angular frontend)  |
| http://localhost:3001/api/...                  | Backend REST API                 |
| mongodb://localhost:27017                      | MongoDB                          |

### Seed the first admin user

The bundled MongoDB starts empty — the project has **no factory-default
credentials**. Create the first user (a `pharmacist`, which is the
highest-privilege role) with one curl call:

```bash
curl -X POST http://localhost:3001/api/user/signup \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Admin",
    "contact": "0000000000",
    "nic": "000000000V",
    "email": "admin@local.test",
    "password": "admin123",
    "role": "pharmacist"
  }'
```

Then log in at http://localhost:4200 with:

| Field    | Value              |
| -------- | ------------------ |
| Email    | `admin@local.test` |
| Password | `admin123`         |

> Change the password (and rotate the JWT secret in
> `backend/routes/user.js`) before exposing this anywhere beyond your
> laptop.

Available roles (per `src/app/sidemenu/menuitem/menuitem.component.ts`):
`pharmacist`, `cashier`, `assistantPharmacist`.

### Common Docker commands

| Action                                              | Command                                |
| --------------------------------------------------- | -------------------------------------- |
| Start (rebuild if changed)                          | `docker compose up --build -d`         |
| Stop (preserves data)                               | `docker compose stop`                  |
| Start again                                         | `docker compose start`                 |
| Restart one service                                 | `docker compose restart backend`       |
| Remove containers (keeps DB volume)                 | `docker compose down`                  |
| Remove containers **and** wipe the database         | `docker compose down -v`               |
| Tail logs                                           | `docker compose logs -f`               |
| Shell into a container                              | `docker compose exec backend sh`       |

Full guide with troubleshooting: see [DOCKER-DEPLOY.md](DOCKER-DEPLOY.md).


## 🚨 Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.
Run `npm run start:serve` for a backend server. Navigate to `http://localhost:3000/`. 

## 🚨 Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## 🚨 Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## 🚨 Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## 🚨 Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

