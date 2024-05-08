# Detailed Design Document

## Introduction

This Detailed Design Document for **XYZ system** built by **XYZ company** provides a complete description of the system.

### Problem Definition

The project aims to solve **XYZ problem**. _Briefly explain the problem and how the final product will help solve the problem._

### Purpose

_Briefly explain the purpose of the document._

### Scope and objectives

The scope of this document is to provide information about the design procedure of the system. The design constraints, data design, architectural design, security design, user interface design, testing strategies, documentation guidelines, and deployment and implementation plans will be elucidated in the scope of this document. Also chosen helper libraries and complete time planning of the system will be included. The intended audience is the ones who will implement the project, hence it is aimed that this document to be a guideline for those developers.

### Overview

_Briefly explain what each sections of the document contains in different paragraphs._

### Definitions, Acronyms, and Abbreviations

Definitions, acronyms and abbreviations are listed in the below table.

|     |                          |
| --- | ------------------------ |
| DDD | Detailed Design Document |
| UI  | User Interface           |
| ERD | Entity Relation Diagram  |
| TS  | TypeScript               |
| OTP | One Time Password        |

## Data Design

### Data model

The system utilizes a relational data model to organize and manage its data. The data model consists of several tables, each representing a specific entity or concept within the system. Relationships between tables are established using keys, including primary keys and foreign keys.

#### Entity Relation Diagram

![Demo Entity Relation Diagram](/assets/images/ERD.png)

The following entities are represented in the ERD:

1. **User**: Stores information about the user, including user ID, first name, last name, email, contact, and password.
2. **UserRefreshToken**: Contains data related to the user's session, such as refresh token, expiry date of the token, and the user's ID.
3. **UserValidationOTP**: Contains OTP code associated with the user, during email or contact verification process, it includes attributes like the OTP code, expiry date of the code, and user's ID.
4. **VehicleManufacturer**: Represents individual vehicle manufacturing companies, with attributes like manufacturer ID and name.
5. **VehicleType**: Represents vehicle types, with attributes like type ID and type name.
6. **Vehicle**: Represents individual vehicles, with attributes such as vehicle ID, manufacturer ID, type ID, license plate number and availability.
7. **Employee**: Represents individual employees of the company, with attributes such as employee ID, first name, last name, contact number, email, join date and active status.
8. **PaymentSource**: Contains details of different payment sources, including source ID, and name.
9. **PaymentStatus**: Represents payment status like Success, Failed and others, it includes attributes like status ID and status name.
10. **Payment**: Stores information related to the payment transaction associated with vehicle contract, it includes attributes like payment ID, amount, discount percentage, final amount, transaction reference code, source ID, transaction date, completion date, employee remark, and status ID.
11. **UserVehicleContract**: Represents the user's vehicle contract, it includes attributes like contract ID, user's ID, vehicle's ID, contract date, end date of the contract, payment ID, and contract creator's _(employee)_ ID.

#### Table Definition

The following tables are part of the relational data model:

1. **User**:
   |Column Name|Properties|
   |---|---|
   | id | Primary Key, INT |
   |firstName |VARCHAR(255)|
   |lastName| VARCHAR(255)|
   |email| VARCHAR(255)|
   |isEmailValid| BOOLEAN|
   |contact| VARCHAR(255)|
   |isContactValid| BOOLEAN|
   |password| VARCHAR(255)|

1. **UserRefreshToken**:
   |Column Name|Properties|
   |---|---|
   | token | Composite Key, VARCHAR(255) |
   |userId| Composite Key, Foreign Key referencing User, INT|
   |expiresOn |DateTime|

1. **UserValidationOTP**:
   |Column Name|Properties|
   |---|---|
   | otp | Composite Key, VARCHAR(255) |
   |userId| Composite Key, Foreign Key referencing User, INT|
   |expiresOn |DateTime|

1. **VehicleManufacturer**:
   |Column Name|Properties|
   |---|---|
   | id | Primary Key, INT |
   |name| VARCHAR(255)|

