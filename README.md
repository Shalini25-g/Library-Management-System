# Library-Management-System
	The Library Management System is a database project designed to manage various operations of a library using Oracle SQL. This project demonstrates the use of SQL DDL, DML, joins, views, constraints, stored procedures, and triggers. The system handles library functions like managing, issuing and returning books, and tracking overdue items.
# Key Features
	Add, update, and delete book records, members, and staff.
	Issue and return books with due date tracking.
	Automatic fine calculation for overdue returns using triggers.
	Maintain issue history for each borrower.
	Generate reports using SQL views (e.g., available books, overdue books).
	Enforce data integrity through primary keys, foreign keys, and constraints.

# Database Design
Tables:

	Books (BookID, Title, Author, ISBN, Category, Status)
	Members (MemberID, Name, Email, JoinDate)
	Staff (StaffID, Name, Role, Contact)
	IssueRecords (IssueID, BookID, MemberID, IssueDate, DueDate, ReturnDate)
	Fines (FineID, IssueID, Amount, PaidStatus)

Constraints:

	Primary keys on all ID fields
	Foreign keys between IssueRecords → Books, Members
	Check constraints on book status and fine payment

# Technologies Used
Oracle SQL

	DDL: CREATE, ALTER, DROP
	DML: INSERT, UPDATE, DELETE
	Queries: SELECT, JOIN, GROUP BY, ORDER BY
	Views, Indexes, Sequences
	Stored Procedures and Triggers
