# Library-Management-System
	The Library Management System is a database project designed to manage various operations of a library using Oracle SQL. This project demonstrates the use of SQL DDL, DML, joins, views, constraints, stored procedures, and triggers. The system handles library functions like managing, issuing and returning books, and tracking overdue items.




 Code:
create table books(isbn varchar2(25),	book_title varchar2(60),	category varchar2(25),	rental_price float(5),	status varchar2(10),
                    author varchar2(30),	publisher varchar2(30));
alter table books add constraint pk_books primary key(isbn);
                    
create table branch(branch_id varchar2(10),	manager_id varchar2(10),	branch_address varchar2(30),	contact_no varchar2(20));
alter table branch add constraint pk_branch primary key(branch_id);

create table employees(emp_id varchar2(10) primary key,	emp_name varchar2(30),	position varchar2(10),	salary number(10),	branch_id varchar2(10));

create table issued_status(issued_id varchar2(10) primary key,	issued_member_id varchar2(10),	issued_book_name varchar2(60),	issued_date varchar2(10),
                            issued_book_isbn varchar2(20),issued_emp_id varchar2(10));

create table members(member_id varchar2(10) primary key, member_name varchar2(20),	member_address varchar2(30),	reg_date varchar2(10));

create table return_status(return_id varchar2(10) primary key,	issued_id varchar2(10),	return_book_name varchar2(30),	return_date varchar2(10),	return_book_isbn varchar2(10));

-----Assigning foreign keys:

alter table employees add constraint fk_employees foreign key (branch_id) references branch(branch_id);

alter table issued_status add constraint fk_issued_status foreign key(issued_member_id) references members(member_id);

alter table issued_status add constraint fk_issued_books foreign key(issued_book_isbn) references books(isbn);

alter table issued_status add constraint fk_issued_employees foreign key(issued_emp_id) references employees(emp_id);

alter table return_status add constraint fk_return_issued_id foreign key(issued_id) references issued_status(issued_id);
alter table return_status drop constraint fk_return_issued_id;

alter table return_status add constraint fk_return_book_isbn foreign key(return_book_isbn) references books(isbn);


---------Inserting data into tables---------

insert into members values ('C101', 'Alice Johnson', '123 Main St', '2021-05-15');
insert into members values ('C102', 'Bob Smith', '456 Elm St', '2021-06-20');
insert into members values ('C103', 'Carol Davis', '789 Oak St', '2021-07-10');
insert into members values ('C104', 'Dave Wilson', '567 Pine St', '2021-08-05');
insert into members values ('C105', 'Eve Brown', '890 Maple St', '2021-09-25');
insert into members values ('C106', 'Frank Thomas', '234 Cedar St', '2021-10-15');
insert into members values ('C107', 'Grace Taylor', '345 Walnut St', '2021-11-20');
insert into members values ('C108', 'Henry Anderson', '456 Birch St', '2021-12-10');
insert into members values ('C109', 'Ivy Martinez', '567 Oak St', '2022-01-05');
insert into members values ('C110', 'Jack Wilson', '678 Pine St', '2022-02-25');
insert into members values ('C118', 'Sam', '133 Pine St', '2024-06-01');    
insert into members values ('C119', 'John', '143 Main St', '2024-05-01');

select * from members;


insert into branch values('B001', 'E109', '123 Main St', '+919099988676');
insert into branch values('B002', 'E109', '456 Elm St', '+919099988677');
insert into branch values('B003', 'E109', '789 Oak St', '+919099988678');
insert into branch values('B004', 'E110', '567 Pine St', '+919099988679');
insert into branch values('B005', 'E110', '890 Maple St', '+919099988680');

select * from branch;

