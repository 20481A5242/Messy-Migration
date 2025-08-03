Issues Identified


SQL Injection Vulnerability: The original code used f-strings to build SQL queries in several functions (get_user, update_user, delete_user, search_users, and login). This practice is insecure as it allows user-provided input to be directly executed as part of the SQL command, potentially leading to unauthorized data access or modification.

Plain Text Passwords: The application stored and checked user passwords as plain text. This is a critical security risk because if the database were ever compromised, all user passwords would be exposed.

Changes Made
Parameterized Queries: I refactored all database queries to use ? placeholders and pass the user input as a separate tuple. This prevents SQL injection by separating the SQL command from the user data, ensuring the data is never executed as code.

Secure Password Hashing: I implemented the bcrypt library to securely handle passwords. The create_user function now hashes a user's password before saving it to the database. The login function retrieves the hashed password and uses bcrypt.checkpw() to safely verify the user's provided password without storing or exposing the plain text version.

Trade-offs
Added Dependency: By implementing bcrypt, I added a new dependency to the project. This means the application now requires the bcrypt library to be installed to run, which adds a minor layer of setup for new developers.

Increased Code Complexity: The new password hashing and verification logic is slightly more complex than the original plain text comparison.

Future Improvements
Input Validation: The code currently trusts all user input. I would add validation to ensure that user-provided data, such as email format and password length, is correct before processing it.

Error Handling: The application could have more robust error handling to provide clearer feedback to users and prevent the server from crashing on unexpected input.

Configuration Management: The database connection string is currently hard-coded. This could be moved to a configuration file to make the application more flexible and easier to manage in different environments.
