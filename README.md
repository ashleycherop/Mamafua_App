MAMA FUA PLATFORM – COMPLETE SYSTEM DOCUMENTATION
1. System Overview
Mama Fua is an on-demand domestic services marketplace designed to connect clients seeking domestic cleaning services with independent domestic workers (Mama Fuas).
The platform operates as a dual-sided marketplace with three primary user categories:
Clients: Individuals seeking domestic cleaning and related services.
Workers: Independent domestic service providers looking for job opportunities.
Administrators: Platform managers responsible for monitoring operations, managing users, resolving disputes and overseeing transactions.
The system consists of two major technical components:
Android Mobile Application: Provides the user interface and interaction layer for clients and workers.
Backend System: Provides the server-side services responsible for authentication, business logic, data management, job processing, payment workflows and communication with the Android application.
The backend is deployed using Render hosting. Its implementation details require further verification.
Backend URL: https://mamafua-backend-4oig.onrender.com/
2. Overall System Architecture
The Mama Fua platform follows a client-server architecture, with the Android application communicating with the backend through REST API requests.
2.1 Architectural Components
A. Android Application Layer
Responsible for:
User registration and authentication.
Displaying available workers and jobs.
Creating and managing job requests.
Submitting job applications.
Managing worker and client profiles.
Displaying notifications.
Initiating payment requests.
Viewing transaction and earnings information.
B. Backend Application Layer
Responsible for:
Processing incoming API requests.
Validating user credentials and access permissions.
Managing user accounts and profiles.
Managing job creation and application workflows.
Controlling job status transitions.
Coordinating payment and escrow operations.
Processing reviews and disputes.
Managing administrative operations.
Providing data to the Android application.
C. Database Layer
Responsible for persistent storage of platform information, including:
User accounts.
Worker profiles.
Job listings.
Job applications.
Payment transactions.
Reviews and ratings.
Notifications.
Disputes.
Administrative records.
The actual database engine, schema and hosting arrangement must be confirmed from the backend source code.
2.2 System Communication Flow
A user performs an action through the Android application.
The relevant ViewModel receives the action.
The repository sends an HTTP request through Ktor Client.
The backend receives and validates the request.
The relevant business logic is executed.
The backend reads or updates the database where necessary.
A response is returned to the Android application.
The repository updates the local Room database where applicable.
The ViewModel updates its StateFlow.
Jetpack Compose displays the updated information.
3. Android Application Architecture
The Android application follows the MVVM design pattern alongside Clean Architecture principles.
3.1 Domain Layer (domain/)
Contains the core business logic and abstractions.
Responsibilities:
Defines business entities.
Defines repository interfaces.
Contains use cases.
Defines application-level business rules.
Main entities include:
User
Job
WorkerProfile
Review
Payment
Notification
Dispute
3.2 Data Layer (data/)
Responsible for communication with external services and local persistence.
Remote Data Source
Uses Ktor Client with the OkHttp engine.
Communicates with the Mama Fua REST API.
Uses Kotlinx Serialization for data serialization.
Local Data Source
Uses Room Database.
Provides local caching and persistence.
Supports offline access to previously stored information.
Main components include:
AppDatabase
UserDao
JobDao
NotificationDao
Repositories
Repository implementations coordinate remote and local data sources.
Examples:
JobRepositoryImpl
AuthRepositoryImpl
3.3 Presentation Layer (presentation/)
Responsible for the user interface and application state.
Technologies:
Jetpack Compose
Material 3
ViewModels
Kotlin StateFlow
Navigation Compose
The application provides different dashboards and navigation experiences depending on the authenticated user's role.
4. Backend System Architecture
The backend serves as the central processing and data management component of Mama Fua.
Its primary responsibility is to ensure that the Android application does not independently manage critical business operations.
4.1 API Layer
The API layer provides communication endpoints consumed by the Android application.
Expected API functional groups include:
Authentication APIs
User registration.
User login.
Token verification.
Password recovery.
Password reset.
Session management.
User Management APIs
Retrieve user details.
Update user information.
Retrieve user profiles.
Manage account information.
Worker Management APIs
Create and update worker profiles.
Update availability.
Configure hourly rates.
Configure service radius.
Retrieve nearby workers.
Job Management APIs
Create job listings.
Retrieve available jobs.
Retrieve individual job details.
Update job information.
Retrieve jobs belonging to a client.
Retrieve assigned jobs belonging to a worker.
Job Application APIs
Submit applications.
Retrieve job applicants.
Retrieve worker applications.
Accept or reject applications.
Payment APIs
Initiate M-Pesa STK Push.
Receive payment callbacks.
Verify payment status.
Manage escrow balances.
Process worker payouts.
Retrieve transaction histories.
Review and Rating APIs
Submit reviews.
Retrieve reviews.
Calculate profile ratings.
Notification APIs
Create notifications.
Retrieve notifications.
Update notification status.
Dispute Management APIs
Submit disputes.
Retrieve dispute details.
Update dispute status.
Support administrative resolution.
Administrative APIs
Retrieve platform statistics.
Manage user accounts.
Monitor jobs.
Review transactions.
Manage disputes.
Note: These are functional API categories derived from the documented application requirements. They are not a verified list of existing backend routes.
4.2 Business Logic Layer
The backend business logic controls how the different platform operations are executed.
Its responsibilities include:
Enforcing user-role permissions.
Validating job creation requests.
Preventing unauthorized job applications.
Managing job assignment.
Enforcing valid job status transitions.
Coordinating payment confirmation.
Managing escrow release conditions.
Validating reviews and ratings.
Managing dispute resolution.
Enforcing administrative privileges.
4.3 Database Management Layer
The backend database provides centralized persistent storage.
The following entities represent the expected logical data structure:
Entity
Purpose
Users
Stores user account information and roles.
Worker Profiles
Stores worker-specific information, rates and availability.
Jobs
Stores client job requests and their statuses.
Job Applications
Records applications submitted by workers.
Payments
Stores payment transaction records.
Escrow Transactions
Tracks funds held and released.
Reviews
Stores client and worker feedback.
Notifications
Stores platform notifications.
Disputes
Stores reported issues and resolution information.
Admin Records
Stores relevant administrative activity.