insert into employees values('E101', 'John Doe', 'Clerk', 60000.00, 'B001');
insert into employees values('E102', 'Jane Smith', 'Clerk', 45000.00, 'B002');
insert into employees values('E103', 'Mike Johnson', 'Librarian', 55000.00, 'B001');
insert into employees values('E104', 'Emily Davis', 'Assistant', 40000.00, 'B001');
insert into employees values('E105', 'Sarah Brown', 'Assistant', 42000.00, 'B001');
insert into employees values('E106', 'Michelle Ramirez', 'Assistant', 43000.00, 'B001');
insert into employees values('E107', 'Michael Thompson', 'Clerk', 62000.00, 'B005');
insert into employees values('E108', 'Jessica Taylor', 'Clerk', 46000.00, 'B004');
insert into employees values('E109', 'Daniel Anderson', 'Manager', 57000.00, 'B003');
insert into employees values('E110', 'Laura Martinez', 'Manager', 41000.00, 'B005');
insert into employees values('E111', 'Christopher Lee', 'Assistant', 65000.00, 'B005');

select * from employees;

insert into books values('978-0-553-29698-2', 'The Catcher in the Rye', 'Classic', 7.00, 'yes', 'J.D. Salinger', 'Little, Brown and Company');
insert into books values('978-0-330-25864-8', 'Animal Farm', 'Classic', 5.50, 'yes', 'George Orwell', 'Penguin Books');
insert into books values('978-0-14-118776-1', 'One Hundred Years of Solitude', 'Literary Fiction', 6.50, 'yes', 'Gabriel Garcia Marquez', 'Penguin Books');
insert into books values('978-0-525-47535-5', 'The Great Gatsby', 'Classic', 8.00, 'yes', 'F. Scott Fitzgerald', 'Scribner');
insert into books values('978-0-141-44171-6', 'Jane Eyre', 'Classic', 4.00, 'yes', 'Charlotte Bronte', 'Penguin Classics');
insert into books values('978-0-307-37840-1', 'The Alchemist', 'Fiction', 2.50, 'yes', 'Paulo Coelho', 'HarperOne');
insert into books values('978-0-679-76489-8', 'Harry Potter and the Sorcerers Stone', 'Fantasy', 7.00, 'yes', 'J.K. Rowling', 'Scholastic');
insert into books values('978-0-7432-4722-4', 'The Da Vinci Code', 'Mystery', 8.00, 'yes', 'Dan Brown', 'Doubleday');
insert into books values('978-0-09-957807-9', 'A Game of Thrones', 'Fantasy', 7.50, 'yes', 'George R.R. Martin', 'Bantam');
insert into books values('978-0-393-05081-8', 'A Peoples History of the United States', 'History', 9.00, 'yes', 'Howard Zinn', 'Harper Perennial');
insert into books values('978-0-19-280551-1', 'The Guns of August', 'History', 7.00, 'yes', 'Barbara W. Tuchman', 'Oxford University Press');
insert into books values('978-0-307-58837-1', 'Sapiens: A Brief History of Humankind', 'History', 8.00, 'no', 'Yuval Noah Harari', 'Harper Perennial');
insert into books values('978-0-375-41398-8', 'The Diary of a Young Girl', 'History', 6.50, 'no', 'Anne Frank', 'Bantam');
insert into books values('978-0-14-044930-3', 'The Histories', 'History', 5.50, 'yes', 'Herodotus', 'Penguin Classics');
insert into books values('978-0-393-91257-8', 'Guns, Germs, and Steel: The Fates of Human Societies', 'History', 7.00, 'yes', 'Jared Diamond', 'W. W. Norton'||' &'||' Company');
insert into books values('978-0-7432-7357-1', '1491: New Revelations of the Americas Before Columbus', 'History', 6.50, 'no', 'Charles C. Mann', 'Vintage Books');
insert into books values('978-0-679-64115-3', '1984', 'Dystopian', 6.50, 'yes', 'George Orwell', 'Penguin Books');
insert into books values('978-0-14-143951-8', 'Pride and Prejudice', 'Classic', 5.00, 'yes', 'Jane Austen', 'Penguin Classics');
insert into books values('978-0-452-28240-7', 'Brave New World', 'Dystopian', 6.50, 'yes', 'Aldous Huxley', 'Harper Perennial');
insert into books values('978-0-670-81302-4', 'The Road', 'Dystopian', 7.00, 'yes', 'Cormac McCarthy', 'Knopf');
insert into books values('978-0-385-33312-0', 'The Shining', 'Horror', 6.00, 'yes', 'Stephen King', 'Doubleday');
insert into books values('978-0-451-52993-5', 'Fahrenheit 451', 'Dystopian', 5.50, 'yes', 'Ray Bradbury', 'Ballantine Books');
insert into books values('978-0-345-39180-3', 'Dune', 'Science Fiction', 8.50, 'yes', 'Frank Herbert', 'Ace');
insert into books values('978-0-375-50167-0', 'The Road', 'Dystopian', 7.00, 'yes', 'Cormac McCarthy', 'Vintage');
insert into books values('978-0-06-025492-6', 'Where the Wild Things Are', 'Children', 3.50, 'yes', 'Maurice Sendak', 'HarperCollins');
insert into books values('978-0-06-112241-5', 'The Kite Runner', 'Fiction', 5.50, 'yes', 'Khaled Hosseini', 'Riverhead Books');
insert into books values('978-0-06-440055-8', 'Charlotte''s Web', 'Children', 4.00, 'yes', 'E.B. White', 'Harper'||' &'||' Row');
insert into books values('978-0-679-77644-3', 'Beloved', 'Fiction', 6.50, 'yes', 'Toni Morrison', 'Knopf');
insert into books values('978-0-14-027526-3', 'A Tale of Two Cities', 'Classic', 4.50, 'yes', 'Charles Dickens', 'Penguin Books');
insert into books values('978-0-7434-7679-3', 'The Stand', 'Horror', 7.00, 'yes', 'Stephen King', 'Doubleday');
insert into books values('978-0-451-52994-2', 'Moby Dick', 'Classic', 6.50, 'yes', 'Herman Melville', 'Penguin Books');
insert into books values('978-0-06-112008-4', 'To Kill a Mockingbird', 'Classic', 5.00, 'yes', 'Harper Lee', 'J.B. Lippincott'||' &'||' Co.');
insert into books values('978-0-553-57340-1', '1984', 'Dystopian', 6.50, 'yes', 'George Orwell', 'Penguin Books');
insert into books values('978-0-7432-4722-5', 'Angels'||' &'||' Demons', 'Mystery', 7.50, 'yes', 'Dan Brown', 'Doubleday');
insert into books values('978-0-7432-7356-4', 'The Hobbit', 'Fantasy', 7.00, 'yes', 'J.R.R. Tolkien', 'Houghton Mifflin Harcourt');


