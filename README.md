<h2>The Problem Statement and Questions</h2>
<h3>a) Process Requirements Description</h3>
<p>We are a small company with around 20 employees and we would like to implement a benefits management process.
  Each employee would receive a virtual 300PLN per month, which they could use to pay for:</p>
<ul>
  <li>medical care for themselves (80 PLN)</li>
  <li>medical care for a family member (120 PLN)</li>
  <li>sports package (100 PLN)</li>
  <li>meal package (220 PLN)</li>
  <li>IT training (100 PLN) - only for IT employees</li>
</ul>
<p>Unused money should carry over to the next month.
  The process should be very simple. After the circulation starts for the user specified by the administrator,
  the employee should receive 300 PLN to start and be able to add the selected service.
  We can assume that the employee will have to specify which services they want to use in the next month by the end of the current month.</p>

<h3>b) Issues and questions for development</h3>
<ol>
  <li>Based on Chapter 1 Process Requirements Description, please identify business processes, actors, and data sets.
    Please use any modeling tool to draw the identified processes with as much detail as possible.</li>
  <li>How can we control the employee to fill out the list of services by the end of the month?
    Please propose a specific solution from the user's perspective.</li>
  <li>How can we automate the process of selecting services for an employee each month?
    Please propose a specific solution from the user's perspective.</li>
  <li>An additional requirement has been reported: a new service has appeared - 'Training' - this service does not have
    a fixed price like others. When selecting it, the employee should enter the price themselves. If the price is
    greater than 1000 PLN, it requires the approval of 2 people: the Manager and the CIO.
    Please propose a solution from the user's perspective.</li>
  <li>SQL - Please use the proposed process schema to model the relational tables with columns that include all
    requirements.</li>
  <li>SQL - Please use the proposed tables to outline a SQL query that will return IT department employees' services
    (service name, service price, employee) from April 2022.</li>
  <li>SQL - Please schematically outline an SQL query to the database that will return employees along with their
    current benefit account status.</li>
</ol>


<h2>Problem Solution</h2>

<ol>
<li> 
Based on Chapter 1 Process Requirements Description, please identify business processes, actors, and data sets.
    Please use any modeling tool to draw the identified processes with as much detail as possible
<h3>a) Business Processes:</h3>
<ul>
  <li>Benefits Allocation: Monthly allocation of the virtual 300 PLN to each employee.</li>
  <li>Benefits Selection: Employees specify which services they want to use in the next month.</li>
  <li>Benefits Rollover: Unused money carries over to the next month.</li>
  <li>Benefits Administration: Managing the overall benefits process, including setting up employees and monitoring usage.</li>
</ul>

<h3>b) Actors:</h3>
<ul>
  <li>Employee: Chooses and uses the benefits.</li>
  <li>Administrator: Sets up and monitors the benefits process.</li>
</ul>

<h3>c) Data Sets:</h3>
<ul>
  <li>Employee Information: Contains employee details such as name, employee ID, and department.</li>
  <li>Benefits Catalog: Lists the available services and their costs.</li>
  <li>Employee Benefits Balance: Tracks the current balance of each employee's virtual money.</li>
  <li>Employee Benefits Usage: Records the selected services by employees and their usage history.</li>
</ul>

<h3>Here is a detailed process flow:</h3>
<h4>Benefits Allocation:</h4>
<ul>
  <li>Trigger: End of the month.</li>
  <li>Actor: Administrator.</li>
  <li>Process: Allocate 300 PLN to each employee's benefits balance.</li>
</ul>

<h4>Benefits Selection:</h4>
<ul>
  <li>Trigger: Employee decision.</li>
  <li>Actor: Employee.</li>
  <li>Process:</li>
  <ol>
    <li>Employee reviews the available services and their costs in the Benefits Catalog.</li>
    <li>Employee selects the desired services for the next month.</li>
    <li>Employee submits the selection before the end of the current month.</li>
  </ol>
</ul>

