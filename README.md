# Bus Ticket Booking - Complete GitHub Team Collaboration Guide

> **5 Members | 1 Admin (Owner) + 4 Contributors | Branch Strategy | Merge Conflicts | Cloud Deployment**

---

## Table of Contents

1. [GitHub Repository Setup (Admin)](#1-github-repository-setup-admin)
2. [Adding Team Members as Collaborators](#2-adding-team-members-as-collaborators)
3. [Branch Strategy](#3-branch-strategy)
4. [Each Member's First-Time Setup](#4-each-members-first-time-setup)
5. [Daily Workflow - How to Push & Pull](#5-daily-workflow---how-to-push--pull)
6. [Pull Requests - The Right Way to Merge](#6-pull-requests---the-right-way-to-merge)
7. [Merge Conflicts - How to Solve Step by Step](#7-merge-conflicts---how-to-solve-step-by-step)
8. [MySQL Table-Level Access Control](#8-mysql-table-level-access-control)
9. [Project Folder Structure for Team](#9-project-folder-structure-for-team)
10. [Branch Protection Rules](#10-branch-protection-rules)
11. [Real Scenarios - Who Does What](#11-real-scenarios---who-does-what)
12. [Cloud Deployment](#12-cloud-deployment)
13. [Quick Reference Commands](#13-quick-reference-commands)

---

## 1. GitHub Repository Setup (Admin Does This)

### Step 1.1: Create the Repository on GitHub

1. Go to **github.com** -> Sign in as **sarthakpawar0912** (Admin)
2. Click the **+** icon (top right) -> **New repository**
3. Fill in:
   ```
   Repository name:  bus-ticket-booking-system
   Description:      Bus Ticket Booking System - Spring Boot + Thymeleaf
   Visibility:       Private (keep it private during development)
   Initialize:       CHECK "Add a README file"
   .gitignore:       Select "Java"
   License:          MIT License (or None)
   ```
4. Click **Create repository**

Your repo URL will be: `https://github.com/sarthakpawar0912/bus-ticket-booking-system`

### Step 1.2: Clone It to Your Local Machine

```bash
# Open terminal / Git Bash
cd C:/Users/Sarthak/OneDrive/Desktop

git clone https://github.com/sarthakpawar0912/bus-ticket-booking-system.git

cd bus-ticket-booking-system
```

### Step 1.3: Set Up the Initial .gitignore

```bash
# This is CRITICAL - prevents secrets and build files from being pushed
```

Edit `.gitignore` to include:

```gitignore
# === Java / Spring Boot ===
target/
!.mvn/wrapper/maven-wrapper.jar
!**/src/main/**/target/
!**/src/test/**/target/

# === IDE Files ===
.idea/
*.iml
*.iws
*.ipr
.vscode/
*.swp
*.swo
.project
.classpath
.settings/
bin/

# === OS Files ===
.DS_Store
Thumbs.db
desktop.ini

# === Environment / Secrets (VERY IMPORTANT) ===
application-dev.properties
application-local.properties
application-prod.properties
*.env
.env
.env.local
.env.production

# === Build Files ===
*.jar
*.war
*.class
*.log

# === Node (if using any frontend build tools) ===
node_modules/

# === Docker ===
docker-compose.override.yml
```

### Step 1.4: Push the Initial Setup

```bash
git add .gitignore
git commit -m "Initial project setup with .gitignore"
git push origin main
```

---

## 2. Adding Team Members as Collaborators

### Step 2.1: Add Members on GitHub

1. Go to your repo: `github.com/sarthakpawar0912/bus-ticket-booking-system`
2. Click **Settings** (tab at top)
3. Click **Collaborators** (left sidebar) -> under "Access"
4. Click **Add people**
5. Search by their GitHub username or email
6. Add each member:

```
Member 2: github_username_member2 -> Role: Write
Member 3: github_username_member3 -> Role: Write
Member 4: github_username_member4 -> Role: Write
Member 5: github_username_member5 -> Role: Write
Your account (sarthakpawar0912):   Already Owner (Admin)
```

### Step 2.2: Understand GitHub Roles

| Role | Who | Can Do |
|------|-----|--------|
| **Owner (Admin)** | sarthakpawar0912 (You) | Everything: delete repo, manage settings, merge to main, add/remove members |
| **Write (Contributor)** | Members 2-5 | Push to branches, create PRs, comment. CANNOT push directly to `main` (we'll protect it) |

### Step 2.3: Members Accept the Invitation

Each member will:
1. Get an email from GitHub: "You've been invited to collaborate on bus-ticket-booking-system"
2. Click **Accept invitation**
3. They now have access to the repo

---

## 3. Branch Strategy

### The Golden Rule

```
NOBODY pushes directly to 'main'. EVER.
Not even the Admin.

main is the production-ready branch.
All work happens in feature branches.
Code gets into main ONLY through Pull Requests.
```

### Branch Naming Convention

```
main                              <- Production-ready code (PROTECTED)
  |
  +-- develop                     <- Integration branch (all features merge here first)
       |
       +-- feature/user-service          <- Member 1: Auth, User APIs
       +-- feature/agency-bus-service    <- Member 2: Agency, Bus, Route APIs
       +-- feature/booking-service       <- Member 3: Booking, Trip APIs
       +-- feature/payment-service       <- Member 4: Payment APIs
       +-- feature/frontend-thymeleaf    <- Member 5: All Thymeleaf pages
       |
       +-- feature/agency-add-bus        <- Smaller feature branch (sub-task)
       +-- feature/booking-cancel        <- Smaller feature branch (sub-task)
       |
       +-- bugfix/payment-null-error     <- Bug fix branches
       +-- hotfix/login-crash            <- Urgent fix (goes to main directly)
```

### Visual Flow

```
Member 2 creates branch:  feature/agency-bus-service
Member 2 writes code, commits, pushes to feature/agency-bus-service
Member 2 creates Pull Request: feature/agency-bus-service -> develop
Admin (You) reviews the PR
Admin approves and merges
develop now has Member 2's code

... repeat for all members ...

When develop is stable and tested:
Admin creates PR: develop -> main
Admin merges
main is now updated
```

### Step 3.1: Create the develop Branch (Admin Does This)

```bash
cd bus-ticket-booking-system

# Create develop branch from main
git checkout -b develop
git push origin develop

# Go back to main
git checkout main
```

---

## 4. Each Member's First-Time Setup

### Every team member does this ONCE:

### Step 4.1: Install Git

Download Git from the official site and install it. Verify:

```bash
git --version
# Should show: git version 2.x.x
```

### Step 4.2: Configure Git Identity

```bash
# Each member uses THEIR OWN name and email
git config --global user.name "Member Name"
git config --global user.email "member@email.com"

# Verify
git config --global user.name
git config --global user.email
```

### Step 4.3: Clone the Repository

```bash
# All members run this
cd Desktop   # or wherever they want the project

git clone https://github.com/sarthakpawar0912/bus-ticket-booking-system.git

cd bus-ticket-booking-system
```

GitHub will ask for authentication:
- **Option A:** Use GitHub CLI: `gh auth login`
- **Option B:** Use Personal Access Token (Settings -> Developer Settings -> Personal Access Tokens -> Generate)
- **Option C:** Use SSH key (more advanced but better for long-term)

### Step 4.4: Switch to develop Branch

```bash
git checkout develop
git pull origin develop
```

### Step 4.5: Create Their Feature Branch

```bash
# Member 1 (Admin - Auth/Gateway)
git checkout -b feature/user-service develop

# Member 2 (Agency/Bus)
git checkout -b feature/agency-bus-service develop

# Member 3 (Booking/Trip)
git checkout -b feature/booking-service develop

# Member 4 (Payment)
git checkout -b feature/payment-service develop

# Member 5 (Frontend)
git checkout -b feature/frontend-thymeleaf develop
```

### Step 4.6: Push Their Branch to GitHub

```bash
# Example for Member 2
git push -u origin feature/agency-bus-service

# The -u flag sets up tracking so future 'git push' just works
```

### Step 4.7: Create Their application-dev.properties (NOT committed)

Each member creates `src/main/resources/application-dev.properties`:

```properties
spring.datasource.url=jdbc:mysql://ADMIN_IP:3306/busticketbooking?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=Asia/Kolkata
spring.datasource.username=memberX
spring.datasource.password=MemberX@BusDB2026
```

This file is in `.gitignore` so it will NOT be pushed to GitHub. Each member has their own credentials locally.

---

## 5. Daily Workflow - How to Push & Pull

### The Daily Cycle (EVERY member follows this)

```
MORNING (Start of work):
  1. git checkout develop
  2. git pull origin develop          <- Get latest from everyone
  3. git checkout feature/my-branch
  4. git merge develop                <- Bring latest into your branch
  5. Resolve conflicts if any
  6. Start coding

DURING THE DAY (Save your work frequently):
  7. git add .
  8. git commit -m "Add agency CRUD controller"
  9. git push origin feature/my-branch

END OF DAY (When feature is ready):
  10. git push origin feature/my-branch
  11. Go to GitHub -> Create Pull Request
  12. Wait for Admin review
```

### Detailed Step-by-Step Example: Member 2's Day

**Morning - Get Latest Code:**

```bash
# Step 1: Make sure you're on develop
git checkout develop

# Step 2: Pull latest changes from GitHub
git pull origin develop

# Step 3: Switch to your feature branch
git checkout feature/agency-bus-service

# Step 4: Merge develop INTO your branch (brings in everyone's merged work)
git merge develop

# If there are no conflicts: Great, continue coding
# If there are conflicts: See Section 7 below
```

**During the Day - Code, Commit, Push:**

```bash
# After writing some code...

# See what files you changed
git status

# Example output:
#   modified:   src/main/java/com/busbooking/controller/AgencyController.java
#   new file:   src/main/java/com/busbooking/service/AgencyService.java
#   new file:   src/main/java/com/busbooking/repository/AgencyRepository.java

# Add specific files (BETTER than git add .)
git add src/main/java/com/busbooking/controller/AgencyController.java
git add src/main/java/com/busbooking/service/AgencyService.java
git add src/main/java/com/busbooking/repository/AgencyRepository.java

# Commit with a meaningful message
git commit -m "Add Agency CRUD - controller, service, repository"

# Push to YOUR branch on GitHub
git push origin feature/agency-bus-service
```

**Commit Often with Good Messages:**

```bash
# GOOD commit messages
git commit -m "Add Agency entity with JPA mappings"
git commit -m "Add AgencyRepository with custom findByName query"
git commit -m "Add AgencyService with CRUD operations"
git commit -m "Add AgencyController with REST endpoints"
git commit -m "Add Thymeleaf template for agency listing"
git commit -m "Fix null pointer in AgencyService.getById"

# BAD commit messages (don't do this)
git commit -m "changes"
git commit -m "fix"
git commit -m "updated code"
git commit -m "asdfgh"
```

**End of Day - Create Pull Request:**

```bash
# Make sure everything is pushed
git push origin feature/agency-bus-service
```

Then go to GitHub (see Section 6).

---

## 6. Pull Requests - The Right Way to Merge

### Step 6.1: Member Creates a Pull Request (PR)

1. Go to `github.com/sarthakpawar0912/bus-ticket-booking-system`
2. You'll see a yellow banner: "feature/agency-bus-service had recent pushes. Compare & pull request"
3. Click **Compare & pull request**
4. Fill in:

```
Base branch:  develop         <- WHERE the code goes INTO
Compare:      feature/agency-bus-service  <- YOUR branch

Title:  Add Agency CRUD operations

Description:
## What I Did
- Created Agency entity mapped to agencies table
- Created AgencyRepository with findByName, findByEmail
- Created AgencyService with create, read, update, delete
- Created AgencyController with REST endpoints
- Added agency-list.html Thymeleaf template

## Tables I Touched
- agencies (READ, WRITE)
- agency_offices (READ)
- addresses (READ)

## How to Test
1. Start the app
2. Go to http://localhost:8081/api/agencies
3. Should see list of 10 agencies

## Screenshots
(paste screenshots if UI changes)
```

5. Click **Create pull request**

### Step 6.2: Admin Reviews the PR

1. Admin gets notification (email + GitHub)
2. Admin goes to the PR page
3. Click **Files changed** tab to see all code changes
4. Admin can:
   - **Comment** on specific lines (click the `+` next to any line)
   - **Approve** if code looks good
   - **Request changes** if something needs fixing

**Admin reviews for:**
- Does the code compile?
- Are the JPA entity column names matching the database?
- Is there any hardcoded password or secret?
- Does the code follow the team's package structure?
- Are there any obvious bugs?

### Step 6.3: Admin Merges the PR

1. After approving, click **Merge pull request**
2. Choose **Squash and merge** (recommended - keeps history clean)
3. Click **Confirm merge**
4. Optionally click **Delete branch** (the feature branch, not develop)

Now the code is in `develop`.

### Step 6.4: Other Members Pull the Updated develop

After a PR is merged, ALL other members should:

```bash
git checkout develop
git pull origin develop
git checkout feature/their-branch
git merge develop
```

This keeps everyone in sync.

---

## 7. Merge Conflicts - How to Solve Step by Step

### Why Conflicts Happen

```
Member 2 edits line 15 of Application.java: adds @EnableJpaRepositories
Member 3 edits line 15 of Application.java: adds @EnableScheduling

Both push to different branches.
When Member 3 tries to merge develop (which has Member 2's change),
Git doesn't know WHICH line 15 to keep.

THIS IS A MERGE CONFLICT.
```

### When You'll See Conflicts

```bash
git merge develop
# Output:
# Auto-merging src/main/java/com/busbooking/Application.java
# CONFLICT (content): Merge conflict in src/main/java/com/busbooking/Application.java
# Automatic merge failed; fix conflicts and then commit the result.
```

### Step 7.1: See Which Files Have Conflicts

```bash
git status
# Output:
# Unmerged paths:
#   both modified:   src/main/java/com/busbooking/Application.java
```

### Step 7.2: Open the Conflicting File

Open the file in your IDE (IntelliJ / VS Code). You'll see conflict markers:

```java
package com.busbooking;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

<<<<<<< HEAD
@EnableJpaRepositories
@EnableScheduling
=======
@EnableJpaRepositories
@EnableCaching
>>>>>>> develop

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### Step 7.3: Understand the Conflict Markers

```
<<<<<<< HEAD
  (YOUR changes - what's in YOUR branch)
=======
  (THEIR changes - what's in develop)
>>>>>>> develop
```

### Step 7.4: Resolve the Conflict

You have 3 options:

**Option A: Keep YOUR changes only**
```java
@EnableJpaRepositories
@EnableScheduling
```

**Option B: Keep THEIR changes only**
```java
@EnableJpaRepositories
@EnableCaching
```

**Option C: Keep BOTH (usually the right answer)**
```java
@EnableJpaRepositories
@EnableScheduling
@EnableCaching
```

Delete ALL conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`). The final file should look like normal Java code:

```java
package com.busbooking;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@EnableJpaRepositories
@EnableScheduling
@EnableCaching
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### Step 7.5: Mark as Resolved and Commit

```bash
# After fixing the file, add it
git add src/main/java/com/busbooking/Application.java

# Commit the merge resolution
git commit -m "Merge develop into feature/booking-service - resolve Application.java conflict"

# Push your branch
git push origin feature/booking-service
```

### Step 7.6: Using VS Code to Resolve Conflicts (Easiest Way)

VS Code shows conflicts with colored highlights:

```
    Accept Current Change  |  Accept Incoming Change  |  Accept Both Changes  |  Compare Changes
    -----------------------------------------------------------------------------------------
    <<<<<<< HEAD (Current Change)          <- GREEN
    @EnableScheduling
    =======
    @EnableCaching                         <- BLUE
    >>>>>>> develop (Incoming Change)
```

Just click:
- **Accept Current Change** = Keep yours
- **Accept Incoming Change** = Keep theirs
- **Accept Both Changes** = Keep both (most common)

### Step 7.7: Using IntelliJ IDEA to Resolve Conflicts

1. When conflict happens, IntelliJ shows a **Merge Conflicts** dialog
2. For each file, click **Merge**
3. You'll see 3 panels:
   - Left: YOUR version
   - Right: THEIR version
   - Center: THE RESULT
4. Click arrows `>>` or `<<` to accept changes from left or right
5. Edit the center panel manually if needed
6. Click **Apply**

### Common Conflict Scenarios and Solutions

**Scenario 1: Both edited `pom.xml`**

```xml
<<<<<<< HEAD
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
=======
        <dependency>
            <groupId>com.razorpay</groupId>
            <artifactId>razorpay-java</artifactId>
            <version>1.4.3</version>
        </dependency>
>>>>>>> develop
```

**Solution: Keep BOTH dependencies**
```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
        <dependency>
            <groupId>com.razorpay</groupId>
            <artifactId>razorpay-java</artifactId>
            <version>1.4.3</version>
        </dependency>
```

**Scenario 2: Both edited `application.properties`**

```properties
<<<<<<< HEAD
server.port=8082
spring.application.name=booking-service
=======
server.port=8083
spring.application.name=payment-service
>>>>>>> develop
```

**Solution: This shouldn't happen if each service is in its own module.** If it does, keep the version that belongs to YOUR service.

**Scenario 3: Same file, different methods (usually auto-resolves)**

Member 2 adds `getAgencyById()`, Member 3 adds `getTripsByRoute()` in different files. Git handles this automatically. No conflict.

### How to PREVENT Conflicts

| Rule | Why |
|------|-----|
| Each member works in THEIR OWN package/folder | Different files = no conflicts |
| Pull from develop EVERY MORNING | Smaller differences = easier merges |
| Don't edit shared files without telling the team | `pom.xml`, `Application.java`, `application.properties` |
| Commit and push FREQUENTLY (small commits) | Easier to merge small changes than huge ones |
| Communicate on WhatsApp/Slack before editing shared files | "Hey, I'm editing pom.xml to add a dependency" |

---

## 8. MySQL Table-Level Access Control

### Giving Each Member Access ONLY to Their Tables

```sql
-- =============================================
-- LOGIN AS ROOT ON THE ADMIN'S MACHINE
-- =============================================
mysql -u root -p

-- =============================================
-- DROP the old generic users (if you created them from previous guide)
-- =============================================
DROP USER IF EXISTS 'member2'@'%';
DROP USER IF EXISTS 'member3'@'%';
DROP USER IF EXISTS 'member4'@'%';
DROP USER IF EXISTS 'member5'@'%';

-- =============================================
-- MEMBER 1 (ADMIN - sarthakpawar0912)
-- Full access to EVERYTHING
-- =============================================
-- You already use root, but if you want a separate admin account:
CREATE USER 'admin_sarthak'@'%' IDENTIFIED BY 'Admin@BusDB2026';
GRANT ALL PRIVILEGES ON busticketbooking.* TO 'admin_sarthak'@'%';

-- =============================================
-- MEMBER 2: Agency/Bus/Route Service
-- Tables: agencies, agency_offices, addresses, buses, drivers, routes
-- =============================================
CREATE USER 'member2_agency'@'%' IDENTIFIED BY 'Member2@BusDB2026';

-- Full CRUD on their tables
GRANT SELECT, INSERT, UPDATE, DELETE ON busticketbooking.agencies TO 'member2_agency'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON busticketbooking.agency_offices TO 'member2_agency'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON busticketbooking.addresses TO 'member2_agency'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON busticketbooking.buses TO 'member2_agency'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON busticketbooking.drivers TO 'member2_agency'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON busticketbooking.routes TO 'member2_agency'@'%';

-- Read-only on other tables they might need to JOIN
GRANT SELECT ON busticketbooking.trips TO 'member2_agency'@'%';
GRANT SELECT ON busticketbooking.bookings TO 'member2_agency'@'%';
GRANT SELECT ON busticketbooking.customers TO 'member2_agency'@'%';

-- =============================================
-- MEMBER 3: Booking/Trip Service
-- Tables: trips, bookings, customers, reviews
-- =============================================
CREATE USER 'member3_booking'@'%' IDENTIFIED BY 'Member3@BusDB2026';

-- Full CRUD on their tables
GRANT SELECT, INSERT, UPDATE, DELETE ON busticketbooking.trips TO 'member3_booking'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON busticketbooking.bookings TO 'member3_booking'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON busticketbooking.customers TO 'member3_booking'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON busticketbooking.reviews TO 'member3_booking'@'%';

-- Read-only on tables they need for JOINs/ForeignKeys
GRANT SELECT ON busticketbooking.buses TO 'member3_booking'@'%';
GRANT SELECT ON busticketbooking.routes TO 'member3_booking'@'%';
GRANT SELECT ON busticketbooking.drivers TO 'member3_booking'@'%';
GRANT SELECT ON busticketbooking.addresses TO 'member3_booking'@'%';
GRANT SELECT ON busticketbooking.agencies TO 'member3_booking'@'%';
GRANT SELECT ON busticketbooking.agency_offices TO 'member3_booking'@'%';

-- =============================================
-- MEMBER 4: Payment Service
-- Tables: payments
-- =============================================
CREATE USER 'member4_payment'@'%' IDENTIFIED BY 'Member4@BusDB2026';

-- Full CRUD on their tables
GRANT SELECT, INSERT, UPDATE, DELETE ON busticketbooking.payments TO 'member4_payment'@'%';

-- Read-only on tables they need
GRANT SELECT ON busticketbooking.bookings TO 'member4_payment'@'%';
GRANT SELECT ON busticketbooking.customers TO 'member4_payment'@'%';
GRANT SELECT ON busticketbooking.trips TO 'member4_payment'@'%';
GRANT SELECT ON busticketbooking.buses TO 'member4_payment'@'%';
GRANT SELECT ON busticketbooking.routes TO 'member4_payment'@'%';
GRANT SELECT ON busticketbooking.addresses TO 'member4_payment'@'%';

-- =============================================
-- MEMBER 5: Frontend (Thymeleaf) - Needs to READ everything
-- Tables: ALL (read-only) + write to reviews (user can leave reviews from frontend)
-- =============================================
CREATE USER 'member5_frontend'@'%' IDENTIFIED BY 'Member5@BusDB2026';

-- Read on ALL tables (frontend needs to display everything)
GRANT SELECT ON busticketbooking.* TO 'member5_frontend'@'%';

-- Write access to specific tables the frontend directly interacts with
GRANT INSERT, UPDATE ON busticketbooking.customers TO 'member5_frontend'@'%';
GRANT INSERT, UPDATE ON busticketbooking.reviews TO 'member5_frontend'@'%';
GRANT INSERT, UPDATE ON busticketbooking.bookings TO 'member5_frontend'@'%';

-- =============================================
-- APPLY ALL CHANGES
-- =============================================
FLUSH PRIVILEGES;

-- =============================================
-- VERIFY: See all users and their grants
-- =============================================
SELECT user, host FROM mysql.user WHERE user LIKE 'member%' OR user LIKE 'admin%';

-- Check specific user's permissions
SHOW GRANTS FOR 'member2_agency'@'%';
SHOW GRANTS FOR 'member3_booking'@'%';
SHOW GRANTS FOR 'member4_payment'@'%';
SHOW GRANTS FOR 'member5_frontend'@'%';
```

### Permission Matrix (What Each Member Can Do)

```
Table              | Admin | Member2  | Member3  | Member4  | Member5
                   |       | Agency   | Booking  | Payment  | Frontend
-------------------+-------+----------+----------+----------+---------
agencies           | CRUD  | CRUD     | READ     | -        | READ
agency_offices     | CRUD  | CRUD     | READ     | -        | READ
addresses          | CRUD  | CRUD     | READ     | READ     | READ
buses              | CRUD  | CRUD     | READ     | -        | READ
drivers            | CRUD  | CRUD     | READ     | -        | READ
routes             | CRUD  | CRUD     | READ     | READ     | READ
trips              | CRUD  | READ     | CRUD     | READ     | READ
bookings           | CRUD  | READ     | CRUD     | READ     | READ+WRITE
customers          | CRUD  | READ     | CRUD     | READ     | READ+WRITE
payments           | CRUD  | -        | -        | CRUD     | READ
reviews            | CRUD  | -        | CRUD     | -        | READ+WRITE

CRUD = SELECT + INSERT + UPDATE + DELETE
READ = SELECT only
-    = No access
```

### Test That Permissions Work

```sql
-- Login as member2_agency
mysql -u member2_agency -p -h ADMIN_IP busticketbooking

-- This should WORK
SELECT * FROM agencies;
INSERT INTO agencies (name, contact_person_name, email, phone) 
VALUES ('Test Agency', 'Test Person', 'test@test.com', '1234567890');

-- This should FAIL (no access to payments)
SELECT * FROM payments;
-- ERROR 1142 (42000): SELECT command denied to user 'member2_agency'@'...' for table 'payments'

-- This should FAIL (no DELETE permission on trips, only SELECT)
DELETE FROM trips WHERE trip_id = 1;
-- ERROR 1142 (42000): DELETE command denied
```

---

## 9. Project Folder Structure for Team

### This structure PREVENTS merge conflicts because each member works in their own package:

```
bus-ticket-booking-system/
|
|-- pom.xml                          <- SHARED (be careful editing this)
|
|-- src/main/java/com/busbooking/
|   |
|   |-- BusTicketBookingApplication.java    <- SHARED (rarely changes)
|   |
|   |-- config/                              <- MEMBER 1 (Admin)
|   |   |-- SecurityConfig.java
|   |   |-- WebConfig.java
|   |   |-- JwtConfig.java
|   |
|   |-- security/                            <- MEMBER 1 (Admin)
|   |   |-- JwtUtil.java
|   |   |-- JwtAuthFilter.java
|   |   |-- CustomUserDetailsService.java
|   |
|   |-- agency/                              <- MEMBER 2 ONLY (their package)
|   |   |-- controller/
|   |   |   |-- AgencyController.java
|   |   |   |-- BusController.java
|   |   |   |-- DriverController.java
|   |   |   |-- RouteController.java
|   |   |-- service/
|   |   |   |-- AgencyService.java
|   |   |   |-- BusService.java
|   |   |   |-- DriverService.java
|   |   |   |-- RouteService.java
|   |   |-- repository/
|   |   |   |-- AgencyRepository.java
|   |   |   |-- BusRepository.java
|   |   |   |-- DriverRepository.java
|   |   |   |-- RouteRepository.java
|   |   |-- model/
|   |   |   |-- Agency.java
|   |   |   |-- AgencyOffice.java
|   |   |   |-- Bus.java
|   |   |   |-- Driver.java
|   |   |   |-- Route.java
|   |   |-- dto/
|   |       |-- AgencyDTO.java
|   |       |-- BusDTO.java
|   |
|   |-- booking/                             <- MEMBER 3 ONLY (their package)
|   |   |-- controller/
|   |   |   |-- TripController.java
|   |   |   |-- BookingController.java
|   |   |   |-- CustomerController.java
|   |   |   |-- ReviewController.java
|   |   |-- service/
|   |   |   |-- TripService.java
|   |   |   |-- BookingService.java
|   |   |   |-- CustomerService.java
|   |   |   |-- ReviewService.java
|   |   |-- repository/
|   |   |   |-- TripRepository.java
|   |   |   |-- BookingRepository.java
|   |   |   |-- CustomerRepository.java
|   |   |   |-- ReviewRepository.java
|   |   |-- model/
|   |   |   |-- Trip.java
|   |   |   |-- Booking.java
|   |   |   |-- BookingStatus.java
|   |   |   |-- Customer.java
|   |   |   |-- Review.java
|   |   |-- dto/
|   |       |-- BookingDTO.java
|   |       |-- TripSearchDTO.java
|   |
|   |-- payment/                             <- MEMBER 4 ONLY (their package)
|   |   |-- controller/
|   |   |   |-- PaymentController.java
|   |   |-- service/
|   |   |   |-- PaymentService.java
|   |   |-- repository/
|   |   |   |-- PaymentRepository.java
|   |   |-- model/
|   |   |   |-- Payment.java
|   |   |   |-- PaymentStatus.java
|   |   |-- dto/
|   |       |-- PaymentDTO.java
|   |       |-- PaymentRequestDTO.java
|   |
|   |-- shared/                              <- SHARED models (Address used by everyone)
|   |   |-- model/
|   |   |   |-- Address.java
|   |   |-- repository/
|   |   |   |-- AddressRepository.java
|   |   |-- dto/
|   |       |-- AddressDTO.java
|   |
|   |-- web/                                 <- MEMBER 5 (Thymeleaf page controllers)
|       |-- HomePageController.java
|       |-- AgencyPageController.java
|       |-- BookingPageController.java
|       |-- PaymentPageController.java
|       |-- TripSearchPageController.java
|       |-- CustomerPageController.java
|
|-- src/main/resources/
|   |-- application.properties               <- SHARED (use env variables)
|   |-- application-dev.properties           <- NOT in git (.gitignore)
|   |
|   |-- templates/                           <- MEMBER 5 ONLY
|   |   |-- layout/
|   |   |   |-- base.html
|   |   |-- fragments/
|   |   |   |-- navbar.html
|   |   |   |-- footer.html
|   |   |-- home/
|   |   |   |-- index.html
|   |   |   |-- search-results.html
|   |   |-- agency/
|   |   |   |-- list.html
|   |   |   |-- detail.html
|   |   |   |-- add-bus.html
|   |   |-- booking/
|   |   |   |-- select-seats.html
|   |   |   |-- confirmation.html
|   |   |   |-- my-bookings.html
|   |   |-- payment/
|   |   |   |-- checkout.html
|   |   |   |-- success.html
|   |   |-- customer/
|   |       |-- profile.html
|   |       |-- register.html
|   |       |-- login.html
|   |
|   |-- static/
|       |-- css/
|       |   |-- style.css
|       |-- js/
|       |   |-- app.js
|       |   |-- seat-selector.js
|       |-- images/
|
|-- src/test/java/com/busbooking/           <- Each member writes tests for their package
|   |-- agency/
|   |-- booking/
|   |-- payment/
|
|-- database/
|   |-- schema.sql                           <- The SQL file you shared
|   |-- seed-data.sql                        <- Insert statements
|
|-- Dockerfile
|-- docker-compose.yml
|-- .gitignore
|-- README.md
```

### Key Point: Each member ONLY edits files in their package

```
Member 2 edits files in:  src/main/java/com/busbooking/agency/**
Member 3 edits files in:  src/main/java/com/busbooking/booking/**
Member 4 edits files in:  src/main/java/com/busbooking/payment/**
Member 5 edits files in:  src/main/java/com/busbooking/web/** AND src/main/resources/templates/**

Member 1 edits files in:  src/main/java/com/busbooking/config/** AND src/main/java/com/busbooking/security/**
```

Since they're editing DIFFERENT FILES, merge conflicts are RARE.

---

## 10. Branch Protection Rules (Admin Sets This Up)

### Protect the main and develop Branches

1. Go to repo -> **Settings** -> **Branches**
2. Click **Add branch protection rule**

**For `main` branch:**
```
Branch name pattern: main

CHECK:  Require a pull request before merging
CHECK:  Require approvals (set to 1)
CHECK:  Dismiss stale pull request approvals when new commits are pushed
CHECK:  Require status checks to pass before merging (if you have CI/CD)
CHECK:  Include administrators (even YOU can't push directly)
CHECK:  Restrict who can push to matching branches
        -> Add: sarthakpawar0912 (only admin can merge PRs to main)
```

**For `develop` branch:**
```
Branch name pattern: develop

CHECK:  Require a pull request before merging
CHECK:  Require approvals (set to 1)
UNCHECK: Include administrators (admin can merge to develop without PR in emergencies)
```

After this setup:
- NOBODY can `git push origin main` directly. Will be rejected.
- NOBODY can `git push origin develop` directly. Must use PR.
- Only PRs that are approved can be merged.

---

## 11. Real Scenarios - Who Does What

### Scenario 1: Member 2 Wants to Add a New Bus Endpoint

```bash
# Member 2's machine

# 1. Morning sync
git checkout develop
git pull origin develop
git checkout feature/agency-bus-service
git merge develop

# 2. Write code
# Edit: src/main/java/com/busbooking/agency/controller/BusController.java
# Edit: src/main/java/com/busbooking/agency/service/BusService.java

# 3. Test locally
# Run Spring Boot app, test the endpoint

# 4. Commit and push
git add src/main/java/com/busbooking/agency/controller/BusController.java
git add src/main/java/com/busbooking/agency/service/BusService.java
git commit -m "Add GET /api/buses/{id} endpoint with bus details"
git push origin feature/agency-bus-service

# 5. Go to GitHub -> Create PR -> feature/agency-bus-service -> develop
# 6. Admin reviews and merges
```

### Scenario 2: Member 3 Needs the Bus Entity That Member 2 Created

```bash
# Member 3's machine

# Member 2's PR was merged to develop, which includes Bus.java

# 1. Get latest develop
git checkout develop
git pull origin develop

# 2. Merge into your branch
git checkout feature/booking-service
git merge develop

# Now Member 3 has Bus.java and can use it in Trip.java

# 3. Write code
# Edit: src/main/java/com/busbooking/booking/model/Trip.java
# (Trip has @ManyToOne to Bus)

# 4. Commit and push
git add src/main/java/com/busbooking/booking/model/Trip.java
git commit -m "Add Trip entity with bus and route relationships"
git push origin feature/booking-service
```

### Scenario 3: Two Members Edit pom.xml (CONFLICT!)

```bash
# Member 2 adds spring-boot-starter-mail to pom.xml
# Member 4 adds razorpay dependency to pom.xml
# Member 2's PR gets merged first

# Now Member 4 tries to merge develop:
git checkout develop
git pull origin develop
git checkout feature/payment-service
git merge develop

# CONFLICT in pom.xml!

# Member 4 opens pom.xml, sees:
# <<<<<<< HEAD
#     <dependency>razorpay</dependency>
# =======
#     <dependency>spring-boot-starter-mail</dependency>
# >>>>>>> develop

# Member 4 keeps BOTH:
#     <dependency>razorpay</dependency>
#     <dependency>spring-boot-starter-mail</dependency>

# Resolve:
git add pom.xml
git commit -m "Merge develop - keep both razorpay and mail dependencies"
git push origin feature/payment-service
```

### Scenario 4: Admin Releases to main

```bash
# All features are merged to develop
# develop is tested and working

# Admin (sarthakpawar0912):

# 1. Make sure develop is up to date
git checkout develop
git pull origin develop

# 2. Go to GitHub
# Create PR: develop -> main
# Title: "Release v1.0 - MVP with Agency, Booking, Payment features"
# Description: List all features included

# 3. Merge the PR on GitHub
# develop is now merged into main
# main has the latest stable code
```

### Scenario 5: Urgent Bug Fix (Hotfix)

```bash
# Production bug! Login is crashing!

# Admin:
git checkout main
git pull origin main
git checkout -b hotfix/login-crash

# Fix the bug
# Edit the file
git add .
git commit -m "Fix NullPointerException in login when email is null"
git push origin hotfix/login-crash

# Create PR: hotfix/login-crash -> main (SKIP develop for urgency)
# Merge immediately

# THEN merge main back to develop so develop gets the fix too:
git checkout develop
git pull origin develop
git merge main
git push origin develop
```

---

## 12. Cloud Deployment

### Option A: Railway.app (Easiest - Free Tier)

**For the Database:**

1. Go to **railway.app** -> Sign up with GitHub
2. Click **New Project** -> **Provision MySQL**
3. Click on the MySQL service -> **Variables** tab
4. Copy these values:
   ```
   MYSQL_HOST=containers-us-west-xxx.railway.app
   MYSQL_PORT=6312
   MYSQL_DATABASE=railway
   MYSQL_USER=root
   MYSQL_PASSWORD=xxxxx
   ```
5. Connect to it and run your SQL schema:
   ```bash
   mysql -h containers-us-west-xxx.railway.app -P 6312 -u root -pxxxxx railway < schema.sql
   ```

**For the Spring Boot App:**

1. In Railway, click **New** -> **GitHub Repo** -> Select `bus-ticket-booking-system`
2. Railway auto-detects Spring Boot and builds it
3. Add environment variables:
   ```
   DB_HOST=containers-us-west-xxx.railway.app
   DB_PORT=6312
   DB_NAME=railway
   DB_USER=root
   DB_PASS=xxxxx
   SPRING_PROFILES_ACTIVE=prod
   ```
4. Railway gives you a URL like: `bus-ticket-booking-system.up.railway.app`

### Option B: AWS (EC2 + RDS) - Production Grade

**Step 1: Create RDS MySQL Database**

1. Go to **AWS Console** -> **RDS** -> **Create database**
2. Settings:
   ```
   Engine:        MySQL 8.0
   Template:      Free tier
   DB Instance:   db.t3.micro
   DB name:       busticketbooking
   Master user:   admin
   Master pass:   YourStrongPassword123!
   Public access: Yes (for development, No for production)
   ```
3. After creation, note the **Endpoint**:
   ```
   busticketbooking.xxxx.us-east-1.rds.amazonaws.com
   ```
4. In **Security Group**, open port 3306 for your team's IPs
5. Connect and run your SQL:
   ```bash
   mysql -h busticketbooking.xxxx.us-east-1.rds.amazonaws.com -u admin -p busticketbooking < schema.sql
   ```

**Step 2: Create EC2 Instance for the App**

1. **EC2** -> **Launch Instance**
   ```
   AMI:           Amazon Linux 2023
   Instance type: t2.micro (free tier)
   Key pair:      Create new -> download .pem file
   Security group: Open ports 8080, 22
   ```

2. SSH into the instance:
   ```bash
   ssh -i "your-key.pem" ec2-user@your-ec2-public-ip
   ```

3. Install Java and run:
   ```bash
   sudo yum install java-17-amazon-corretto -y
   
   # Upload your JAR (or pull from GitHub)
   git clone https://github.com/sarthakpawar0912/bus-ticket-booking-system.git
   cd bus-ticket-booking-system
   ./mvnw package -DskipTests
   
   # Run with environment variables
   DB_HOST=busticketbooking.xxxx.rds.amazonaws.com \
   DB_PORT=3306 \
   DB_USER=admin \
   DB_PASS=YourStrongPassword123 \
   java -jar target/bus-ticket-booking-0.0.1-SNAPSHOT.jar
   ```

4. Access at: `http://your-ec2-public-ip:8080`

### Option C: Docker + AWS ECS (Best for Your Project)

**Step 1: Create Dockerfile in project root**

```dockerfile
FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app
COPY . .
RUN ./mvnw package -DskipTests

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

**Step 2: docker-compose.yml for local testing**

```yaml
version: '3.8'
services:
  mysql:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: busticketbooking
    ports:
      - "3306:3306"
    volumes:
      - ./database/schema.sql:/docker-entrypoint-initdb.d/01-schema.sql
      - mysql-data:/var/lib/mysql

  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:mysql://mysql:3306/busticketbooking?useSSL=false&allowPublicKeyRetrieval=true
      - SPRING_DATASOURCE_USERNAME=root
      - SPRING_DATASOURCE_PASSWORD=root
    depends_on:
      - mysql

volumes:
  mysql-data:
```

**Step 3: Test locally**

```bash
docker-compose up --build
# Visit http://localhost:8080
```

**Step 4: Push to AWS ECR + Deploy to ECS**

```bash
# Login to ECR
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com

# Create repository
aws ecr create-repository --repository-name bus-ticket-booking

# Build, tag, push
docker build -t bus-ticket-booking .
docker tag bus-ticket-booking:latest ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com/bus-ticket-booking:latest
docker push ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com/bus-ticket-booking:latest

# Create ECS cluster and service via AWS Console or CLI
```

### Option D: GitHub Actions CI/CD (Automated Deployment)

Create `.github/workflows/deploy.yml`:

```yaml
name: Build and Deploy

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Build with Maven
        run: ./mvnw clean package -DskipTests

      - name: Build Docker Image
        run: docker build -t bus-ticket-booking .

      - name: Login to AWS ECR
        uses: aws-actions/amazon-ecr-login@v2
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_REGION: ap-south-1

      - name: Push to ECR
        run: |
          docker tag bus-ticket-booking:latest ${{ secrets.ECR_REGISTRY }}/bus-ticket-booking:latest
          docker push ${{ secrets.ECR_REGISTRY }}/bus-ticket-booking:latest

      - name: Deploy to ECS
        run: |
          aws ecs update-service --cluster bus-booking-cluster --service bus-booking-service --force-new-deployment
```

---

## 13. Quick Reference Commands

### For ALL Members - Daily Commands

```bash
# === START OF DAY ===
git checkout develop
git pull origin develop
git checkout feature/your-branch
git merge develop

# === DURING WORK ===
git status                              # See what changed
git add path/to/file.java              # Stage specific file
git commit -m "Meaningful message"      # Commit
git push origin feature/your-branch     # Push to GitHub

# === SEE HISTORY ===
git log --oneline -10                   # Last 10 commits
git log --oneline --graph --all         # Visual branch tree
git diff                                # See unstaged changes
git diff --staged                       # See staged changes

# === UNDO MISTAKES ===
git checkout -- path/to/file.java       # Discard local changes to a file
git reset HEAD path/to/file.java        # Unstage a file
git stash                               # Temporarily save changes
git stash pop                           # Restore stashed changes
```

### For ADMIN Only - Management Commands

```bash
# === MERGE develop TO main (Release) ===
git checkout main
git pull origin main
git merge develop
git push origin main

# === CREATE A NEW BRANCH FOR A MEMBER ===
git checkout develop
git pull origin develop
git checkout -b feature/new-feature
git push -u origin feature/new-feature

# === DELETE A MERGED BRANCH ===
git branch -d feature/old-branch           # Delete locally
git push origin --delete feature/old-branch # Delete on GitHub

# === SEE ALL BRANCHES ===
git branch -a                               # All branches (local + remote)
git branch -r                               # Remote branches only
```

### Emergency Commands

```bash
# === I COMMITTED TO THE WRONG BRANCH ===
# (Haven't pushed yet)
git log --oneline -1                    # Note the commit hash
git reset HEAD~1                        # Undo the commit (keeps changes)
git stash                               # Stash the changes
git checkout correct-branch             # Switch to right branch
git stash pop                           # Apply changes here
git add . && git commit -m "message"    # Commit on right branch

# === I NEED TO SEE WHAT'S IN A PR BEFORE IT'S MERGED ===
git fetch origin
git checkout origin/feature/their-branch

# === SOMETHING BROKE, GO BACK TO LAST WORKING STATE ===
git log --oneline -20                   # Find the good commit
git checkout <good-commit-hash>         # View that state (detached HEAD)
# To actually revert:
git revert <bad-commit-hash>            # Creates a new commit that undoes the bad one
```

---

## Team Communication Template

Share this with your WhatsApp/Slack group:

```
=== BUS TICKET BOOKING - TEAM SETUP ===

GitHub Repo: https://github.com/sarthakpawar0912/bus-ticket-booking-system

Branch Strategy:
  main     <- DO NOT TOUCH (production)
  develop  <- All features merge here via PR
  feature/your-branch <- Your working branch

Your Branches:
  Member 1 (Admin):    feature/user-service
  Member 2 (Agency):   feature/agency-bus-service
  Member 3 (Booking):  feature/booking-service
  Member 4 (Payment):  feature/payment-service
  Member 5 (Frontend): feature/frontend-thymeleaf

Database Credentials:
  (shared separately and securely)

Daily Rules:
  1. Pull develop every morning
  2. Never push to main or develop directly
  3. Create PR when feature is ready
  4. Only edit files in YOUR package
  5. Tell the group before editing pom.xml or shared files
  6. ddl-auto = validate ALWAYS
```