select * from books;

insert into issued_status values('IS106', 'C106', 'Animal Farm', '2024-03-10', '978-0-330-25864-8', 'E104');
insert into issued_status values('IS107', 'C107', 'One Hundred Years of Solitude', '2024-03-11', '978-0-14-118776-1', 'E104');
insert into issued_status values('IS108', 'C108', 'The Great Gatsby', '2024-03-12', '978-0-525-47535-5', 'E104');
insert into issued_status values('IS109', 'C109', 'Jane Eyre', '2024-03-13', '978-0-141-44171-6', 'E105');
insert into issued_status values('IS110', 'C110', 'The Alchemist', '2024-03-14', '978-0-307-37840-1', 'E105');
insert into issued_status values('IS111', 'C109', 'Harry Potter and the Sorcerers Stone', '2024-03-15', '978-0-679-76489-8', 'E105');
insert into issued_status values('IS112', 'C109', 'A Game of Thrones', '2024-03-16', '978-0-09-957807-9', 'E106');
insert into issued_status values('IS113', 'C109', 'A Peoples History of the United States', '2024-03-17', '978-0-393-05081-8', 'E106');
insert into issued_status values('IS114', 'C109', 'The Guns of August', '2024-03-18', '978-0-19-280551-1', 'E106');
insert into issued_status values('IS115', 'C109', 'The Histories', '2024-03-19', '978-0-14-044930-3', 'E107');
insert into issued_status values('IS116', 'C110', 'Guns, Germs, and Steel: The Fates of Human Societies', '2024-03-20', '978-0-393-91257-8', 'E107');
insert into issued_status values('IS117', 'C110', '1984', '2024-03-21', '978-0-679-64115-3', 'E107');
insert into issued_status values('IS118', 'C101', 'Pride and Prejudice', '2024-03-22', '978-0-14-143951-8', 'E108');
insert into issued_status values('IS119', 'C110', 'Brave New World', '2024-03-23', '978-0-452-28240-7', 'E108');
insert into issued_status values('IS120', 'C110', 'The Road', '2024-03-24', '978-0-670-81302-4', 'E108');
insert into issued_status values('IS121', 'C102', 'The Shining', '2024-03-25', '978-0-385-33312-0', 'E109');
insert into issued_status values('IS122', 'C102', 'Fahrenheit 451', '2024-03-26', '978-0-451-52993-5', 'E109');
insert into issued_status values('IS123', 'C103', 'Dune', '2024-03-27', '978-0-345-39180-3', 'E109');
insert into issued_status values('IS124', 'C104', 'Where the Wild Things Are', '2024-03-28', '978-0-06-025492-6', 'E110');
insert into issued_status values('IS125', 'C105', 'The Kite Runner', '2024-03-29', '978-0-06-112241-5', 'E110');
insert into issued_status values('IS126', 'C105', 'Charlotte''s Web', '2024-03-30', '978-0-06-440055-8', 'E110');
insert into issued_status values('IS127', 'C105', 'Beloved', '2024-03-31', '978-0-679-77644-3', 'E110');
insert into issued_status values('IS128', 'C105', 'A Tale of Two Cities', '2024-04-01', '978-0-14-027526-3', 'E110');
insert into issued_status values('IS129', 'C105', 'The Stand', '2024-04-02', '978-0-7434-7679-3', 'E110');
insert into issued_status values('IS130', 'C106', 'Moby Dick', '2024-04-03', '978-0-451-52994-2', 'E101');
insert into issued_status values('IS131', 'C106', 'To Kill a Mockingbird', '2024-04-04', '978-0-06-112008-4', 'E101');
insert into issued_status values('IS132', 'C106', 'The Hobbit', '2024-04-05', '978-0-7432-7356-4', 'E106');
insert into issued_status values('IS133', 'C107', 'Angels'||' &'||' Demons', '2024-04-06', '978-0-7432-4722-5', 'E106');
insert into issued_status values('IS134', 'C107', 'The Diary of a Young Girl', '2024-04-07', '978-0-375-41398-8', 'E106');
insert into issued_status values('IS135', 'C107', 'Sapiens: A Brief History of Humankind', '2024-04-08', '978-0-307-58837-1', 'E108');
insert into issued_status values('IS136', 'C107', '1491: New Revelations of the Americas Before Columbus', '2024-04-09', '978-0-7432-7357-1', 'E102');
insert into issued_status values('IS137', 'C107', 'The Catcher in the Rye', '2024-04-10', '978-0-553-29698-2', 'E103');
insert into issued_status values('IS138', 'C108', 'The Great Gatsby', '2024-04-11', '978-0-525-47535-5', 'E104');
insert into issued_status values('IS139', 'C109', 'Harry Potter and the Sorcerers Stone', '2024-04-12', '978-0-679-76489-8', 'E105');
insert into issued_status values('IS140', 'C110', 'Animal Farm', '2024-04-13', '978-0-330-25864-8', 'E102');