<h4>Benefits Rollover:</h4>
<ul>
  <li>Trigger: End of the month.</li>
  <li>Actor: Administrator.</li>
  <li>Process: Any unused money from an employee's balance is carried over to the next month.</li>
</ul>

<h4>Benefits Administration:</h4>
<ul>
  <li>Trigger: Setup, monitoring, and management of benefits.</li>
  <li>Actor: Administrator.</li>
  <li>Process:</li>
  <ol>
    <li>Set up employee information in the system.</li>
    <li>Manage the Benefits Catalog.</li>
    <li>Monitor employee benefits usage and balances.</li>
  </ol>
</ul>

<hr>

Sequence UML:

<br>

<pre>
<code>@startuml
!theme blueprint

actor "Administrator" as A
actor "Employee" as E
actor "Manager" as M
actor "CIO" as C
database "Employee Information" as EI
database "Benefits Catalog" as BC
database "Employee Benefits Balance" as EBB
database "Employee Benefits Usage" as EBU

== Benefits Allocation ==
A -> EI : Setup Employee Information
A -> BC : Manage Benefits Catalog

loop for each employee
A -> EBB : Allocate 300 PLN monthly
end

== Benefits Selection ==
E -> BC : Review available services and costs
E -> EBU : Select desired services for the next month

opt Training > 1000 PLN
  EBU -> M : Request approval
  M -> EBU : Approve / Reject Training
  EBU -> C : Request approval
  C -> EBU : Approve / Reject Training
end

E -> EBU : Submit selection before the end of the month

EBU -> EBB : Update Employee Benefits Balance

== Benefits Administration ==
A -> EBB : Monitor Employee Benefits Balance
A -> EBU : Monitor Employee Benefits Usage

== Benefits Rollover ==
loop for each employee with unused balance
A -> EBB : Benefits Rollover at the end of the month
end

@enduml</code>
</pre>

<p><img src="https://www.plantuml.com/plantuml/png/ZPD1Rzim38Nl-XLllG_Bi5k158s3708isz0kFu2scLY3B7aIxQx_Ve9iHqlX3Bq42VtnlKU6VWn2NlhMg0_ISCieJS-TrrQKeagSnzscRhNLGJp5dtUWWCtyQDTsnhqof-fkhXx8qfb7z30Nj_llgR1LDGcL53YtOszFphSasjbHQlyFLR3bavRO6al6dHDHBEfSq88CsMhALHJ_PO-1pZDvL6gzpeysnhWgnS9whJRu-12ZuHxFB7s7fVWZL6mZDAu1R-ChPPph43l0L3NWIMMIBmhSpxwCBwiLVdpzXjPPQSohObjV9tncmzKSS7VnVk91ymje86seD8p0Vj0L1v2jKRaWOQJZo14VLL1pq9xh2vn2IiEm_4V634gvJl3JaxRQdl60pwineaBIYfA7vF-xvo2WhlDk8AE0GxRRf2hZK-H-SPNriiXsMMJx3f6qfAlf-hBLWd1PLSbnC4wJHHRkbBvEyyMU_4SSkvg4SVF0tfnBVbFcaybI1sUrE7zRQuQFj-1yhTUsJyuODx2FXZTk2ozQ6lIs3roZp7PJjWKfaiNrfDq-ne--DNy1" alt="Sequence diagram"></p>

<hr>

Activity UML:

<br>

<pre>
<code>@startuml
!theme blueprint

start

:Administrator setup Employee Information;
:Administrator manage Benefits Catalog;
:Administrator allocate 300 PLN monthly to Employee Benefits Balance;

:Employee reviews available services in Benefits Catalog;
:Employee selects desired services for the next month;

if (Employee selects Training > 1000 PLN) then (yes)
    :Employee submits custom price;
    :Request approval from Manager;
    if (Manager approves) then (yes)
        :Request approval from CIO;
        if (CIO approves) then (yes)
            :Add Training to Employee Benefits Usage;
        else (no)
            :Reject Training;
        endif
    else (no)
        :Reject Training;
    endif
