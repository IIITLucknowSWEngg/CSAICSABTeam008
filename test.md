# Test Results for Concord

### Project Name: Concord
### Description: Concord is a real-time communication platform inspired by Discord, focusing on scalability, modularity, and secure communication.

---

## Test Results

### 1. User Registration
#### Description: A user signs up on Concord by providing valid details.

| Test Case       | Input                     | Expected Output                                                                 | Result                                   |
|------------------|---------------------------|---------------------------------------------------------------------------------|-----------------------------------------|
| **Happy Path**   | Valid email, username, password | User is successfully registered and redirected to the homepage.                 | **Pass**: User registered successfully. |
| **Error Path**   | Invalid email format      | Registration fails, displaying an error message: "Invalid email format."        | **Pass**: Error message displayed.      |
| **Abused Path**  | SQL injection in username or email | Registration fails, logging the attack attempt and showing "Invalid input detected." | **Pass**: Input sanitized, attack prevented. |

---

### 2. User Login
#### Description: A registered user logs into Concord.

| Test Case       | Input                     | Expected Output                                                                 | Result                                   |
|------------------|---------------------------|---------------------------------------------------------------------------------|-----------------------------------------|
| **Happy Path**   | Correct email and password | User is successfully logged in and redirected to the dashboard.                 | **Pass**: User logged in successfully.  |
| **Error Path**   | Incorrect password        | Login fails, displaying an error message: "Incorrect password."                 | **Pass**: Error message displayed.      |
| **Abused Path**  | Brute-force login attempts | User account is locked after 5 failed attempts, and an alert is sent to the user’s email. | **Fail**: Account not locked; refer Bug ID: #CONCORD-101. |

---

### 3. Create Server
#### Description: A user creates a new server on Concord.

| Test Case       | Input                     | Expected Output                                                                 | Result                                   |
|------------------|---------------------------|---------------------------------------------------------------------------------|-----------------------------------------|
| **Happy Path**   | Valid server name and optional icon | Server is successfully created and added to the user’s server list.             | **Pass**: Server created successfully.  |
| **Error Path**   | Empty server name         | Creation fails, displaying an error message: "Server name cannot be empty."     | **Pass**: Error message displayed.      |
| **Abused Path**  | Very long server name (>100 characters) | Creation fails, showing "Server name exceeds character limit."                  | **Pass**: Error handled correctly.      |

---

### 4. Join Server
#### Description: A user joins an existing server using an invite link.

| Test Case       | Input                     | Expected Output                                                                 | Result                                   |
|------------------|---------------------------|---------------------------------------------------------------------------------|-----------------------------------------|
| **Happy Path**   | Valid invite link         | User successfully joins the server and is added to its member list.             | **Pass**: User joined server successfully. |
| **Error Path**   | Expired invite link       | Join request fails, showing "Invite link has expired or is invalid."            | **Pass**: Error message displayed.      |
| **Abused Path**  | Spamming invalid links    | Account is flagged for suspicious behavior after multiple failed attempts, limiting further requests. | **Pass**: Spamming detected and flagged. |

---

### 5. Send Message
#### Description: A user sends a text message in a server channel.

| Test Case       | Input                     | Expected Output                                                                 | Result                                   |
|------------------|---------------------------|---------------------------------------------------------------------------------|-----------------------------------------|
| **Happy Path**   | Valid text message        | Message is successfully sent and appears in the channel.                        | **Pass**: Message sent successfully.    |
| **Error Path**   | Empty text message        | Message fails to send, displaying "Message cannot be empty."                    | **Pass**: Error message displayed.      |
| **Abused Path**  | Sending spam messages     | User is temporarily muted if spamming behavior is detected (e.g., sending 10+ messages in a second). | **Pass**: User muted for spamming.      |

---

### 6. Voice/Video Call
#### Description: Users participate in a voice or video call within a server.

| Test Case       | Input                     | Expected Output                                                                 | Result                                   |
|------------------|---------------------------|---------------------------------------------------------------------------------|-----------------------------------------|
| **Happy Path**   | Valid call initiation     | Call is successfully initiated, and participants can join.                      | **Pass**: Call initiated successfully.  |
| **Error Path**   | Invalid microphone access | Call fails, showing "Unable to access microphone. Please check your settings."  | **Pass**: Error message displayed.      |
| **Abused Path**  | Flooding call requests    | User is temporarily blocked from initiating calls after multiple spam attempts. | **Pass**: User blocked for spamming.    |

---

### 7. Manage Channels
#### Description: Admins create or delete channels in a server.

| Test Case       | Input                     | Expected Output                                                                 | Result                                   |
|------------------|---------------------------|---------------------------------------------------------------------------------|-----------------------------------------|
| **Happy Path**   | Valid channel name        | Channel is successfully created or deleted.                                     | **Pass**: Channel created successfully. |
| **Error Path**   | Empty channel name        | Operation fails, showing "Channel name cannot be empty."                        | **Pass**: Error message displayed.      |
| **Abused Path**  | Repeatedly deleting channels | Admin is warned or temporarily restricted from performing actions after excessive misuse. | **Pass**: Abuse detected and restricted.|

---

### 8. Push Notifications
#### Description: Notify users about messages or mentions.

| Test Case       | Input                     | Expected Output                                                                 | Result                                   |
|------------------|---------------------------|---------------------------------------------------------------------------------|-----------------------------------------|
| **Happy Path**   | Valid mention in a message | User receives a notification with the correct content and timestamp.            | **Pass**: Notification received.        |
| **Error Path**   | Notification delivery failure | Notification fails, logging the issue and retrying delivery.                    | **Pass**: Issue logged and retry successful. |
| **Abused Path**  | Excessive notifications (spam mentions) | User is muted or restricted if excessive mentions are detected.                  | **Pass**: User muted for spam mentions. |

---

### 9. User Moderation
#### Description: Admins manage user permissions or ban users.

| Test Case       | Input                     | Expected Output                                                                 | Result                                   |
|------------------|---------------------------|---------------------------------------------------------------------------------|-----------------------------------------|
| **Happy Path**   | Valid ban or permission change | User permissions are updated, or the user is banned successfully.              | **Pass**: Moderation actions successful.|
| **Error Path**   | Attempt to ban server owner | Operation fails, showing "Server owners cannot be banned."                     | **Pass**: Error message displayed.      |
| **Abused Path**  | Frequent unjustified bans | Admin privileges are revoked or restricted temporarily after excessive misuse.  | **Pass**: Abuse detected and privileges revoked.|

---

## Summary of Results
- **Total Test Cases**: 27  
- **Passed**: 26  
- **Failed**: 1  
  - Bug ID: #CONCORD-101 (Account lockout not triggered after multiple failed login attempts)