The actual database engine, relationships, constraints and migration strategy remain to be verified.
5. Comprehensive Platform Features
5.1 Authentication and User Onboarding
Role-based registration for clients and workers.
Email and password authentication.
JWT-based authentication.
Persistent login sessions.
Secure token storage through TokenManager.
Password recovery and reset.
5.2 Client Features
Discover nearby workers through Maps.
View worker profiles and ratings.
Create domestic service job requests.
Specify job descriptions, budgets and locations.
View job applicants.
Review worker profiles.
Accept suitable applicants.
Track open, in-progress and completed jobs.
Initiate M-Pesa payments.
Confirm job completion.
Submit reviews.
Raise disputes.
5.3 Worker Features
View nearby available jobs.
Filter opportunities by service proximity.
View job descriptions and requirements.
Apply for available jobs.
Manage accepted jobs.
Mark jobs as started.
Request job completion.
Manage personal profiles.
Configure hourly rates.
Configure service radius.
Toggle availability.
Monitor earnings.
View pending escrow balances.
Track completed payouts.
5.4 Shared Features
Job status tracking.
In-app notifications.
Mutual reviews and ratings.
Payment status monitoring.
Job completion confirmation.
Dispute reporting.
User profile management.
5.5 Administrative Features
The administrative component provides platform-level oversight.
Responsibilities include:
Monitoring registered users.
Managing client and worker accounts.
Monitoring job listings.
Tracking job progress.
Monitoring financial transactions.
Reviewing disputes.
Resolving escalated cases.
Managing escrow release decisions.
Monitoring overall platform activity.
The administrative interface and its actual implementation need to be verified separately.
6. Payment and Escrow Architecture
Mama Fua incorporates M-Pesa STK Push to facilitate payments between clients and workers.
6.1 Payment Workflow
A client creates a job.
Workers submit applications.
The client selects a suitable worker.
The application initiates the payment process.
The backend initiates an M-Pesa STK Push request.
The client authorizes the payment.
Safaricom sends a payment callback to the backend.
The backend verifies and records the payment.
The payment is associated with the relevant job.
Funds are held in the escrow process until the job is completed.
The worker requests completion.
The client confirms completion.
The backend processes the eligible worker payout.
6.2 Dispute Handling
When a dispute is submitted:
The backend records the dispute.
The relevant job is flagged.
The disputed payment is prevented from being released.
An administrator reviews the case.
The administrator determines the appropriate resolution.
The payment is released, refunded or otherwise resolved according to the applicable platform policy.
Important: Actual escrow custody, fund settlement and payout mechanisms must be verified. A database record indicating escrow does not by itself establish that funds are held in a regulated escrow account.
7. Backend and Android Integration
The two components communicate through REST APIs.
7.1 Networking
The Android application uses:
Ktor Client.
OkHttp engine.
Kotlinx Serialization.
Kotlin Coroutines.
7.2 Authentication Communication
The Android application stores authentication tokens using TokenManager and includes the relevant authorization credentials in protected API requests.
The backend is responsible for validating tokens and enforcing access permissions.
7.3 Data Synchronization
Room Database provides local persistence.
The repositories coordinate:
Fetching information from the backend.
Updating local records.
Retrieving cached information.
Maintaining application state.
7.4 Error Handling
The integration should account for:
Network connectivity failures.
Authentication errors.
Invalid requests.
Server errors.
Payment failures.
Expired sessions.
Incomplete responses.
8. Security Architecture
The platform requires security controls across both the mobile and backend components.
Key security considerations include:
HTTPS communication.
Secure password hashing.
JWT verification.
Role-based access control.
Input validation.
Protection of sensitive user information.
Secure M-Pesa callback verification.
Protection against duplicate payment processing.
Server-side authorization of job operations.
Audit logs for administrative and financial actions.
Secure management of API credentials and environment variables.
9. Technology Stack
9.1 Android Application
Component
Technology
Programming Language
Kotlin
UI Framework
Jetpack Compose
Design System
Material 3
Architecture
MVVM + Clean Architecture
Dependency Injection
Koin
Networking
Ktor Client
HTTP Engine
OkHttp
Serialization
Kotlinx Serialization
Local Database
Room
Asynchronous Operations
Kotlin Coroutines and Flow
Navigation
Navigation Compose
Image Loading
Coil
Location
Google Play Services Location
Maps
Maps Compose

