# 🏫 School Facilitator Booking & Scheduling System  
### Functional Description (by User Type)

---

## 📚 **Table of Contents**

1. [👩‍🎓 Student](#student)  
2. [👩‍🏫 Facilitator](#facilitator)  
3. [👩‍💼 Secretary](#secretary)  
4. [👩‍💻 Administrator](#administrator)  
5. [💡 System-Wide Features](#system-wide-features)  
6. [🧭 Example Scenario](#example-scenario)  
7. [📊 Reporting and Statistics](#reporting-and-statistics)  

---

<a id="student"></a>
## 👩‍🎓 **Student**

Students use the system to find academic help sessions offered by facilitators for specific courses.

### What students can do

* **Log in using college credentials (LDAP)**.  
  During development, they can use a simple test login (username = password).

* **View available courses** that offer facilitator support.  
  Each course lists the facilitators who teach or support it.

* **Book a session** by:
  1. Choosing the course where they need help.  
  2. Selecting a facilitator who teaches that course.  
  3. Choosing an available date and time slot from the facilitator’s calendar.  
  4. Writing a **mandatory comment** describing what they need help with (e.g., “review assignment 2”, “clarify SQL joins”).  
  5. Confirming the booking.

* **Booking rules for students**  
  * A student can book **only one hour per day**.  
  * If a student needs more time, they must submit a **multi-session request** to the secretary.  
  * Bookings are automatically prevented on official holidays or days the facilitator is absent.

* **Cancel their own bookings**, as long as the session hasn’t started.

* **View their current and past appointments** in a personal calendar or list view.

* **Receive email notifications** when:  
  * Their booking is confirmed.  
  * A session is cancelled by the facilitator or secretary.  
  * Their multi-session request is approved or rejected.

---

<a id="facilitator"></a>
## 👩‍🏫 **Facilitator**

Facilitators are the staff members who provide academic support to students.

### What facilitators can do

* **Log in with their college credentials (LDAP)** or in dev mode using the test login.  
* **View their assigned courses** and upcoming sessions in a personal calendar.  
* **Define their weekly availability** for each semester.  
  They can choose which days and times they are available to accept student bookings.  
* **Start and end their work shifts.**  
  When a shift starts, the system records the start time; when it ends, it records the duration.  
* **View booked sessions** for the day or week and the **student’s help request comment**.  
* **Cancel appointments**:  
  * Cancel a single session (e.g., if the student doesn’t show or for personal reasons).  
  * Cancel all sessions for a specific day (e.g., illness or absence).  
  * Each cancellation automatically frees the slot and sends an email to all affected students explaining the reason.  
* **Report an absence** (illness, meeting, personal leave).  
  When an absence is logged, all affected appointments are cancelled automatically.  
* **Receive email notifications** for new bookings and cancellations (optional).

---

<a id="secretary"></a>
## 👩‍💼 **Secretary**

The secretary manages daily operations, ensuring smooth scheduling for all facilitators and students.

### What the secretary can do

* **Log in securely** with their college account.  
* **View and manage the global calendar**, seeing all facilitators’ appointments at once.  
  They can filter by facilitator, course, or date.  
* **Create, modify, or cancel bookings** on behalf of students.  
* **Book slots for internal meetings or administrative tasks** (e.g., staff training).  
  These are marked as “Office” or “Meeting” and are unavailable for student booking.  
* **Manage facilitator absences**.  
  When a facilitator is absent, the secretary can mark the absence and automatically cancel all affected appointments. Students are notified by email.  
* **Review and approve multi-session requests** from students.  
  * Approved requests temporarily override the “one hour per day” limit.  
  * Rejected requests notify the student with a message.  
* **View all student bookings** and search by student name, course, or facilitator.  
* **Receive automatic notifications** when new users sign up via LDAP, prompting them to assign roles or confirm details.

---

<a id="administrator"></a>
## 👩‍💻 **Administrator**

The administrator configures the system, manages users and semesters, and ensures proper data structure.

### What the administrator can do

* **Create and manage user accounts**.  
  * Add new users manually or edit those auto-created through LDAP login.  
  * Change roles between *Student, Facilitator, Secretary,* and *Admin*.  
  * Activate or deactivate users.  

* **Manage semesters**.  
  * Define the start and end dates for each semester.  
  * Only one semester is active at a time for slot generation.  

* **Define official holidays and non-working days**.  
  These are stored in the system calendar and automatically excluded when facilitator slots are generated.  

* **Oversee all facilitator schedules** and slot generation logic.  
  * Run the “Generate Slots” procedure at the start of each semester.  
  * Ensure holidays and absences are respected.  

* **Monitor activity and logs**.  
  * View all cancellations, absences, and multi-session approvals.  
  * Track which user made each change (audit trail).  

* **Set up and test the email system** and SMTP connection for notifications.  

* **Switch the system between development and production modes** if necessary.  

---

<a id="system-wide-features"></a>
## 💡 **System-Wide Features**

* **Automatic email notifications**  
  * Sent through internal mail server (`172.20.1.169:587`).  
  * Used for: new user creation, cancellations, approvals, and absences.  

* **LDAP + Dev mode authentication**  
  * Production: login through Active Directory (`acgadmindc1.acgadmin.edu`).  
  * Development: users log in using username = password or “apextest”.  

* **Audit trail**  
  * Every booking, cancellation, or absence is logged with timestamps and responsible user.  

* **Role-based APEX pages**  
  * Students see only their sessions.  
  * Facilitators see only their own calendar.  
  * Secretaries see all calendars.  
  * Admins see configuration and reports.  

* **Color-coded calendar**  
  * Green = available slot  
  * Blue = booked session  
  * Gray = meeting or office work  
  * Red = cancelled/absent  
  * Yellow = pending approval  

* **System automatically prevents scheduling conflicts**  
  * Holidays and absences block slot creation.  
  * Only one student can book a slot.  
  * Bookings outside availability hours are rejected.  

---

<a id="example-scenario"></a>
## 🧭 **Example Scenario**

1. **Student Login**  
   A student logs in, selects “Database Systems,” and sees available facilitators.  
   They pick “Dr. Smith” for Tuesday 14:00, add a comment “help with ER diagrams,” and book the slot.

2. **Facilitator View**  
   Dr. Smith logs in and sees the booking appear in their weekly calendar.

3. **Facilitator Illness**  
   The next day, Dr. Smith reports an absence.  
   The system cancels all Tuesday sessions, logs the absence, and emails affected students.

4. **Secretary View**  
   The secretary receives an alert and can verify cancellations in the global calendar.

5. **Admin Setup**  
   Before next semester, the admin updates dates, adds holidays, and runs slot generation again.

---

<a id="reporting-and-statistics"></a>
## 📊 **Reporting and Statistics**

At the end of each semester, the system provides reports and dashboards to help evaluate performance.

### 1. General Usage Statistics
* Total number of appointments created, completed, and cancelled.  
* Number of active students and facilitators.  
* Average sessions per student and per facilitator.  
* Session distribution by course.  
* Session load by weekday and time slot.

### 2. Facilitator Performance Overview
* Scheduled sessions, attendance rate, and total hours.  
* Number of distinct students assisted.  
* Courses supported.  
* Self-view statistics for facilitators.

### 3. Student Engagement Reports
* Number of unique students using the service.  
* Booking frequency.  
* Multi-session request rates.  
* Cancellations and no-shows.  
* Most in-demand courses.

### 4. Operational Reports for Secretaries
* Daily/weekly summaries of facilitator schedules.  
* Pending multi-session requests.  
* Absences and cancellations by reason.  
* Export of daily logs to Excel or PDF.

### 5. Administrative Reports
* Semester summary: holidays, generated slots, booked slots, utilization rate.  
* Cancellation breakdown (facilitator vs. secretary).  
* Cross-semester utilization comparison.  
* Email delivery report.

### 6. Visualization and Export
* Reports available in APEX “Reports & Analytics”.  
* Export to **Excel** or **PDF**.  
* Charts (bar, line, pie) for trends and comparisons.  
* Calendar color coding applies in charts.

---

### Example Summary Dashboard Metrics

| Metric | Description |
|--------|-------------|
| **Total Appointments Booked** | Number of sessions created in the semester. |
| **Average Booking per Student** | Total bookings ÷ active students. |
| **Facilitator Utilization** | Hours booked ÷ available hours. |
| **Cancellation Rate** | Cancelled ÷ total booked sessions. |
| **Most Active Course** | Course with most bookings. |
| **Most Active Facilitator** | Facilitator with most completed sessions. |