1. **VehicleType**:
   |Column Name|Properties|
   |---|---|
   | id | Primary Key, INT |
   |name| VARCHAR(255)|

1. **Vehicle**:
   |Column Name|Properties|
   |---|---|
   |id| Primary Key, INT|
   |manufacturerId| Foreign Key referncing VehicleManufacturer, INT|
   |licencePlateNumber|VARCHAR(255)|
   |isAvailiable| BOOLEAN|
   |typeId| Foreign Key referncing VehicleType, INT|

2. **Employee**:
   |Column Name|Properties|
   |---|---|
   | id | Primary Key, INT |
   |firstName |VARCHAR(255)|
   |lastName| VARCHAR(255)|
   |email| VARCHAR(255)|
   |contact| VARCHAR(255)|
   |isActive| BOOLEAN|
   |joinDate| Date|

3. **PaymentSource**:
   |Column Name|Properties|
   |---|---|
   | id | Primary Key, INT |
   |name| VARCHAR(225)|

4. **PaymentStatus**:
   |Column Name|Properties|
   |---|---|
   | id | Primary Key, INT |
   |name| VARCHAR(225)|

5. **Payment**:
   |Column Name|Properties|
   |---|---|
   | id | Primary Key, INT |
   |amount |MONEY|
   |discount| DOUBLE|
   |finalAmount| MONEY|
   |transactionRef| VARCHAR(255)|
   |sourceId| Foreign Key referncing PaymentSource, INT|
   |transactionDate|DateTime|
   |completionDate|DateTime|
   |remark|LONGTEXT|
   |statusId| Foreign Key referncing PaymentStatus, INT|

6. **UserVehicleContract**:
   |Column Name|Properties|
   |---|---|
   | id | Primary Key, INT |
   |userId| Foreign Key referncing User, INT|
   |vehicleId| Foreign Key referncing Vehicle, INT|
   |contractDate|DateTime|
   |contractEndDate|DateTime|
   |paymentId| Foreign Key referncing Payment, INT|
   |contractBy| Foreign Key referncing Employee, INT|

The relational data model facilitates efficient storage, retrieval, and manipulation of data within the system. Relationships between entities are maintained through foreign key constraints, ensuring data integrity and consistency.

## System Architecture

### High-Level System Architecture

The system architecture follows a monolithic architecture pattern, where all components of the application are containerized using Docker. The architecture consists of the following high-level components:

1. **User Interface (UI) Layer**: Responsible for presenting the user interface and handling user interactions. It includes the frontend components of the application built using React.
2. **Backend Application**: Contains the application logic, business rules, and data processing functionalities. It includes server-side code written in Node.js using Nest.js framework.
3. **Data Storage Layer**: Manages the persistence and retrieval of data.
   1. It includes a relational database system (e.g., MariaDB/PostgreSQL) for storing structured data.
   2. And Redis as an in-memory data store for caching frequently accessed data. It improves performance by reducing database load and latency.
4. **Proxy Layer**: Acts as a reverse proxy and load balancer, directing incoming requests to the appropriate services. It improves security, performance, and scalability by offloading SSL termination, caching, and request routing.
   
The folowing diagram illustrates the high-level system architecture:

![High-level System Architecture Diagram](assets/images/highLevelSystemArchitecture.png)

### Technology stack and Framework Used

The system utilizes the following technologies and frameworks:
1. User Interface (UI) Layer:
   1. HTML, CSS, JavaScript for fornt-end development.
   2. React.js for building interactive user interfaces.
2. Backend Application:
   1. Node.js with Nest.js for building the monolithic backend application.
3. Data Storage Layer:
   1. Mariadb/PostgreSQL for relational database management.
   2. Redis for in-memory caching of frequently accessed data.
4. Nginx Reverse Proxy:
   1. Nginx as a reverse proxy and load balancer.
   
### Deployment Architecture