else (no)
    :Employee submits selection before the end of the month;
    :Update Employee Benefits Usage;
    :Update Employee Benefits Balance;
endif

:Administrator monitor Employee Benefits Usage;
:Administrator monitor Employee Benefits Balance;
:Administrator perform Benefits Rollover at the end of the month;

stop

@enduml</code></pre>
<p><img src="https://www.plantuml.com/plantuml/png/VLFBZXCn4BpxAvfRzXR4ZPNGiYiEA_504Vi1dPcQnCXs6xjd87-FPnqcKKpYoupSLTLLRliXAoSTlJDlT0yFsheHCTcWnhoMZ7d_iVSssAo9LH9bw1ZfauzE3W0zXq6IPxKIxY-XdWFlG8y86AncUc9b9xi6nyv9nmfwjrxJzyzVoKlGlJkGogdHelB8ZaE7--9iAIPC5hypySJMyTQ1Cj9aEsIoeJLmninmw4gfHxO9_ObNOf7kGG5_j1eg7Ur0guRw8t494tRqWTwkQuAxcHfeTK2-CrJMMSDnwsSltPXLFCLaIvQAsU3NYAp4CIQPsD6G2kBBwoccYfaD73UECEIcsGsnf-TlLUYVMDcu9LHNcLU_nFp_L5uoxt2IXikWLP0BdGr-ejD5wWmVUZkObdcTLHaB-iefrnbP2RJ580bpmfbACio_ny5MyaliMN4xMmDghsKrrhm52RPyhwhVnhUDMaP4cf_Y2RWHvsH28jOhiKrMYSOybChetLy0" alt="Activity diagram"></p>
<hr>

Class UML:

<br>

<pre>
<code>@startuml

!theme blueprint

class Employee {
  +employee_id: Integer
  +name: String
  +department: String
}

class Benefit {
  +benefit_id: Integer
  +benefit_name: String
  +benefit_price: Integer
  +is_for_IT_only: Boolean
}

class EmployeeBenefitsBalance {
  +balance_id: Integer
  +employee_id: Integer
  +balance: Integer
}

class EmployeeBenefitsUsage {
  +usage_id: Integer
  +employee_id: Integer
  +benefit_id: Integer
  +usage_date: Date
  +custom_price: Integer
  +approved_by_manager: Boolean
  +approved_by_CIO: Boolean
}

class Administrator {
}

class Manager {
}

class CIO {
}

Employee -- EmployeeBenefitsBalance: "1" -- "1"
Employee -- EmployeeBenefitsUsage: "1" -- "0..*"
Benefit -- EmployeeBenefitsUsage: "1" -- "0..*"
Administrator -- Employee: "1" -- "0..*"
Manager -- Employee: "1" -- "0..*"
CIO -- Employee: "1" -- "0..*"

note left of Employee: "Employee Information"
note right of Benefit: "Benefits Catalog"
note bottom of EmployeeBenefitsBalance: "Employee Benefits Balance"
note top of EmployeeBenefitsUsage: "Employee Benefits Usage"

note "Benefits Allocation" as N1
note "Benefits Selection" as N2
note "Benefits Rollover" as N3
note "Benefits Administration" as N4

Administrator .. N1
EmployeeBenefitsBalance .. N1

Employee .. N2
Benefit .. N2

Administrator .. N3
EmployeeBenefitsBalance .. N3

Administrator .. N4

@enduml
</code></pre>

