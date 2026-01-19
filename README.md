# Database-Library-Management-System-project
Library Management System



Database Management Systems



Submitted By:


 Tanbir Hasan Taz (2023100000415)
Yousuf Howlader (2020000000150)
Md Fahmidul Islam Inkiat (2023100000666)



Submitted To:
S.I.M. Adnan
Lecturer

Department of Computer Science and Engineering

Southeast University



1. Introduction
Libraries are essential institutions for organizing and providing access to knowledge resources. With the increasing number of books, users, and transactions, manual library management systems have become inefficient and prone to errors. As a result, database-driven systems are required to ensure accuracy, consistency, and efficient data handling.
The Library Management System (LMS) is a relational database system designed to manage information related to library users, books, authors, categories, loans, and reservations. The system supports core library operations such as book borrowing, reservation management, and tracking issue, due, and return dates. Proper use of primary and foreign keys ensures data integrity and minimizes redundancy.
This project focuses on the design and implementation of a normalized database using Entity-Relationship modeling and relational schema design. The database is normalized up to Third Normal Form (3NF) to improve efficiency and maintain data consistency. SQL is used to implement the database schema and perform meaningful queries.
The scope of this project is limited to backend database design and implementation. However, the system can be extended in the future by integrating a front-end application and additional features.
Objectives and Scope
To design a well-structured relational database for a library management system.
To efficiently manage library resources including books, users, authors, categories, loans, and reservations.
To apply Entity-Relationship (ER) modeling, normalization up to Third Normal Form (3NF), and SQL-based implementation.
To support efficient querying for library operations such as borrowing history, book availability, overdue tracking, and reservation records.

2. Requirements Description
The Vehicle Rental Management System involves the following entities and users:
Entities
User: Stores personal and contact information of library members.
Book: Stores details of books available in the library.
Author: Stores information about book authors.
Category: Stores book classification details.
Loan: Manages book borrowing transactions between users and the library.
Reservation: Records book reservation information.
Book_Author: Manages the many-to-many relationship between books and authors.
Users
Admin (Librarian): Manages books, authors, categories, and system data.
Library Member: Borrows and reserves books.
Main Functionalities
Add, update, and remove book records.
Register and manage library users.
Manage book borrowing and return transactions.
Track loan duration, due dates, and return status.
Manage book reservations and maintain availability records.
3. ER Diagram
The ER diagram consists of the following entities and relationships:
Entities and Attributes
User (UserID, Name, Email, Phone, Role)
Book (BookID, Title, ISBN, PublishYear, CategoryID)
Author (AuthorID, AuthorName)
Category (CategoryID, CategoryName)
Loan (LoanID, IssueDate, DueDate, ReturnDate, UserID, BookID)
Reservation (ReservationID, ReservationDate, UserID, BookID)
Book_Author (BookID, AuthorID)
Relationships
A User can have many Loans (1:M).
A Book can be loaned many times (1:M).
A User can make many Reservations (1:M).
A Book can be reserved many times (1:M).
A Book can be written by many Authors, and an Author can write many Books (M:N), resolved using the Book_Author junction table.
Each Book belongs to one Category, while a Category can include many Books (1:M).








4. Relational Schema
User ( UserID, Name, Email, Phone, Role )
Category ( CategoryID, CategoryName )
Author ( AuthorID, AuthorName )
Book ( BookID, Title, ISBN, PublishYear, CategoryID → Category.CategoryID )
Book_Author ( BookID → Book.BookID, AuthorID → Author.AuthorID )
Loan ( LoanID, IssueDate, DueDate, ReturnDate, UserID → User.UserID, BookID → Book.BookID )
Reservation ( ReservationID, ReservationDate, UserID → User.UserID, BookID → Book.BookID )
5. Normalization and Final Relational Schema
Rule of 1NF:

✔ Each attribute contains atomic (single) values
✔ No repeating groups or multivalued attributes

To satisfy 1NF, the data is divided into separate tables where each field stores a single value.

Tables created in 1NF:
User ( UserID, Name, Email, Phone, Role )
Book ( BookID, Title, ISBN, PublishYear, Category, Author )
Loan ( LoanID, IssueDate, DueDate, ReturnDate, UserID, BookID )
Reservation ( ReservationID, ReservationDate, UserID, BookID )
At this stage, all attributes contain atomic values, but redundancy still exists.

Rule of 2NF:

✔ Must be in 1NF
✔ No partial dependency (applies to composite primary keys)
The Book–Author relationship is many-to-many, so a separate junction table is created.

Tables created in 2NF:
User ( UserID, Name, Email, Phone, Role )
Category ( CategoryID, CategoryName )
Author ( AuthorID, AuthorName )
Book ( BookID, Title, ISBN, PublishYear, CategoryID )
Book_Author ( BookID, AuthorID )
Loan ( LoanID, IssueDate, DueDate, ReturnDate, UserID, BookID )
Reservation ( ReservationID, ReservationDate, UserID, BookID )
All non-key attributes are fully dependent on their respective primary keys.

Rule of 3NF:

✔ Must be in 2NF
✔ No transitive dependency
(Non-key attributes must not depend on other non-key attributes)

In the Book table:
BookID → CategoryName (repeating value)


So, Category is separated into its own table.
All remaining attributes depend only on their primary keys, and no transitive dependency exists.

User ( UserID, Name, Email, Phone, Role )
Category ( CategoryID, CategoryName )
Author ( AuthorID, AuthorName )
Book ( BookID, Title, ISBN, PublishYear, CategoryID → Category.CategoryID )
Book_Author ( BookID → Book.BookID, AuthorID → Author.AuthorID )
Loan ( LoanID, IssueDate, DueDate, ReturnDate, UserID → User.UserID, BookID → Book.BookID )
Reservation ( ReservationID, ReservationDate, UserID → User.UserID, BookID → Book.BookID )


