# Materialise
Developer Assignment Submission Notes
C# Backend – Audit Trail Task
Candidate: Cibi Chakrawarthy Dhandapani
What I Have Done (Deliverables Achieved)
Implemented an audit trail in the .NET backend that captures:
What was changed (item value)
When the change occurred (timestamp)
Who performed the action (username)

The audit trail is recorded for both:


Adding a new item


Deleting an existing item



Access to the audit trail is strictly limited to Admin users (validated through header).
The code is cleanly structured with proper service/controller separation and comments.

Important for Testing (Please Read)
When a user with the role User adds an item, the item is successfully added and an audit log is created internally.  And also user must enter a username to add the value. 
If the role is User, the "Fetch Audit Trail" button is not visible at all in the UI.
This is done intentionally to prevent regular users from even attempting to access the audit endpoint.
Only users with role: Admin will see the button and be able to trigger the API.

Only the user with username: cibi and role: Admin can access the audit trail.


If any other user (e.g., username: john, role: User) tries to access: GET /api/items/audit
They will receive a :




Why this is hardcoded:
To keep the assignment within scope and avoid overengineering, I decided to simulate role-based access by directly checking the username and role from headers.
In real-world scenarios, JWT , cookie-based authentication, azure AD would be used  but those require user registration, database setup, hashing, etc.
Since the provided project uses only in-memory lists and no database, this approach was used to clearly demonstrate the intended access control without adding unnecessary complexity.
This also makes it easy for reviewers to test the feature immediately in Postman or Swagger without needing to set up login or tokens.
Improvements Done in the Code
Added AuditEntry model to represent audit logs
Trimmed input values to avoid saving empty or whitespace-only data
Manually enforced role-based access control and returned proper Forbidden for unauthorized users
Added XML-style comments for Swagger documentation and clarity
Used consistent ApiResponse<T> wrappers across all endpoints
Moved logic to ItemService to keep controller clean and follow separation of concerns
Included ILogger usage to capture errors and unauthorized access attempts
Added test project setup to allow future unit testing
How It Works
User calls the API to add or delete an item
System records an audit log with:
 Action (Add or Delete)
Item value
Timestamp (in UTC)
PerformedBy (username)
Audit logs are stored in-memory and can be retrieved using: GET /api/items/audit
This route returns data only if the headers contain:
 username: cibi
 role: Admin
Any other values will receive a 403 error.

Technologies Used
ASP.NET Core (.NET 8 Web API)
In-memory list for storing both items and audit logs
Header-based role and identity simulation
Swagger for API testing
xUnit test project structure included
Logger integration (ILogger) for basic observability
What Can Be Done Further
Replace in-memory storage with a database (e.g., SQL Server) using EF Core
Implement auto-increment Id in AuditEntry (already planned)
Add an UpdateItem feature and track both old and new values in audit
Use JWT-based authentication and role-based access using claims
Add user registration/login with encrypted credentials
Protect routes using [Authorize(Roles = "Admin")] once auth is in place
Add unit tests to verify audit trail, access control, and error handling
Save logs in file system or send them to a log aggregator (e.g., Serilog or Application Insights)
Implement frontend-based access restriction to hide audit view from regular users
Final Note
I selected the .NET backend, My goal was to keep the code clear, simple, and extensible, while demonstrating understanding of clean architecture, basic security, and audit trail design.
Please remember: only cibi with role Admin can see the audit log, and this is intentional to simplify testing and highlight the access control logic.

Thank you for reviewing this task.
— Cibi Chakrawarthy