The deployment architecture of the system involves using Docker containers for packaging and deploying each component of the application separately. The deployment architecture includes:
1. **Docker Containers**: Each component of the monolithic application is packaged as a separate Docker container, including the frontend, backend, database, and Redis components.
2. **Docker Compose**: Docker Compose is used to define and manage multi-container Docker applications. It allows for defining the services, networks, and volumes required for running the application.
3. **Nginx Reverse Proxy**: Nginx is used as a reverse proxy server to route incoming requests to the appropriate backend and frontend servers. It handles load balancing, SSL termination, caching, and request routing.

### Scalability Consideration

The use of Docker containers allows for easy scaling of individual components by deploying multiple instances of each container. Nginx load balancing improves performance and scalability by distributing incoming traffic among multiple backend instances, ensuring optimal resource utilization and high availability. Redis cache improves performance and scalability by caching frequently accessed data, reducing database load and latency.

## User Interface Design

### Design Principles

The User Interface (UI) design of the system is guided by the following principles:

1. **Consistency**: Maintain consistency in layout, styling, and interaction patterns throughout the application to provide a seamless user experience.
2. **Clarity**: Ensure clarity in the presentation of information, use of language, and labeling of UI elements to minimize user confusion.
3. **Accessibility**: Design the UI to be accessible to users of all abilities, including those with disabilities, by adhering to WCAG guidelines and providing alternative text for images and other non-text elements.
4. **Simplicity**: Keep the UI simple and intuitive, avoiding clutter and unnecessary complexity to facilitate ease of use.
5. **Responsiveness**: Ensure that the UI is responsive and adaptable to different screen sizes and devices, providing a consistent experience across desktop, tablet, and mobile platforms.

### Wireframes

Wireframes have been created to visualize the layout and structure of key screens within the application. These wireframes serve as a blueprint for the UI design and provide a reference for developers during implementation.

[Click here](https://www.figma.com/file/PsOKjeZCMApHsjgbnn9Dkp/Meeting?node-id=78-6742&mode=dev) to go to the wireframe.

### Visual Design

The visual design of the UI is characterized by:

1. **Color Scheme**: A clean and modern color palette with subtle gradients and contrasting accents to guide user attention and enhance visual appeal.
2. **Typography**: Utilization of readable and accessible fonts for both headings and body text, ensuring legibility across different screen sizes.
3. **Iconography**: Consistent use of icons and visual cues to convey information and assist with navigation, enhancing user understanding and engagement.
4. **Whitespace**: Strategic use of whitespace to create visual breathing room, improve readability, and draw attention to key UI elements.
   
### Interaction Design

Interaction design focuses on how users interact with the application and includes considerations such as:

1. **Navigation**: Clear and intuitive navigation paths to help users move between different sections of the application easily.
2. **Feedback**: Instant feedback for user actions such as form submissions, button clicks, and error messages to provide reassurance and guidance.
3. **Animations**: Subtle animations and transitions to enhance user experience and provide visual feedback during interactions.
4. **Microinteractions**: Delightful microinteractions such as button hover effects, loading spinners, and tooltips to engage users and improve overall usability.

### Prototype

A clickable prototype has been developed to simulate the user flow and interactions within the application. Stakeholders and end-users can interact with the prototype to provide feedback and validate design decisions before development.

[Click here](https://www.figma.com/file/PsOKjeZCMApHsjgbnn9Dkp/Meeting?node-id=78-6742&mode=dev) to go to the clickable prototype of the system.

### Design Review

The UI design has undergone multiple rounds of review and iteration based on feedback from stakeholders, usability testing, and best practices in UI/UX design. Continuous refinement and improvement are ongoing processes to ensure the UI meets user needs and expectations effectively.

This section provides an overview of the User Interface (UI) Design considerations, including design principles, wireframes, visual design elements, interaction design, accessibility considerations, prototype development, and design review processes.

## Security Design

## Integration and Interface Design

## Deployment and Implementation Plan

## Testing Strategy

## Documentation Guidelines

## Glossary