6. Database Schema
1. user
Stores personal and contact information of library members.
UserID INT(11) (PK)
Name VARCHAR(100)
Email VARCHAR(100)
Phone VARCHAR(15)
Role VARCHAR(50)
2. category
Stores book classification information.
CategoryID INT(11) (PK)
CategoryName VARCHAR(100)
3. author
Stores information about book authors.
AuthorID INT(11) (PK)
AuthorName VARCHAR(100)
4. book
Stores detailed information about books available in the library.
BookID INT(11) (PK)
Title VARCHAR(200)
ISBN VARCHAR(20) (UNIQUE)
PublishYear YEAR
CategoryID INT(11) (FK) → References category(CategoryID)
5. book_author
Junction table used to manage the many-to-many relationship between books and authors.
BookID INT(11) (FK) → References book(BookID)
AuthorID INT(11) (FK) → References author(AuthorID)
 Composite Primary Key: (BookID, AuthorID)


6. loan
Stores information about book borrowing transactions.
LoanID INT(11) (PK)
IssueDate DATE
DueDate DATE
ReturnDate DATE
UserID INT(11) (FK) → References user(UserID)
BookID INT(11) (FK) → References book(BookID)
7. reservation
Stores book reservation records.
ReservationID INT(11) (PK)
ReservationDate DATE
UserID INT(11) (FK) → References user(UserID)
BookID INT(11) (FK) → References book(BookID)


7. Database Implementation (SQL Scripts)
CREATE DATABASE LibraryManagementSystem;


CREATE TABLE user (
    UserID INT(11) PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    Phone VARCHAR(15),
    Role VARCHAR(50)
);


CREATE TABLE category (
    CategoryID INT(11) PRIMARY KEY,
    CategoryName VARCHAR(100) NOT NULL
);


CREATE TABLE  author (
    AuthorID INT(11) PRIMARY KEY,
    AuthorName VARCHAR(100) NOT NULL
);


CREATE TABLE book (
    BookID INT(11) PRIMARY KEY,
    Title VARCHAR(200) NOT NULL,
    ISBN VARCHAR(20) UNIQUE,
    PublishYear YEAR,
    CategoryID INT(11),
    FOREIGN KEY (CategoryID) REFERENCES category(CategoryID)
);


CREATE TABLE book_author (
    BookID INT(11),
    AuthorID INT(11),
    PRIMARY KEY (BookID, AuthorID),
    FOREIGN KEY (BookID) REFERENCES book(BookID),
    FOREIGN KEY (AuthorID) REFERENCES author(AuthorID)
);


CREATE TABLE loan (
    LoanID INT(11) PRIMARY KEY,
    IssueDate DATE NOT NULL,
    DueDate DATE NOT NULL,
    ReturnDate DATE,
    UserID INT(11),
    BookID INT(11),
    FOREIGN KEY (UserID) REFERENCES user(UserID),
    FOREIGN KEY (BookID) REFERENCES book(BookID)
);


CREATE TABLE reservation (
    ReservationID INT(11) PRIMARY KEY,
    ReservationDate DATE NOT NULL,
    UserID INT(11),
    BookID INT(11),
    FOREIGN KEY (UserID) REFERENCES user(UserID),
    FOREIGN KEY (BookID) REFERENCES book(BookID)
);









8. SQL Queries
1. View all users.
SQL Command:

Output:




2. Display user name and email.
SQL Command:

Output:

3. View all books.
SQL Command:

Output:

4. Display book titles published after 2015.
SQL Command:

Output:

5. Books published after 2010 AND belonging to CategoryID = 1.
SQL Command:

Output:

6. Books from CategoryID 1 OR CategoryID 15.
SQL Command:

Output:

7. Books ordered by publish year (ascending).
SQL Command:

Output:


8. Books ordered by publish year (descending).
SQL Command:

Output:



9. Display distinct publish years.
SQL Command:

Output:

10. Total number of users.
SQL Command:

Output:

11. Total number of books.
SQL Command:

Output:

12. Average publish year of books.
SQL Command:

Output:

13. Oldest and newest book publish year.
SQL Command:

Output:

14. Number of books in each category
SQL Command:

Output:

15. Categories having more than one book
SQL Command:

Output:

16. Display books with their category names.
SQL Command:

Output:

17. Display loan details with user name.
SQL Command:

Output:

18. Display book title with author name.
SQL Command:

Output:

19. Users whose name starts with 'A'
SQL Command:

Output:

20. Books published between 2000 and 2020
SQL Command:

Output:

21. Books published after the average publish year.
SQL Command:

Output:

22. Users who borrowed at least one book.
SQL Command:

Output: 

23. Loans that are not yet returned.
SQL Command:

Output:

24. Users with reserved books.
SQL Command:

Output:

25. Count total reservations per book.
SQL Command:

Output:


9. Conclusion and Future Advancement
This project provided practical experience in designing and implementing a relational database system using the Library Management System as a case study. Through this project, key database concepts such as ER modeling, normalization up to Third Normal Form (3NF), and SQL implementation were successfully applied. Challenges encountered during the project included correctly defining entity relationships and maintaining referential integrity across multiple tables.
Future enhancements of the system may include the integration of an online library portal, real-time book availability tracking, automated fine calculation for overdue books, and the development of mobile or web-based applications to improve user accessibility and system efficiency.
10. Contribution of Each Team Member
Member 1: ER Diagram and Requirements Analysis
Member 2: SQL Implementation and Queries
Member 3: Database Schema and Normalization