<p><img src="https://www.plantuml.com/plantuml/png/ZL6nRjim4Dtv5Qp75Z6QE9Edn6a73ss1ffs5itHI190yWJm5C8B-UwcGD1r2CZpPT-_TUtnF3ux1ygYL-CGlg54Ur8Y3xGqBKIjmJdxNWw8ZedmJKdx1E5LzivLxmzYXdVO6D6xbC_lBRfeR7BokHiEdxR-ak4E3RSz1y126KY-jIzsqzq-iySD5xwgMRBN_Kv5HnwtS4Ia4asnpa6ZlTg30r1YV4ORo6RDnCptl5bt-EkYYn-Z_NkymtqlGQ82zzpTWd7Rrw9ZqJ2Km39PUiQaEnqg30R_ElHJuq_xNJ6UFZUvDxzW2avLl6VWHvCvNNY6CgG9vSxDK-bQkRbSJxd-M-Bc-pFwwNdzUYGXUpV-Gulog98PK5oa-vILO66AK2bkMr9wpJY7tfYMhWNiogy2sVVVoJeyXF3l5aK_0eAYBp0Cna_RKvLwpqKaZOb63QIW4Sd6pv-z8IfPVzgWKrI6612T_tfQ4PrHOP_okn7-JLtX56-1D2UUFbZNkHV4jr-l9UA6CY2Oup7SYii8q9xYvABYPExaN3sYQKQl_" alt="Class diagram"></p>
</li>

<hr>

<li>

How can we control the employee to fill out the list of services by the end of the month? Please propose a specific solution from the user's perspective.

<h2>Solution for Employee Benefit Selection Control</h2>

<p>To ensure employees fill out the list of services by the end of the month, you can implement a notification and reminder system from the user's perspective:</p>

<h3>Set up a notification system:</h3>
<ul>
  <li>Create an automated email or in-app notification system that sends reminders to employees about the deadline for selecting benefits for the next month. These notifications can be sent periodically, for example, 15 days, 7 days, and 1 day before the deadline.</li>
</ul>

<h3>Implement a dashboard:</h3>
<ul>
  <li>Create a user-friendly dashboard for employees where they can easily access and review the available services, their costs, and their current benefits balance. The dashboard should display a clear and prominent reminder about the deadline for selecting benefits.</li>
</ul>

<h3>Create a user-friendly interface:</h3>
<ul>
  <li>Design an easy-to-use interface for employees to select desired services, submit their choices, and track their benefits usage. The interface should show a clear warning if the employee tries to submit their selections past the deadline.</li>
</ul>

<h3>Restrict access to benefits:</h3>
<ul>
  <li>Employees who fail to select their desired services by the end of the month should be restricted from accessing the benefits until they make their selections. This can be enforced by disabling access to the benefits in the system until the employee submits their choices.</li>
</ul>

<h3>Manager intervention:</h3>
<ul>
  <li>If an employee consistently fails to submit their benefits selections on time, their manager should be notified and intervene to address the issue.</li>
</ul>

</li>

<hr>

<li>

How can we automate the process of selecting services for an employee each month? Please propose a specific solution from the user's perspective.

<h2>Automating the Process of Selecting Services for an Employee</h2>

<p>One way to automate the process of selecting services for an employee each month is by implementing a user-friendly web or mobile application. From the user's perspective, the solution would work as follows:</p>

<ol>
  <li>Employee logs in to the application using their credentials.</li>
  <li>The application displays the available services in the Benefits Catalog along with their costs, and highlights any services with specific restrictions (e.g., IT training for IT employees only).</li>
  <li>The application allows the employee to select the desired services for the next month and provides a summary of the total cost.</li>
  <li>The employee can save their selection as a "default" or "favorite" package, which will be automatically applied each month unless they manually change it.</li>
  <li>The application sends reminders to the employee towards the end of the month, prompting them to review and update their selection if needed.</li>
  <li>If the employee selects the 'Training' service with a custom price greater than 1000 PLN, the application automatically sends a request for approval to both the Manager and the CIO.</li>
</ol>

<hr>

<li>

An additional requirement has been reported: a new service has appeared - 'Training' - this service does not have a fixed price like others. When selecting it, the employee should enter the price themselves. If the price is greater than 1000 PLN, it requires the approval of 2 people: the Manager and the CIO. Please propose a solution from the user's perspective.

<h2>Solution for the 'Training' Service Requirement</h2>

