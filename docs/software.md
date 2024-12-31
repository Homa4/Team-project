# Реалізація інформаційного та програмного забезпечення

В рамках проекту розробляється: 

```sql
-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema databaseLab5
-- -----------------------------------------------------
DROP SCHEMA IF EXISTS `databaseLab5` ;

-- -----------------------------------------------------
-- Schema databaseLab5
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `databaseLab5` DEFAULT CHARACTER SET utf8 ;
USE `databaseLab5` ;

-- -----------------------------------------------------
-- Table `databaseLab5`.`User`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`User` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`User` (
  `id` INT NOT NULL,
  `name` VARCHAR(45) NULL,
  `surname` VARCHAR(45) NULL,
  `email` VARCHAR(45) NULL,
  `password` CHAR(8) NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`Admin`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`Admin` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`Admin` (
  `id` INT NOT NULL,
  `name` VARCHAR(45) NULL,
  `surname` VARCHAR(45) NULL,
  `email` VARCHAR(45) NULL,
  `password` CHAR(8) NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`Permission`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`Permission` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`Permission` (
  `name` VARCHAR(45) NULL,
  `description` VARCHAR(255) NULL,
  `id` INT NOT NULL,
  `User_id` INT NOT NULL,
  `Admin_id` INT NOT NULL,
  PRIMARY KEY (`id`, `User_id`, `Admin_id`),
  INDEX `fk_Permission_User_idx` (`User_id` ASC) VISIBLE,
  INDEX `fk_Permission_Admin1_idx` (`Admin_id` ASC) VISIBLE,
  CONSTRAINT `fk_Permission_User`
    FOREIGN KEY (`User_id`)
    REFERENCES `databaseLab5`.`User` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Permission_Admin1`
    FOREIGN KEY (`Admin_id`)
    REFERENCES `databaseLab5`.`Admin` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`UploadedMedia`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`UploadedMedia` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`UploadedMedia` (
  `id` INT NOT NULL,
  `source` INT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`UploadedMediaSource`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`UploadedMediaSource` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`UploadedMediaSource` (
  `id` INT NOT NULL,
  `url` VARCHAR(255) NULL,
  `UploadedMedia_id` INT NOT NULL,
  PRIMARY KEY (`id`, `UploadedMedia_id`),
  INDEX `fk_UploadedMediaSource_UploadedMedia1_idx` (`UploadedMedia_id` ASC) VISIBLE,
  CONSTRAINT `fk_UploadedMediaSource_UploadedMedia1`
    FOREIGN KEY (`UploadedMedia_id`)
    REFERENCES `databaseLab5`.`UploadedMedia` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`Form`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`Form` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`Form` (
  `id` INT NOT NULL,
  `name` VARCHAR(45) NULL,
  `description` VARCHAR(45) NULL,
  `content` JSON NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`FormResult`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`FormResult` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`FormResult` (
  `id` INT NOT NULL,
  `formid` INT NULL,
  `answer` JSON NULL,
  `Form_id` INT NOT NULL,
  PRIMARY KEY (`id`, `Form_id`),
  INDEX `fk_FormResult_Form1_idx` (`Form_id` ASC) VISIBLE,
  CONSTRAINT `fk_FormResult_Form1`
    FOREIGN KEY (`Form_id`)
    REFERENCES `databaseLab5`.`Form` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`Content`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`Content` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`Content` (
  `id` INT NOT NULL,
  `type` ENUM("answer") NULL,
  `Form_id` INT NOT NULL,
  PRIMARY KEY (`id`, `Form_id`),
  INDEX `fk_Content_Form1_idx` (`Form_id` ASC) VISIBLE,
  CONSTRAINT `fk_Content_Form1`
    FOREIGN KEY (`Form_id`)
    REFERENCES `databaseLab5`.`Form` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`OpenAnswer`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`OpenAnswer` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`OpenAnswer` (
  `id` INT NOT NULL,
  `text` TEXT NULL,
  `Content_id` INT NOT NULL,
  PRIMARY KEY (`id`, `Content_id`),
  INDEX `fk_OpenAnswer_Content1_idx` (`Content_id` ASC) VISIBLE,
  CONSTRAINT `fk_OpenAnswer_Content1`
    FOREIGN KEY (`Content_id`)
    REFERENCES `databaseLab5`.`Content` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`SingleChoise`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`SingleChoise` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`SingleChoise` (
  `id` INT NOT NULL,
  `variant` VARCHAR(45) NULL,
  `Content_id` INT NOT NULL,
  PRIMARY KEY (`id`, `Content_id`),
  INDEX `fk_SingleChoise_Content1_idx` (`Content_id` ASC) VISIBLE,
  CONSTRAINT `fk_SingleChoise_Content1`
    FOREIGN KEY (`Content_id`)
    REFERENCES `databaseLab5`.`Content` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `databaseLab5`.`MultiChoise`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `databaseLab5`.`MultiChoise` ;

CREATE TABLE IF NOT EXISTS `databaseLab5`.`MultiChoise` (
  `id` INT NOT NULL,
  `variants` JSON NULL,
  `Content_id` INT NOT NULL,
  PRIMARY KEY (`id`, `Content_id`),
  INDEX `fk_MultiChoise_Content1_idx` (`Content_id` ASC) VISIBLE,
  CONSTRAINT `fk_MultiChoise_Content1`
    FOREIGN KEY (`Content_id`)
    REFERENCES `databaseLab5`.`Content` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

-- -----------------------------------------------------
-- Data for table `databaseLab5`.`User`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`User` (`id`, `name`, `surname`, `email`, `password`) VALUES (1, 'David', 'Krykota', 'a@gmail.com', 'a111');
INSERT INTO `databaseLab5`.`User` (`id`, `name`, `surname`, `email`, `password`) VALUES (2, 'Tymur', 'Naboka', 'b@gmail.com', 'b222');
INSERT INTO `databaseLab5`.`User` (`id`, `name`, `surname`, `email`, `password`) VALUES (3, 'Nika', 'Maslo', 'c@gmail.com', 'c333');

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`Admin`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`Admin` (`id`, `name`, `surname`, `email`, `password`) VALUES (1, 'admin', 'adminovich', 'admin@gmail.com', 'admin111');

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`Permission`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`Permission` (`name`, `description`, `id`, `User_id`, `Admin_id`) VALUES ('edit', 'allows to edit form', 1, 1, 1);
INSERT INTO `databaseLab5`.`Permission` (`name`, `description`, `id`, `User_id`, `Admin_id`) VALUES ('answer', 'allows to chose an answer', 2, 1, 1);
INSERT INTO `databaseLab5`.`Permission` (`name`, `description`, `id`, `User_id`, `Admin_id`) VALUES ('check_results', 'allows to check results', 3, 1, 1);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`UploadedMedia`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`UploadedMedia` (`id`, `source`) VALUES (1, 123);
INSERT INTO `databaseLab5`.`UploadedMedia` (`id`, `source`) VALUES (2, 456);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`UploadedMediaSource`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`UploadedMediaSource` (`id`, `url`, `UploadedMedia_id`) VALUES (1, 'media.com', 1);
INSERT INTO `databaseLab5`.`UploadedMediaSource` (`id`, `url`, `UploadedMedia_id`) VALUES (2, 'othermedia.com', 2);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`Form`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`Form` (`id`, `name`, `description`, `content`) VALUES (1, 'wild animals of ukraine', 'choose the best choices for survival', '{\"1\": \"Deers\", \"2\": \"Bears\", \"3\": \"Birds\"}');
INSERT INTO `databaseLab5`.`Form` (`id`, `name`, `description`, `content`) VALUES (2, 'emotions', 'which emotions you experience the most', '{\"1\": \"Emotion1\",\"2\":\"Emotion2\",\"3\":\"Emotion3\"}');

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`FormResult`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`FormResult` (`id`, `formid`, `answer`, `Form_id`) VALUES (1, 1, '{\"1\":\"a\",\"2\":\"c\",\"3\":\"b\"}', 1);
INSERT INTO `databaseLab5`.`FormResult` (`id`, `formid`, `answer`, `Form_id`) VALUES (2, 2, '{\"1\":\"d\",\"2\":\"a\",\"3\":\"c\",\"4\":\"a\"}', 2);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`Content`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`Content` (`id`, `type`, `Form_id`) VALUES (1, 'answer', 1);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`OpenAnswer`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`OpenAnswer` (`id`, `text`, `Content_id`) VALUES (1, 'I love wild animals of Ukraine and wish to protect them', 1);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`SingleChoise`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`SingleChoise` (`id`, `variant`, `Content_id`) VALUES (1, 'a', 1);
INSERT INTO `databaseLab5`.`SingleChoise` (`id`, `variant`, `Content_id`) VALUES (2, 'sadness', 1);
INSERT INTO `databaseLab5`.`SingleChoise` (`id`, `variant`, `Content_id`) VALUES (3, 'Deer', 1);

COMMIT;


-- -----------------------------------------------------
-- Data for table `databaseLab5`.`MultiChoise`
-- -----------------------------------------------------
START TRANSACTION;
USE `databaseLab5`;
INSERT INTO `databaseLab5`.`MultiChoise` (`id`, `variants`, `Content_id`) VALUES (1, '{\"1\":\"a\",\"2\":\"b\",\"3\":\"c\"}', 1);
INSERT INTO `databaseLab5`.`MultiChoise` (`id`, `variants`, `Content_id`) VALUES (2, '{\"1\":\"d\",\"2\":\"b\",\"3\":\"c\"}', 1);

COMMIT;
```
## REST API для управління даними

### Підключення до бази даних MySql

#### connection.js

```javascript
import { config } from "dotenv";
import { createConnection } from "mysql2";
import chalk from "chalk";

config();

const connection = createConnection({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  database: process.env.DB_NAME,
  password: process.env.DB_PASSWORD,
});

const connectToDB = () => {
  connection.connect((error) => {
    if (error) {
      console.error("Failed to connect to the database:", error.message);
    } else {
      console.log(chalk.greenBright("Successfully connected to the database"));
    }
  });
};

export { connection, connectToDB };

```
### Налаштування сервера

#### server.js

```javascript
import { createServer } from "http";
import { connection, connectToDB } from "./connection.js";
import { handleRequest } from "./router.js";
import chalk from "chalk";

const PORT = 3000;

connectToDB();

createServer(async (req, res) => {
  try {
    handleRequest(req, res);
  } catch (error) {
    console.error("Error handling request:", error.message);
    res.writeHead(500, { "Content-Type": "application/json" });
    res.end(JSON.stringify({ error: error.message }));
  }
}).listen(PORT, () => {
  console.log(
    chalk.greenBright(`Server is running on http://localhost:${PORT}`)
  );
});

```
### Маршрутизація

#### router.js

```javascript
import {
  getAllForms,
  getFormById,
  createForm,
  updateForm,
  deleteForm,
} from "./formController.js";

export const handleRequest = (req, res) => {
  const url = new URL(req.url, `http://${req.headers.host}`);
  const { pathname } = url;

  const id = pathname.split("/")[2];

  if (req.method === "GET" && pathname === "/forms") {
    getAllForms(req, res);
  } else if (req.method === "GET" && pathname.startsWith("/forms/")) {
    getFormById(req, res, id);
  } else if (req.method === "POST" && pathname === "/forms") {
    createForm(req, res);
  } else if (req.method === "PUT" && pathname.startsWith("/forms/")) {
    updateForm(req, res, id);
  } else if (req.method === "DELETE" && pathname.startsWith("/forms/")) {
    deleteForm(req, res, id);
  } else {
    res.writeHead(404, { "Content-Type": "application/json" });
    res.end(JSON.stringify({ error: "Not Found" }));
  }
};

```
### Контролери

#### formController.js

```javascript
import { connection } from "./connection.js";
import errorProducer from "./errors.js";
import sendResponse from "./sendResponse.js";
import HTTP_STATUS_CODES from "./statusCode.js";

// Utility to parse JSON body
const parseRequestBody = async (req) => {
  return new Promise((resolve, reject) => {
    let body = "";
    req.on("data", (chunk) => {
      body += chunk.toString();
    });
    req.on("end", () => {
      try {
        resolve(JSON.parse(body));
      } catch (err) {
        reject(err);
      }
    });
  });
};

export const getAllForms = (req, res) => {
  const query = "SELECT * FROM Form";
  console.log(query);

  connection.query(query, (err, results) => {
    if (err) {
      return sendResponse(res, 500, { error: err.message });
    }
    sendResponse(res, 200, results);
  });
};

export const getFormById = (req, res, id) => {
  const query = "SELECT * FROM Form WHERE id = ?";
  console.log(query);

  connection.query(query, [id], (err, results) => {
    if (err) {
      return sendResponse(res, 500, { error: err.message });
    }
    if (results.length === 0) {
      return sendResponse(res, 404, { error: "Form not found" });
    }
    sendResponse(res, 200, results[0]);
  });
};

export const createForm = async (req, res) => {
  try {
    const form = await parseRequestBody(req);
    const query =
      "INSERT INTO Form (id, name, description, content) VALUES (?, ?, ?, ?)";
    console.log(query);

    const values = [
      form.id,
      form.name,
      form.description,
      JSON.stringify(form.content),
    ];

    connection.query(query, values, (err) => {
      if (err) {
        return sendResponse(res, 500, { error: err.message });
      }
      sendResponse(res, 201, { message: "Form created successfully" });
    });
  } catch (error) {
    sendResponse(res, 400, { error: "Invalid request data" });
  }
};

export const updateForm = async (req, res, id) => {
  try {
    const updatedForm = await parseRequestBody(req);
    const query =
      "UPDATE Form SET name = ?, description = ?, content = ? WHERE id = ?";
    const values = [
      updatedForm.name,
      updatedForm.description,
      JSON.stringify(updatedForm.content),
      id,
    ];

    connection.query(query, values, (err, results) => {
      if (err) {
        return sendResponse(res, 500, { error: err.message });
      }
      if (results.affectedRows === 0) {
        return sendResponse(res, 404, { error: "Form not found" });
      }
      sendResponse(res, 200, { message: "Form updated successfully" });
    });
  } catch (error) {
    sendResponse(res, 400, { error: "Invalid request data" });
  }
};

export const deleteForm = (req, res, id) => {
  const query = "DELETE FROM Form WHERE id = ?";

  connection.query(query, [id], (err, results) => {
    if (err) {
      return sendResponse(res, 500, { error: err.message });
    }
    if (results.affectedRows === 0) {
      return sendResponse(res, 404, { error: "Form not found" });
    }
    sendResponse(res, 200, { message: "Form deleted successfully" });
  });
};


```
### Помилик

#### errors.js

```javascript
const createError = (message, statusCode, details = null) => {
  const error = new Error(message);
  error.statusCode = statusCode;
  error.details = details;
  return error;
};

const errorProducer = {
  missingFields: () => createError("Missing required fields", 400),
  notFound: () => createError("Resource not found", 404),
  createError: (details) =>
    createError("Error creating resource", 500, details),
  getError: (details) => createError("Error fetching resource", 500, details),
  updateError: (details) =>
    createError("Error updating resource", 500, details),
  deleteError: (details) =>
    createError("Error deleting resource", 500, details),
};

export default errorProducer;
```
### Коди статусів HTTP

#### statusCode.js

```javascript
const HTTP_STATUS_CODES = {
  OK: 200,
  CREATED: 201,
  BAD_REQUEST: 400,
  NOT_FOUND: 404,
  INTERNAL_SERVER_ERROR: 500,
};

export default HTTP_STATUS_CODES;
```

### Відправка відповіді 

#### sendResponse.js

```javascript
const sendResponse = (res, statusCode, data = null) => {
  const response = {
    message: "Success",
  };

  if (data !== null) {
    response.data = data;
  }

  res.writeHead(statusCode, { "Content-Type": "application/json" });

  res.end(JSON.stringify(response));
};

export default sendResponse;
```