select * from issued_status;

insert into return_status(return_id, issued_id, return_date) values('RS101', 'IS101', '2023-06-06');
insert into return_status(return_id, issued_id, return_date) values('RS102', 'IS105', '2023-06-07');
insert into return_status(return_id, issued_id, return_date) values('RS103', 'IS103', '2023-08-07');
insert into return_status(return_id, issued_id, return_date) values('RS104', 'IS106', '2024-05-01');
insert into return_status(return_id, issued_id, return_date) values('RS105', 'IS107', '2024-05-03');
insert into return_status(return_id, issued_id, return_date) values('RS106', 'IS108', '2024-05-05');
insert into return_status(return_id, issued_id, return_date) values('RS107', 'IS109', '2024-05-07');
insert into return_status(return_id, issued_id, return_date) values('RS108', 'IS110', '2024-05-09');
insert into return_status(return_id, issued_id, return_date) values('RS109', 'IS111', '2024-05-11');
insert into return_status(return_id, issued_id, return_date) values('RS110', 'IS112', '2024-05-13');
insert into return_status(return_id, issued_id, return_date) values('RS111', 'IS113', '2024-05-15');
insert into return_status(return_id, issued_id, return_date) values('RS112', 'IS114', '2024-05-17');
insert into return_status(return_id, issued_id, return_date) values('RS113', 'IS115', '2024-05-19');
insert into return_status(return_id, issued_id, return_date) values('RS114', 'IS116', '2024-05-21');
insert into return_status(return_id, issued_id, return_date) values('RS115', 'IS117', '2024-05-23');
insert into return_status(return_id, issued_id, return_date) values('RS116', 'IS118', '2024-05-25');
insert into return_status(return_id, issued_id, return_date) values('RS117', 'IS119', '2024-05-27');
insert into return_status(return_id, issued_id, return_date) values('RS118', 'IS120', '2024-05-29');

