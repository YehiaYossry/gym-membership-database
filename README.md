# Gym Management System Database

A comprehensive **SQL Server (T-SQL)** database schema and management system designed to handle gym operations, including member subscriptions, trainer assignments, class schedules, payment processing, and training plans.

## 📌 Features & Business Logic

- **Member & Subscription Tracking:** Tracks member details, active subscriptions, expiration timelines, and payment histories.
- **Class & Trainer Management:** Maps trainers to specialized fitness classes, difficulty levels, certificates, and work hours.
- **Custom Functions:**
  - `DaysUntilExpiry()`: Tabular function calculating remaining subscription days for each member.
  - `fn_FilterClasses()`: Multi-parameter filter function for querying classes by type, date, or difficulty level.
- **Stored Procedures:** Comprehensive CRUD procedures for inserting, updating, and cascade-deleting database records across all entities.
- **Data Integrity:** Strict foreign key constraints and primary keys maintain relational integrity across entities.

## 🛠️ Database Schema Architecture

### Core Entities

- **`Memberr`:** Primary member profiles linked to personal trainers and training plans.
- **`trainer`:** Trainer records including work hours and personal details.
- **`membership` & `Membershiptype`:** Categorized membership packages (Monthly, Quarterly, Yearly, VIP) with pricing tiers.
- **`Subscription` & `Payment`:** Tracks membership start/end dates, active status, payment types, and amounts.
- **`Class` & `ClassDifficulty`:** Scheduled classes categorized by duration and difficulty levels (Beginner, Intermediate, Advanced, Expert).
- **`trainingplan`:** Custom workout and diet plans assigned to members.

### Junction & Composite Entities

- **`enrollsin`:** Many-to-many relationship mapping members to enrolled classes.
- **`Classdate`:** Scheduled execution dates for individual classes.
- **`Memberphoneno` & `trainerphoneno`:** Multi-valued phone contact storage.
- **`trainercertificate`:** Professional certifications held by trainers.

## 🚀 Execution & Setup Guide

### 1. Requirements

- Microsoft SQL Server (2016 or newer recommended)
- SQL Server Management Studio (SSMS) or Azure Data Studio

### 2. Database Creation & Seeding

1. Open SQL Server Management Studio (SSMS) and connect to your SQL Server instance.
2. Ensure the target database `Gym` exists, or create it:

   ```sql
   CREATE DATABASE Gym;
   GO
   USE Gym;
   GO
   ```

3. Execute the SQL script. The script will automatically:
   - Drop existing tables if present to avoid conflicts.
   - Create all tables with foreign key constraints.
   - Compile utility functions (`DaysUntilExpiry`, `fn_FilterClasses`).
   - Create stored procedures for insertion, updates, and cascade deletions (`DeleteFromTable`).
   - Seed default initial data for members, trainers, classes, payments, and subscriptions.

> **Warning:** The script drops existing tables if they are present. Back up any important data before running it.

## 📊 Sample Queries

### Check Remaining Subscription Days

```sql
SELECT
    d.MemberName AS [Member Name],
    d.statuss AS [Subscription Status],
    d.DaysUntilExpiry AS [Days to expire]
FROM [dbo].[DaysUntilExpiry]() d
ORDER BY d.DaysUntilExpiry DESC;
```

### Count Members per Personal Trainer

```sql
SELECT
    t.trainerfname + ' ' + t.trainerlname AS [Trainer name],
    COUNT(m.memberid) AS [Members]
FROM trainer t
JOIN Memberr m ON m.trainerid = t.trainerid
GROUP BY t.trainerfname, t.trainerlname;
```
