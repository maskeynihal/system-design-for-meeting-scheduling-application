# API documentation

## POST: /v1/login

- This route will be used to login to the system by providing credentials
- The response of the API will be `refreshToken` and `accessToken`

## GET: /v1/meetings/availability?userIds[]=1&userIds[]=2&from="2022-01-01 13:30"&to="2022-01-01 19:45"

- The route will be used to return the array of date time range in which the selected users are available
- The params are:
  - userIds (array of string)
    - The users whose availability is to be found.
  - from (datetime)
    - Start date time for the availability
  - to (datetime)
    - End date time for the availability

## GET: /v1/resources?from="2022-01-01 10:20"&to="2022-01-01 14:05"

- The route will be used to return the array of datetime range with `resourceIds` in which the resources are available
- The params are:
  - from (datetime)
    - Start date time for the availability
  - to (datetime)
    - End date time for the availability

## POST: /v1/meetings

- The route will be used to create a new meetings
- The request payload are:
  - userIds (array of string)
    - The list of user who are invited to the meeting
  - title (string)
    - Title of the meeting
  - description (string)
    - Description of the meeting
  - location (string)
    - location of the meeting
  - start_time (string)
    - Start time of the meeting
  - end_time (string)
    - End time of the meeting
  - timezone (string)
    - The time zone of the meeting. The time zone is for `start_time` and `end_time` of the meeting
  - is_all_day (boolean)
    - `true` if the meeting is for whole day, `false` if not
  - recurring_pattern (string/enum)
    - Decides how the meetings are recurred
    - Possible values:
      - ONCE
      - DAILY
      - WEEKLY
      - MONTHLY
      - YEARLY
  - recurrence_end_date (date)
    - When the recurrence ends
  - reminders (array of object)
    - This will store the information about when the reminder notifications are sent to users.
    - The shape of the object is:
      - recurring_pattern (string/enum)
        - Decides how the reminder for the meeting should be recurred. The `recurring_pattern` can be different from the `recurring_pattern` of the meeting. For example: `recurring_pattern` of the meeting may be `daily` but the meeting organizer may want to send `reminder` for the meeting `weekly`. May be so that the invitees can be up to date to the meeting agenda or so on.
      - first_recurring_start_date (date)
        - Store date in which the first reminder is sent. So using this date, other following reminder dates can be calculated.
      - first_recurring_start_time (time)
        - Store time in which the first reminder is sent. So using this time, other following reminder times can be calculated
      - reminder_timezone (string)
        - timezone of the reminder
  - users (array of object)
    - This will store the array of users/guest who are invited to the meeting
    - The shape of the object will be:
      - user_id (string/foreign key)
        - The user id of the user who are enrolled in the system
      - is_guest (boolean)
        - `true` if the user is not enrolled in the system else `false`
      - email (string)
        - valid email for users who are guest.

## POST: /v1/meetings/:meeting-uuid/invitation

- The route will be used to update if the invitation is accepted or not.
- For the guest we can have a unique signed with time limited url which can be used to accept or reject the invitation
- Invitee who are enrolled in the system can accept/reject the invitation from the system
- The request payload are:
  - message (string)
    - Add message to meeting creator (optional)
  - invitation_status (string/enum)
    - Allowed values:
      - ACCEPTED
      - DECLINED
      - MAYBE

## PUT: /v1/meetings/:meeting-uuid

- Route to update the meeting information
- All the payload available in the POST request of creating meeting

## PATCH: /v1/meetings/:meeting-uuid/status

- Route to update the status of the meeting
- Request payload
  - status (string/enum)
    - Possible value
      - CANCELLED
  - message (string)
    - Add message to send to invitee to update the status