select * from return_status;


-----Task 1. Create a New Book Record -- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')"

insert into books values('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott'||' &'||' Co.');

-----Task 2.Update an Existing Member's Address

update members set member_address='400Mayfair St' where member_id='C105';

-----Task 3: Delete a Record from the Issued Status Table -- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.

delete from issued_status where issued_id='IS121';

-----Task 4: Retrieve All Books Issued by a Specific Employee -- Objective: Select all books issued by the employee with emp_id = 'E101'.
select issued_book_name from issued_status where issued_emp_id='E101';

-----Task 5: List Members Who Have Issued More Than One Book -- Objective: Use GROUP BY to find members who have issued more than one book.
select issued_emp_id,count(*) from issued_status group by issued_emp_id  having count(*)>1;

-----Task 6: Create Summary Tables: Used CTAS to generate new tables based on query results - each book and total book_issued_cnt**
create table issued_count as
select isbn,count(isbn) total_books, count(issued_id) total_book_issued_count from books b
inner join issued_status ist  on b.isbn=ist.issued_book_isbn
group by isbn;

drop table issued_count;


------Task 7. Retrieve All Books in a Specific Category:
select * from books where category = 'Classic';

------Task 8: Find Total Rental Income by Category:

select books.category,sum(books.rental_price) total_rental_income from books
inner join issued_status  on issued_status.issued_book_isbn=books.isbn
group by books.category;

------Task 9: List Members Who Registered in the Last 180 Days:

select member_name from members where to_date(reg_date, 'YYYY-MM-DD')>sysdate-180;

------Task 10:List Employees with Their Branch Manager's Name and their branch details:

select 
    e1.emp_id,
    e1.emp_name,
    e1.position,
    e1.salary,
    b.branch_id,
    b.branch_address,
    b.contact_no,
    b.manager_id,
    e2.emp_name as manager
from employees e1
join branch b on e1.branch_id = b.branch_id
join employees e2 on e2.emp_id = b.manager_id
order by branch_id;

------Task 11:Create a Table of Books with Rental Price Above a Certain Threshold:

create table price as
select book_title,rental_price from books where rental_price>6.0;


------Task 12:Retrieve the List of Books Not Yet Returned

select * from issued_status ist
left join return_status rs
  on rs.issued_id = ist.issued_id
where rs.return_id is null;


------Task 13: Identify Members with Overdue Books

create table overdue_books as
select m.member_name,ist.issued_book_name,ist.issued_date,r.return_date from members m
join issued_status ist on ist.issued_member_id=m.member_id
right join return_status r on r.issued_id=ist.issued_id ;

alter table overdue_books add diff number(5);

update overdue_books set diff=(to_date(return_date, 'YYYY-MM-DD')- to_date(issued_date, 'YYYY-MM-DD'));

select member_name,issued_book_name,diff from overdue_books where diff>60 ;

------Task 14: Update Book Status on Return

create table book_status as
select issued_date,return_date,issued_book_name  from overdue_books; 


alter table book_status add status varchar2(20);
update book_status set status='Received' where issued_date is not null and return_date is not null;
update book_status set status='Not Received' where issued_date is null and return_date is not null;