<p>The solution is covered in the responses to the previous issues, including a visual. Here is a brief summary:</p>
<ol>
  <li>The employee selects 'Training' from the benefits catalogue.</li>
  <li>Enters the price of the training in the designated field.</li>
  <li>If the price exceeds 1,000 PLN, the application automatically sends an approval request to the Manager and CIO.</li>
  <li>The employee receives a notification that the request has been approved or rejected.</li>
  <li>Once approved, the employee can use the selected training.</li>
</ol>

</li>

<hr>

<li>

SQL - Please use the proposed process schema to model the relational tables with columns that include all requirements.

<pre>
<code>
CREATE TABLE Employee (
  employee_id INTEGER PRIMARY KEY,
  name VARCHAR,
  department VARCHAR
);

CREATE TABLE Benefit (
  benefit_id INTEGER PRIMARY KEY,
  benefit_name VARCHAR,
  benefit_price INTEGER,
  is_for_IT_only BOOLEAN
);

CREATE TABLE Employee_Benefits_Balance (
  balance_id INTEGER PRIMARY KEY,
  employee_id INTEGER,
  balance INTEGER,
  FOREIGN KEY (employee_id) REFERENCES Employee(employee_id)
);

CREATE TABLE Employee_Benefits_Usage (
  usage_id INTEGER PRIMARY KEY,
  employee_id INTEGER,
  benefit_id INTEGER,
  usage_date DATE,
  custom_price INTEGER NULL,
  approved_by_manager BOOLEAN NULL,
  approved_by_CIO BOOLEAN NULL,
  FOREIGN KEY (employee_id) REFERENCES Employee(employee_id),
  FOREIGN KEY (benefit_id) REFERENCES Benefit(benefit_id)
);


INSERT INTO Employee (employee_id, name, department)
VALUES (1, 'Alice', 'IT'),
       (2, 'Bob', 'HR'),
       (3, 'Carol', 'Finance'),
       (4, 'Dave', 'IT'),
       (5, 'Eve', 'Sales'),
       (6, 'Frank', 'Finance'),
       (7, 'Grace', 'IT'),
       (8, 'Harry', 'Sales'),
       (9, 'Isaac', 'HR'),
       (10, 'Jane', 'IT'),
       (11, 'Kevin', 'Finance'),
       (12, 'Lucy', 'Sales'),
       (13, 'Mike', 'IT'),
       (14, 'Nancy', 'HR'),
       (15, 'Olivia', 'Finance'),
       (16, 'Paul', 'Sales'),
       (17, 'Quinn', 'IT'),
       (18, 'Rachel', 'HR'),
       (19, 'Sam', 'Finance'),
       (20, 'Tina', 'Sales');

INSERT INTO Benefit (benefit_id, benefit_name, benefit_price, is_for_IT_only)
VALUES (1, 'Medical Care for Themselves', 80, false),
       (2, 'Medical Care for a Family Member', 120, false),
       (3, 'Sports Package', 100, false),
       (4, 'Meal Package', 220, false),
       (5, 'IT Training', 100, true),
       (6, 'Training', null, false);


INSERT INTO Employee_Benefits_Balance (balance_id, employee_id, balance)
VALUES (1, 1, 280),
       (2, 2, 0),
       (3, 3, 300),
       (4, 4, 860),
       (5, 5, 300),
       (6, 6, 60),
       (7, 7, 300),
       (8, 8, 520),
       (9, 9, 0),
       (10, 10, 300),
       (11, 11, 620),
       (12, 12, 300),
       (13, 13, 320),
       (14, 14, 0),
       (15, 15, 300),
       (16, 16, 160),
       (17, 17, 0),
       (18, 18, 1600),
       (19, 19, 180),
       (20, 20, 100);


