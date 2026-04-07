# Fitness Influencer Coaching Platform

This repository contains the Entity-Relationship (ER) design for a fitness influencer coaching platform. It defines user and trainer roles, fitness plans, sessions, reviews, progress tracking, payments, and related communication workflows.

![ER Diagram](ER.png)

## Overview

The platform supports:
- multiple user roles including admin, trainer, assistant, and standard users
- trainer-managed fitness plans that can be assigned to users
- multiple users subscribing to the same plan
- user reviews and FAQs for each trainer fitness plan
- session scheduling and attendance tracking
- progress reporting and payment records for user fitness plans

## Tables

### roles
- `id` number SERIAL pk
- `role_name` enum [user, admin, trainer, assistant]
- `can_schedule_meetings` boolean
- `can_access_admin_dashboard` boolean
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### users
- `id` string pk
- `role_id` number
- `email` string unique
- `phone` string
- `insta` string
- `twitter` string
- `fb` string
- `password` string
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### trainers
- `id` string fk
- `role_id` number
- `email` string unique
- `phone` string
- `insta` string
- `twitter` string
- `fb` string
- `qualification` string
- `password` string
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### trainers_users
- `id` string fk
- `user_id` string
- `trainer_id` string
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### communications
- `id` string pk
- `type` enum [online, offline, sessions]
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### trainer_fitness_plans
- `id` string pk
- `plan_name` string
- `plan_communication` string
- `trainer_incharge` string
- `created_by` string
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### user_fitness_plans
- `id` string pk
- `user_id` string
- `weight_in_kg` number
- `height_in_cm` number
- `body_fat_in_gm` number
- `other_measurements` string
- `bmi` number
- `status` enum [active, completed, cancelled]
- `end_date` timestamp
- `trainer_fitness_plans_id` string
- `joined_at` timestamp
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### faqs
- `id` string pk
- `trainer_fitness_plans_id` string
- `title` string
- `description` string
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### reviews
- `id` string pk
- `trainer_fitness_plans_id` string
- `trainer_id` string
- `user_id` string
- `rating` number
- `title` string
- `description` string
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### payments
- `id` string pk
- `status` enum [success, failure]
- `user_fitness_plan_id` string
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### sessions_with_trainers
- `id` string pk
- `trainer_fitness_plans_id` string
- `communication_type` string
- `scheduled_at` timestamp
- `started_at` timestamp
- `ended_at` timestamp
- `recording_link` string | null
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### session_records
- `id` string pk
- `sessions_with_trainers_id` string
- `user_id` string
- `joined_at` timestamp
- `left_at` timestamp
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

### progress_reports
- `id` string pk
- `user_fitness_plan_id` string
- `image_link` string
- `is_deleted` boolean
- `created_at` timestamp
- `updated_at` timestamp

## Key relationships

- `users.role_id` links to `roles.id`
- `trainers.role_id` links to `roles.id`
- `trainers_users.user_id` links to `users.id`
- `trainers_users.trainer_id` links to `trainers.id`
- `trainer_fitness_plans.created_by` links to `trainers.id`
- `user_fitness_plans.user_id` links to `users.id`
- `user_fitness_plans.trainer_fitness_plans_id` links to `trainer_fitness_plans.id`
- `faqs.trainer_fitness_plans_id` links to `trainer_fitness_plans.id`
- `reviews.trainer_fitness_plans_id` links to `trainer_fitness_plans.id`
- `reviews.user_id` links to `users.id`
- `reviews.trainer_id` links to `trainers.id`
- `payments.user_fitness_plan_id` links to `user_fitness_plans.id`
- `sessions_with_trainers.trainer_fitness_plans_id` links to `trainer_fitness_plans.id`
- `session_records.sessions_with_trainers_id` links to `sessions_with_trainers.id`
- `session_records.user_id` links to `users.id`
- `progress_reports.user_fitness_plan_id` links to `user_fitness_plans.id`

## Design notes

- The `roles` table enables a single user or trainer to have a defined access level.
- Soft deletes are supported through `is_deleted` on every major table.
- `trainer_fitness_plans` represents plans owned and managed by trainers.
- `user_fitness_plans` is the join table linking users to the plans they subscribe to.
- Sessions use `communications` types and `sessions_with_trainers` for scheduling.
- Attendance/video call details are stored in `session_records`.
- Progress tracking is handled by `progress_reports` for each user fitness plan.

## Usage

Open `ER.png` to view the ER diagram visually. The diagram is stored in this folder and referenced above for quick access.