------Task:
select count(issued_id) issued_no_of_books from issued_status;
select count(isbn) total_books from books;
select count(return_id) returned_books from return_status;

create table no_return as
select ist.issued_id,ist.issued_book_name,ist.issued_date,ist.issued_emp_id from issued_status ist
left join return_status rs
on ist.issued_id=rs.issued_id
where rs.issued_id is null;

select count(issued_book_name) from no_return;

-------Task 15:Income given by each employee:
create table  income as 
select issued_status.issued_id,issued_book_name,issued_emp_id,issued_status.issued_date,return_id,
return_status.return_date,emp_name,branch.branch_id,branch.manager_id,books.rental_price,to_date(return_date, 'YYYY-MM-DD')-to_date(issued_date, 'YYYY-MM-DD') no_of_days ,
rental_price*(to_date(return_date, 'YYYY-MM-DD')-to_date(issued_date, 'YYYY-MM-DD')) total_rental_price
from issued_status 
join return_status on issued_status.issued_id=return_status.issued_id
join employees on employees.emp_id=issued_status.issued_emp_id
join branch on branch.branch_id=employees.branch_id
join books on books.isbn=issued_status.issued_book_isbn;


select emp_name, branch_id,sum(total_rental_price) from income group by branch_id,emp_name;


---Task 16: Branch Performance Report
---Create a query that generates a performance report for each branch, showing the number of books issued, the number of books returned, and the total revenue generated from book rentals
create table branch_report as
select branch. branch_id,manager_id,count(ist.issued_id) no_of_books_issued,count(rs.return_id) no_of_books_returned,sum(books.rental_price) total_revenue from issued_status ist
left join return_status rs
on ist.issued_id=rs.issued_id
join employees
on employees.emp_id=ist.issued_emp_id
right join branch 
on employees.branch_id=branch.branch_id
join books
on ist.issued_book_isbn=books.isbn
group by  branch. branch_id,manager_id;

----Task 17:Use the CREATE TABLE AS (CTAS) statement to create a new table active_members containing members who have issued at least one book in the last 2 months.

create table active_members as
select m.member_id,m.member_name,count(ist.issued_member_id)no_of_issued_book from issued_status ist
full join members m 
on ist.issued_member_id=m.member_id
group by m.member_id,m.member_name having count(ist.issued_member_id)>0;

drop table active_members;

select issued_date,add_months(to_date(issued_date, 'YYYY-MM-DD'),-2) from issued_status;


-----Task 18: Find Employees with the Most Book Issues Processed
---Write a query to find the top 3 employees who have processed the most book issues. Display the employee name, number of books processed, and their branch.
select e.emp_name,e.emp_id,ist.issued_emp_id,e.branch_id,count(ist.issued_emp_id) from employees e
join issued_status ist
on ist.issued_emp_id=e.emp_id
group by  e.emp_name,e.emp_id,ist.issued_emp_id,e.branch_id 
order by count(ist.issued_emp_id)desc
fetch first 3 rows only;

----Incase of tie,use dense_rank() over count(ist.issued_emp_id)
select *
from (
    select 
        emp_name,
        emp_id,
        issued_emp_id,
        branch_id,
        count(ist.issued_emp_id) as issued_count,
        dense_rank() over (order by count(ist.issued_emp_id) desc) as rnk
    from issued_status ist
    join employees e on e.emp_id = ist.issued_emp_id
    group by emp_name, emp_id, issued_emp_id, branch_id
)
where rnk <= 3;

----Task 19: Identify Members Issuing High-Risk Books
---Write a query to identify members who have issued books more than twice with the status "damaged" in the books table.
---Display the member name, book title, and the number of times they've issued damaged books.

create table high_risk_books as
select  m.member_name,count(*) no_of_damaged_books,
        listagg(b.book_title, ', ') within group (order by b.book_title) as damaged_book_titles -------------forming list of books
from issued_status ist
join members m
on ist.issued_member_id=m.member_id
join books b
on ist.issued_book_isbn=b.isbn 
where b.status='yes'
group by m.member_name having count(*)>2 ;