INSERT INTO Employee_Benefits_Usage (usage_id, employee_id, benefit_id, usage_date, custom_price, approved_by_manager, approved_by_CIO)
VALUES
   (1, 2, 4, '2022-01-05', null, null, null),
    (2, 8, 2, '2022-01-08', null, null, null),
    (3, 3, 1, '2022-01-12', null, null, null),
    (4, 7, 5, '2022-01-15', 800, null, null),
    (5, 12, 4, '2022-01-17', null, null, null),
    (6, 14, 1, '2022-01-20', null, null, null),
    (7, 9, 3, '2022-01-22', null, null, null),
    (8, 17, 5, '2022-01-25', null, null, null),
    (9, 1, 5, '2022-02-05', null, null, null),
    (10, 10, 2, '2022-02-08', null, null, null),
    (11, 4, 4, '2022-02-10', null, null, null),
    (12, 5, 3, '2022-02-11', null, null, null),
    (13, 7, 1, '2022-02-15', null, null, null),
    (14, 16, 4, '2022-02-17', null, null, null),
    (15, 10, 5, '2022-02-20', null, null, null),
    (16, 15, 6, '2022-02-22', 1200, true, true),
    (17, 19, 1, '2022-02-25', null, null, null),
    (18, 1, 4, '2022-03-05', null, null, null),
    (19, 4, 3, '2022-03-10', null, null, null),
    (20, 1, 5, '2022-03-11', null, null, null),
    (21, 7, 2, '2022-03-15', null, null, null),
    (22, 18, 4, '2022-03-17', null, null, null),
    (24, 20, 6, '2022-03-24', 1300, true, true),
    (25, 1, 5, '2022-04-05', null, null, null),
    (26, 10, 4, '2022-04-05', null, null, null),
    (27, 4, 1, '2022-04-11', null, null, null),
    (28, 17, 2, '2022-04-14', null, null, null),
    (29, 4, 5, '2022-04-22', null, null, null),
    (30, 10, 5, '2022-04-25', null, null, null),
    (31, 2, 3, '2022-05-05', null, null, null),
    (32, 9, 4, '2022-05-08', null, null, null),
    (33, 4, 5, '2022-05-10', null, null, null),
    (34, 6, 1, '2022-05-25', null, null, null),
    (22, 18, 4, '2022-03-17', null, null, null),
    (24, 20, 6, '2022-03-24', 1300, true, true),
    (25, 1, 5, '2022-04-05', null, null, null),
    (26, 10, 4, '2022-04-05', null, null, null),
    (27, 4, 1, '2022-04-11', null, null, null),
    (28, 17, 2, '2022-04-14', null, null, null),
    (29, 4, 5, '2022-04-22', null, null, null),
    (30, 10, 5, '2022-04-25', null, null, null),
    (31, 2, 3, '2022-05-05', null, null, null),
    (32, 9, 4, '2022-05-08', null, null, null),
    (33, 4, 5, '2022-05-10', null, null, null),
    (34, 6, 1, '2022-05-11', null, null, null),
    (35, 8, 6, '2022-05-15', 900, null, null),
    (36, 17, 4, '2022-05-25', null, null, null),
    (37, 1, 6, '2022-06-05', 1800, true, true),
    (38, 5, 1, '2022-06-11', null, null, null),
    (39, 7, 3, '2022-06-15', null, null, null),
    (40, 18, 2, '2022-06-17', null, null, null),
    (41, 19, 3, '2022-06-25', null, null, null),
    (42, 2, 2, '2022-07-05', null, null, null),
    (43, 10, 5, '2022-07-08', null, null, null),
    (44, 3, 4, '2022-07-10', null, null, null),
    (45, 15, 2, '2022-07-17', null, null, null),
    (46, 11, 4, '2022-07-20', null, null, null),
    (47, 14, 6, '2022-07-22', 1500, true, true),
    (48, 17, 3, '2022-07-25', null, null, null);</code></pre>

</li>

<hr>

<li>

<p>SQL - Please use the proposed tables to outline a SQL query that will return IT department employees' services (service name, service price, employee) from April 2022.</p>

<pre><code>SELECT E.name AS employee_name, B.benefit_name AS service_name,
       COALESCE(EBU.custom_price, B.benefit_price) AS service_price
