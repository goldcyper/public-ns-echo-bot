# Citizenship Application System

## Overview
The citizenship application system allows users to apply for citizenship via Discord. The bot collects application information, performs proxy verification, and posts applications to a transcript channel for review.

## How It Works

### For Users
Users start the application process with:
```
?citizenship
```

The bot will:
1. Send a DM asking for nation name, reason for applying, and additional information
2. Provide a verification link to check for proxies/VPNs
3. Confirm application submission
4. Post the application to the admin transcript channel

### For Admins

**Setup the System:**
```
?menu → Citizenship Applications → Setup Transcript Channel
```

This configures where applications are posted for review.

**Optional - Configure Proxy Checking:**
```
?menu → Setup & Settings → Setup Proxy Check API Key
```

Enter your proxycheck.io API key to enable automatic proxy/VPN detection.

**Manage Custom Questions:**
```
?manage_citizenship_questions
```

Opens an interactive menu where you can:
- Add new questions
- Edit existing questions
- Delete questions
- Add conditional questions (questions that appear based on previous answers)
- Reorder questions

**View All Questions:**
```
?list_citizenship_questions
```

Shows all current citizenship questions in order.

## Features

### 1. **Interactive Question Management**
Use the `?manage_citizenship_questions` menu to build custom application forms with:
- Text questions
- Yes/No questions
- Multiple choice
- Optional vs required questions
- Conditional logic (ask follow-up questions based on answers)

### 2. **Proxy/VPN Detection**
- Users receive a secure verification link
- Their IP is automatically checked against proxycheck.io
- Results included in application transcript
- Local secure server handles verification (no external hosting needed)

### 3. **Guild-Specific Forms**
- Each Discord server can have its own unique application form
- Questions are stored per-guild in the database
- Complete isolation between servers

### 4. **Response Tracking**
- All responses stored in database
- Admin review through transcript channel
- Applications include Discord user info, nation name, and all answers

## Database Schema

### citizenship_questions Table
Stores all application questions with conditional logic for each guild.

**Fields:**
- `id` - Question ID
- `guild_id` - Discord server ID
- `question_text` - The question to ask
- `question_type` - Type: text, yes_no, multiple_choice, optional
- `order` - Display order
- `parent_question_id` - For conditional questions
- `trigger_answer` - Answer that triggers this question

### citizenship_responses Table  
Stores all user responses to questions.

**Fields:**
- `id` - Response ID
- `application_id` - Links to application
- `question_id` - Which question was answered
- `response_text` - User's answer
- `responded_at` - Timestamp

### citizenship_applications Table
Stores complete applications including proxy check results.

**Fields:**
- `id` - Application ID
- `guild_id` - Discord server ID
- `discord_user_id` - Applicant's Discord ID
- `nation_name` - Their nation name
- `proxy_verification_status` - passed/failed/not_configured
- `ip_addresses` - Encrypted IP addresses
- `submitted_at` - Timestamp

## Example Workflow

1. Admin sets up transcript channel via menu
2. Admin creates custom questions via `?manage_citizenship_questions`
3. User runs `?citizenship`
4. Bot asks questions in DM (custom questions if configured, default questions otherwise)
5. Bot sends proxy verification link
6. User's IP is checked automatically
7. Complete application posted to transcript channel with all details
8. Admins review and make decision

## Tips for Admins

- Start with simple questions and add complexity as needed
- Use conditional questions for follow-ups (e.g., "If no, why not?")
- Configure proxy checking to detect VPN/proxy users
- Review applications promptly - users receive DM confirmation when submitted
- Transcript channel should be admin-only for privacy
