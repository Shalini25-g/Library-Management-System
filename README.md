# Library-Management-System
	The Library Management System is a database project designed to manage various operations of a library using Oracle SQL. This project demonstrates the use of SQL DDL, DML, joins, views, constraints, stored procedures, and triggers. The system handles library functions like managing, issuing and returning books, and tracking overdue items.




 Code:
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