9.2 Backend System
Component
Current documentation status
Hosting
Render
Backend URL
https://mamafua-backend-4oig.onrender.com/
Programming Language
javascript
Backend Framework
Node 
Database Engine
superbase




Authentication Implementation
JWT is documented at the Android integration level; backend implementation to be verified
API Documentation
Not accessible during inspection
Payment Integration
M-Pesa STK Push is documented; implementation to be verified
Deployment Configuration
To be verified

10. Build and Deployment
10.1 Android Application
Minimum SDK: 24 (Android 7.0).
Target SDK: 34 (Android 14).
Build System: Gradle.
Build Configuration: Kotlin DSL.
Dependency Management: Version Catalogs (libs.versions.toml).
10.2 Backend Deployment
The backend is hosted on Render.
The deployment documentation should cover:
Source repository.
Build and start commands.
Runtime environment.
Environment variables.
Database connection configuration.
Production API URL.
Deployment logs.
Error monitoring.
Database backup procedures.
These deployment details require confirmation from the backend project and Render configuration.
11. Future Technical Documentation and Verification
To complete the technical documentation, the following backend details should be extracted from the actual backend source code:
Backend programming language and framework.
Complete project directory structure.
Database engine and database schema.
Entity relationships and foreign keys.
Complete API endpoint list.
HTTP methods and request/response structures.
Authentication and authorization middleware.
Job lifecycle implementation.
M-Pesa integration and callback handling.
Escrow and payout implementation.
Notification delivery mechanism.
Admin authorization and permissions.
Environment variables and deployment configuration.
Error handling and logging mechanisms.
Testing and backup procedures.
12. Conclusion
Mama Fua is structured as a dual-sided domestic services marketplace supported by an Android mobile application and a centralized backend system.
The Android application provides the primary user experience, while the backend is responsible for centralized business logic, authentication, data processing, job management and payment coordination.
The architecture provides a foundation for managing the complete service lifecycle, from user registration and worker discovery to job execution, payment processing, reviews and dispute resolution.
Completing the backend implementation documentation will provide a comprehensive technical reference for maintaining, extending, testing and scaling the Mama Fua platform.

