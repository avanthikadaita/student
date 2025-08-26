---
layout: post
categories: ['CSP Sprint Objectives']
title: Sprint 5 - CSP Data, Debugging, Deployment, and Testing
description: Building a Production Product for College Board CPT
type: issues
courses: {'csp': {'week': 17}}
comments: True
permalink: /csp/sprint5/objectives
---

## Sprint Objectives

1. **Team Project**

   - **Objective for Sprint 5**
     - **Feature Ready Milestone**: Iterate on a project so that it is feature-ready for Team and Individual CPT requirements.
     - **Database Functionality**: Ensure that data used in the system is persistent and dynamic.
     - **Deployment**: Ensure backend and database are public and available for group updates.

   - **Issues/Project Management**
      - **Team Issue**: Define the project purpose and key features. Form an issue to serve as a burndown list. The Team Issue will be used in Team Live reviews and retrospectives.
      - **Individual Issue**: Describe feature purpose and progress toward CPT requirements. The Individual Issue is a burndown list and will be used in Group and Individual reviews.
      - **Kanban Board**: Contain Team or Individual burndown items. Issues are used to grow the project in iterative increments. One or more issues are used to support the burndown of Team and Individual Issues.

   - **Team Organization**
     - **Team Size**: As close to 6 people as possible.
     - **Leadership**: One Scrum Master, and one assistant/backup Scrum Master.
     - **Integrations**: Two people with rights to pull code into the repository.
     - **Deployment Admins**: Two people with rights to deploy.
     - **Visual Design**: One person to maintain front-end design documentation.
     - **Data Modeler**: One person to maintain backend data definition documentation.
     - **Coders**: Everyone will have frontend and backend coding responsibilities for their personal end-to-end feature.
     - **Multiple Roles/Hats**: You will likely have multiple jobs and coding responsibilities.

   - **Classroom Reviews**
     - **Groups/Individual**: Reviews will always be 3 people.
       - **Individual Issue and Kanban Issue(s)**: Show leveraged and distinct features. Highly focused on code and data updates.
       - **Expectation**: This review will clearly show distinct coding on frontend and backend features. Burndown of CPT features highlights CPT questionnaire/writeup.

     - **Team/Table**: Reviews will be 6 people.
       - **Team Issue and Kanban**: Show coordinated progress on Team Goals. Highly focused on demo and functionality.
       - **Expectation**: Requires coordination and design.
     - There will be frequent checkpoints: team, individual, or both.

2. **Extending Flocker Social Media Project**:
   - Your project should have designs for signup, login, and your social media features and actions.  These feature were provided as starters.
   - Additional areas of consideration:
     - Python/Jinja MVC [admin menu](https://flask.opencodingsociety.com/login?next=/users/table2)
     - Javascript Frontend [home page](https://open-coding-society.github.io/flocker_frontend/) a personalized redesign of flocker and its navigation.
     - Database.  Extending provided database to capture information from key individual features in the web site.

3. **Teaching Elements**:
   - There are distinct elements on Big Ideas 1, 2, 3, 4 in your project. Consider how you will capture these elements in your personal blog to aid in your review and study.
   - We will have an MCQ test in this Sprint to help support your personal blog build-up and College Board MCQ preparation.

## Course Priorities related to College Board Create Performance Task (CPT)

### Big Idea 1.4 Debugging Code and Fixing Errors

Students will learn to debug and test their project from frontend to backend.

- **Backend Debugging:**
  - Use Postman to trace and debug backend code.
- **Frontend Debugging:**
  - Utilize browser Inspect tools to trace and debug frontend code.
- **End-to-End Tracing:**
  - Ensure seamless communication and data flow between frontend and backend.
- **Testing:**
  - Build and execute tests using Postman.
  - Add test data to systems to validate functionality and performance.

### Big Idea 2 Data

Students will manage and store data into a database.

- **Database Management with SQLite:**
  - Set up and manage SQLite databases to store user data, posts, and images.
  - Design database schemas to efficiently handle social media data.
- **Image Upload and Storage:**
  - Implement functionality to upload and store images in the database or file system.
  - Ensure images are properly linked to user posts and profiles.
- **Data Security and Privacy:**
  - Implement security measures to protect user data and images.
  - Ensure compliance with privacy regulations and best practices.
- **Data Retrieval and Display:**
  - Develop efficient queries to retrieve and display data on the frontend.
  - Optimize database performance for fast data access.
- **Data Backup and Recovery:**
  - Implement backup strategies to prevent data loss.
  - Develop recovery procedures to restore data in case of failure.

### Big Idea 4 Internet

Students will deploy their Python/Flack project. Additionally, they will learn internet concepts as described.
 
- **Deployment Strategies:**
  - Deploy the application on platforms like Heroku, AWS
  - Use continuous integration and continuous deployment (CI/CD) pipelines to automate deployment.
- **Domain Name System (DNS):**
  - Understand how DNS works and configure domain names for the deployed application.
- **HTTP and RESTful APIs:**
  - Use HTTP methods (GET, POST, PUT, DELETE) to interact with the backend.
  - Design and implement RESTful APIs for communication between frontend and backend.
- **Security and Authentication:**
  - Implement authentication mechanisms (e.g., JWT) to secure the application.
  - Use HTTPS to encrypt data transmitted between the client and server.
- **Performance Optimization:**
  - Optimize frontend performance by minimizing assets and using caching.
  - Optimize backend performance by using efficient algorithms and database indexing.
- **Monitoring and Logging:**
  - Implement monitoring tools to track application performance and errors.
  - Use logging to record application events and debug issues.