------Task 20:Create a stored procedure to manage the status of books in a library system.
set serveroutput on;
create or replace procedure book_availability (p_issued_id  varchar2,p_issued_member_id  varchar2,
                            p_issued_book_isbn varchar2,p_issued_emp_id varchar2)
as
    v_status varchar2(10);
begin
    select status into v_status from books where books.isbn=p_issued_book_isbn;
    if v_status='yes' then
        insert into issued_status(issued_id, issued_member_id, issued_date, issued_book_isbn, issued_emp_id)
        values
        (p_issued_id, p_issued_member_id, sysdate, p_issued_book_isbn, p_issued_emp_id);
    
        update books set status='no' where isbn=p_issued_book_isbn;
        dbms_output.put_line('Book records added successfully for book isbn: ' || p_issued_book_isbn);
    else
        dbms_output.put_line('Sorry to inform you the book you have requested is unavailable book_isbn: '|| p_issued_book_isbn);
    end if;
    
exception
    when no_data_found then
        dbms_output.put_line('No book found with ISBN: ' || p_issued_book_isbn);
    when others then
        dbms_output.put_line('An error occurred: ' || sqlerrm);
end;
/


call book_availability('IS155', 'C108', '978-0-553-29698-2', 'E104');
call book_availability('IS160','C101','978-1-60129-456-2','E105');
call book_availability('IS160','C101','978-1-50129-656-2','E105');

----Another way to call procedure
begin
    book_availability('IS160','C101','978-1-50129-656-2','E105');
end;
/

-----Task 20: Create Table As Select (CTAS) Objective: Create a CTAS (Create Table As Select) query to identify overdue books and calculate fines.
create table fine as
select ist.issued_id,ist.issued_book_name,ist.issued_date,r.return_id,r.return_date,round(nvl(to_date(r.return_date,'YYYY-MM-DD'), sysdate) - to_date(ist.issued_date,'YYYY-MM-DD')) as no_of_days,
        b.rental_price,round((b.rental_price*(nvl(to_date(r.return_date,'YYYY-MM-DD'), sysdate) - to_date(ist.issued_date,'YYYY-MM-DD')))) income 
from issued_status ist
left join return_status r
on ist.issued_id=r.issued_id
join books b
on ist.issued_book_isbn=b.isbn;

drop table fine;


update fine set income=round(65*rental_price+(with_fine*(nvl(to_date(return_date,'YYYY-MM-DD'), sysdate) - to_date(issued_date,'YYYY-MM-DD')-65))) where nvl(to_date(return_date,'YYYY-MM-DD'), sysdate) - to_date(issued_date,'YYYY-MM-DD')>65;

alter table fine add with_fine float(10);
alter table fine add extra_days number(20);

update fine set with_fine= rental_price+1.0 where (nvl(to_date(return_date,'YYYY-MM-DD'), sysdate) - to_date(issued_date,'YYYY-MM-DD')) >65;
update fine set extra_days=round((nvl(to_date(return_date,'YYYY-MM-DD'), sysdate) - to_date(issued_date,'YYYY-MM-DD')-65))where nvl(to_date(return_date,'YYYY-MM-DD'), sysdate) - to_date(issued_date,'YYYY-MM-DD')>65;

alter table fine add normal_income number(10);
update fine set normal_income=round(rental_price*(nvl(to_date(return_date,'YYYY-MM-DD'), sysdate) - to_date(issued_date,'YYYY-MM-DD'))) where (nvl(to_date(return_date,'YYYY-MM-DD'), sysdate) - to_date(issued_date,'YYYY-MM-DD'))<=65;
update fine set normal_income=round(rental_price*65) where normal_income is null;

alter table fine add fine_income number(10);
update fine set fine_income=round(with_fine)*round((nvl(to_date(return_date,'YYYY-MM-DD'), sysdate) - to_date(issued_date,'YYYY-MM-DD')-65)) where (nvl(to_date(return_date,'YYYY-MM-DD'), sysdate) - to_date(issued_date,'YYYY-MM-DD'))>65;

alter table fine rename column income to total_income;

