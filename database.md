# Database design

This database schema represents a relational database structure with multiple tables and relationships between them. Let's go through each table and its columns to understand the schema in detail:

### Tables

**companies**
The table `companies` will store the companies that are enrolled in the system.

- **id** (primary key): Represents the unique identifier for each company.
- **name**: Stores the name of the company.
- **contact_email** (unique): Stores the email address for contacting the company. It has a uniqueness constraint.
- **address**: Stores the address of the company.
- **created_at**: Represents the date and time when the company record was created.
- **updated_at**: Represents the date and time when the company record was last updated.

**resources**
The table `resources` will store the data related to resources of the company. The resources are those which are used in meetings.

- **id** (primary key): Represents the unique identifier for each resource.
- **name**: Stores the name of the resource.
- **company_id** (foreign key reference to companies.id): Represents the ID of the company to which the resource belongs.
- **created_at**: Represents the date and time when the resource record was created.
- **updated_at**: Represents the date and time when the resource record was last updated.

**users**
The table `users` store information of individual who uses the system. The table also login credentials of the user.

- **id** (primary key): Represents the unique identifier for each user.
- **first_name**: Stores the first name of the user.
- **last_name**: Stores the last name of the user.
- **username**: Stores the username of the user.
- **email** (unique): Stores the email address of the user. It has a uniqueness constraint.
- **password**: Stores the hashed password of the user.
- **created_at**: Represents the date and time when the user record was created.
- **updated_at**: Represents the date and time when the user record was last updated.

**company_user**
The table `company_user` creates linkage between the `users` and `companies`. The separate table for linkage so that the user can be part of more than one company.

- **company_id** (foreign key reference to companies.id): Represents the ID of the company.
- **user_id** (foreign key reference to users.id): Represents the ID of the user.

**meetings**
The table `meetings` store the information about the meetings.

- **id** (primary key): Represents the unique identifier for each meeting.
- **title**: Stores the title of the meeting.
- **description**: Stores the description of the meeting.
- **location**: Stores the location of the meeting.
- **start_time**: Represents the start time of the meeting.
- **end_time**: Represents the end time of the meeting.
- **is_all_day**: Indicates whether the meeting is an all-day event.
- **timezone**: Stores the timezone in which the meeting was created.
- **recurring_pattern**: Represents the recurring pattern of the meeting (e.g., once, daily, weekly, etc.).
- **recurrence_end_date**: Represents the end date for recurring meetings.
- **created_by** (foreign key reference to users.id): Represents the ID of the user who created the meeting.
- **created_at**: Represents the date and time when the meeting record was created.
- **updated_at**: Represents the date and time when the meeting record was last updated.
- **status**: Represents the status of the meeting (e.g., scheduled, cancelled).

**meeting_resource**
The table `meeting_resource` stores the data related to resources that are used in meeting. The meeting can use one or more resources or none.

- **meeting_id** (foreign key reference to meetings.id): Represents the ID of the meeting.
- **resource_id** (foreign key reference to resources.id): Represents the ID of the resource.

**invitations**
The table `invitation` store the data related to the users/guests that are invited to the meetings. The table also have information if the users are enrolled in the system are if they are guest (who doesn't have account in the system)

- **meeting_id** (foreign key reference to meetings.id): Represents the ID of the meeting.
- **user_id** (foreign key reference to users.id): Represents the ID of the user.
- **email**: Stores the email address of the invitee.
- **is_guest**: Indicates whether the invitee is a guest user.
- **message**: Stores an optional message accompanying the invitation.
- **invitation_status**: Represents the status of the invitation (e.g., accepted, declined, etc.).

**reminders**
The table `reminder` will store the information about the reminders about the event. Users can add custom reminders to the event like _Reminder all the users about the event before 15 mins of event_

- **id** (primary key): Represents the unique identifier for each reminder.
- **meeting_id** (foreign key reference to meetings.id): Represents the ID of the meeting.
- **value**: Stores the value of the reminder.
- **recurring_pattern**: Represents the recurring pattern of the reminder.
- **reminder_type**: Represents the type of reminder (e.g., email).
- **reminder_time**: Represents the date and time of the reminder.
- **reminder_timezone**: Stores the timezone for the reminder.

**notifications**
The `notifications` table stores information about the notifications to be sent, including the recipient, message content, and delivery method.

- **id** (primary key): Represents the unique identifier for each notification.
- **reminder** (foreign key reference to reminders.id): Represents the ID of the related reminder.
- **sender**: Stores the sender of the notification.
- **receiver**: Stores the receiver of the notification.
- **notification_channel**: Stores the channel through which the notification is sent.
- **message**: Stores the template message for the notification, which can be customized.
- **value**: Stores a JSON object with variables that will be replaced in the message.

### Enums

**RECURRING_PATTERN**

- ONCE
- DAILY
- WEEKLY
- MONTHLY
- YEARLY

**MEETING_STATUS**

- SCHEDULED
- CANCELLED

**INVITATION_STATUS**

- ACCEPTED
- DECLINED
- MAYBE
- CANCELLED

**NOTIFICATION_CHANNEL**

- EMAIL