FROM Employee E
JOIN Employee_Benefits_Usage EBU ON E.employee_id = EBU.employee_id
JOIN Benefit B ON EBU.benefit_id = B.benefit_id
WHERE E.department = 'IT'
AND EBU.usage_date >= '2022-04-01'
AND EBU.usage_date &lt;= '2022-04-30';
</code></pre>

<p>The query starts by selecting the employee name, benefit name, and service price.
The Employee table is joined with the Employee_Benefits_Usage table using the employee_id field to connect employees with their benefits usage.
The Employee_Benefits_Usage table is joined with the Benefit table using the benefit_id field to connect benefits usage with the corresponding benefit details.
The WHERE clause filters the results to include only IT department employees and benefits usage within April 2022.
The COALESCE function is used to display the custom price (for the 'Training' service) if it exists, and if not, it shows the standard benefit price.</p>

<p><img src="https://www.plantuml.com/plantuml/png/TP5FR_8m38Vl-HH-JmNfHUhOJd1XVpPfkuwTKMXSY9GuLUoEyUsNjZGROlXKktaU9J-lM7IKrZlrJ_Re4RQknIPQ4jN_KMhnZWuh0VID2qV43McF_u4nVjfgx3IPF4bqXSi4lWLBy7p2u61bdxlD0nBMLdZpmNg7Aj2FtveCNpa-k4wpZjqEdmDgQvYC85Y6v7bTWy56H_58CWNP8y7aY3nLaAe31g_HqYDRb0CYGJ4lv_DPUJShRuh-ZNbmMnONdj7rsB84F-G0-6LP6EeGeLX7RSdIxWBDjNYot3XzXEgteuD7TenvcJbqZ32XSCN4eideQ798JJp7oTXQ_IrQl5Wd626q0X6h40q31UaYlKyQ1rqSIPKrPughU1s3Fz_9KWsyojUkUnHD0hbrmccnBA5HAoJJUlSD" alt="Activity diagram of the SQL query"></p>

<p>The diagram starts by selecting employee_name, service_name, and service_price.
Employee table is joined with the Employee_Benefits_Usage table using the employee_id field.
Employee_Benefits_Usage table is joined with the Benefit table using the benefit_id field.
The diagram checks if the employee is in the IT department. If yes, it proceeds to the next condition; if not, it filters out records not from the IT department.
The diagram checks if the usage date is between '2022-04-01' and '2022-04-30'. If yes, it proceeds to the next condition; if not, it filters out records not in April 2022.
The diagram checks if a custom_price exists for the 'Training' service. If yes, it displays the custom_price; if not, it displays the standard benefit_price.
The filtered results are displayed.</p>

</li>

<hr>

<li>

<p>SQL - Please schematically outline an SQL query to the database that will return employees along with their current benefit account status.</p>

<pre><code>SELECT e.employee_id, e.name, e.department, eb.balance
FROM Employee e
JOIN Employee_Benefits_Balance eb ON e.employee_id = eb.employee_id;</code></pre>
<p>This query selects the employee_id, name, and department columns from the Employee table (aliased as "e") and the balance column from the Employee_Benefits_Balance table (aliased as "eb"). It then joins these two tables using the employee_id as the common key. The result will be a list of employees with their respective benefit account balances.</p>
<img src="https://www.plantuml.com/plantuml/png/ZP6nRa8n34NtV8MxmI_0Wf1OEZ9qGa9-UJV5Kfv3OuUMls-UeArGXpQB5TrxZcHvOyxLsvJ3avymWORSSAwY7i8SzR7Q8q8keBU6UgMAFitNFNZXB2FxxqGIBUwX52LhCS8ijRmaFq5fSOKj0_MtsdruvUWTphYmEdM63vfgcMWtdNEv0kGyPAp_3notK2HnEsuviyP7mtCH_N6pZWVz0xI35YLybMIyc_RmLfMoc5D9tuLUVnS_pRjAfU6c98wnD7KQRcPR1sG3BRGi7soejLIQOCQlw2KT9St7AESGDltNZ_K9">


</li>

</ol>
