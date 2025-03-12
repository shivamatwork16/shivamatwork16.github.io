+++
date = '2025-03-12T17:04:24+05:30'
draft = false
title = 'Send Mails with CSV Data'
pin = true
+++

## 📝 CSV Email Sender System - 🚀

<!--more-->
Welcome to the **CSV Email Sender System**! This system is designed to process CSV files, send emails to users based on the data, and handle scheduling and uniqueness constraints. Below is a detailed explanation of how the system works, key features, and the logic behind it.

---

## 🎯 **Purpose**
The system:
1. **Processes CSV files** containing user data (emails, names, etc.).
2. **Sends emails** to users based on the provided template and SMTP settings.
3. **Handles scheduling** for email sending using the `csv_send_at` field.
4. **Ensures uniqueness** of CSV data using a combination of `pageSettingId` and `file_hash`.

---

## 🛠️ **Key Features**
1. **CSV Processing**:
   - Fetches and parses CSV files from a URL.
   - Converts CSV data into a structured format for email sending.

2. **Email Sending**:
   - Sends emails to users using a predefined template.
   - Tracks email sending status (`first_sent`, `second_sent`, `last_sent`).

3. **Scheduling**:
   - Uses the `csv_send_at` field to schedule email sending.
   - Sends emails immediately if `csv_send_at` is not set or invalid.

4. **Uniqueness Handling**:
   - Ensures that the combination of `pageSettingId` and `file_hash` is unique.
   - Prevents duplicate entries for the same CSV data under the same `pageSettingId`.

5. **Background Processing**:
   - Uses a cron job helper to handle email sending in the background.

---

## 🧠 **How It Works**

### 1. **CSV Fetching and Parsing**
   - The system fetches the CSV file from the provided URL (`invited_csv`).
   - The CSV file is parsed into a structured format (e.g., an array of user objects).

   ```javascript
   const csvResponse = await axios.get(csvUrl, { responseType: "stream" });
   const users = await parseCsv(csvResponse.data);
   ```

---

### 2. **Email Sending Logic**
   - Emails are sent to users who haven't received them yet (`first_sent` is `null`).
   - The email payload includes:
     - Template ID
     - User email, first name, and last name
     - SMTP settings
     - Logo and other metadata.

   ```javascript
   const emailpayload = {
     templateId: pageSetting.email_invite_template,
     email: user.email,
     firstName: user.first_name,
     lastName: user.last_name,
     smtpId: pageSetting.reviewInviteSmtpId,
     addedBy: pageSetting.addedBy,
     logo: pageSetting.logo,
     pageSettingId: pageSetting._id,
   };
   await ReviewEmails.reviewCsvInvite(emailpayload);
   ```

---

### 3. **Scheduling with `csv_send_at`**
   - The system checks if the `csv_send_at` field is set and valid.
   - If `csv_send_at` is in the future, email sending is delayed until the specified time.
   - If `csv_send_at` is not set or invalid, emails are sent immediately.

   ```javascript
   if (csvSendAtDate && currentTime < csvSendAtDate) {
     console.warn(`Scheduled time for sending emails (${csvSendAtDate}) has not been reached yet. Skipping.`);
     return;
   }
   ```

---

### 4. **Uniqueness Handling**
   - The system generates a SHA-256 hash of the CSV data (`file_hash`).
   - It ensures that the combination of `pageSettingId` and `file_hash` is unique.
   - If a record with the same combination exists, the system skips creating a new entry.

   ```javascript
   const fileHash = crypto.createHash("sha256").update(JSON.stringify(users)).digest("hex");

   const existingHashRecord = await db.reviewinvitecsv.findOne({
     pageSettingId: pageSetting._id,
     file_hash: fileHash,
   });

   if (existingHashRecord) {
     console.warn(`A record with the same pageSettingId and file_hash already exists. Skipping creation.`);
   }
   ```

---

### 5. **Background Processing**
   - The system uses a cron job helper to handle email sending in the background.
   - This ensures that the main thread is not blocked during email sending.

   ```javascript
   const cronJobHelper = (task, delay = 0) => {
     if (delay > 0) {
       setTimeout(task, delay);
     } else {
       setImmediate(task);
     }
   };
   ```

---

## 🗂️ **Database Schema**
The system uses the following schema for storing CSV data:

```javascript
const reviewInviteCsvSchema = new Schema(
  {
    file_url: { type: String, required: true },
    file_hash: { type: String, required: true },
    users: [
      {
        email: { type: String, required: true },
        first_name: { type: String, required: true },
        last_name: { type: String, required: true },
        first_sent: { type: Date }, // first invite email
        second_sent: { type: Date }, // reminder one email
        last_sent: { type: Date }, // reminder two email
      },
    ],
    addedBy: { type: Schema.Types.ObjectId, ref: "users", index: true },
    pageSettingId: {
      type: Schema.Types.ObjectId,
      ref: "reviewpagesetting",
      index: true,
      required: true,
    },
  },
  { timestamps: true },
);

// Compound unique index for pageSettingId and file_hash
reviewInviteCsvSchema.index(
  { pageSettingId: 1, file_hash: 1 },
  { unique: true },
);
```

---

## 🚦 **Error Handling**
- The system logs errors for:
  - Missing CSV URLs.
  - Invalid `csv_send_at` dates.
  - CSV fetching or parsing failures.
  - Database operation failures.

   ```javascript
   catch (err) {
     console.error("Error processing CSV and sending emails:", err);
   }
   ```

---

## 🛑 **Edge Cases Handled**
1. **Missing CSV URL**:
   - The system logs a warning and exits if no CSV URL is provided.

2. **Invalid `csv_send_at` Date**:
   - If the date is invalid, the system treats it as `null` and sends emails immediately.

3. **Duplicate CSV Data**:
   - The system skips creating duplicate entries for the same `pageSettingId` and `file_hash`.

4. **Background Processing Failures**:
   - The cron job helper ensures that email sending runs in the background without blocking the main thread.

---

## 🧰 **Dependencies**
1. **Node.js Libraries**:
   - `axios`: For fetching CSV files.
   - `crypto`: For generating SHA-256 hashes.
   - `node-cron`: For scheduling background tasks.

2. **Database**:
   - MongoDB with Mongoose for schema and query handling.

---

## 🚀 **How to Use**
1. **Set Up**:
   - Install dependencies:
     ```bash
     npm install axios crypto node-cron
     ```
   - Configure your MongoDB connection.

2. **Run the System**:
   - Call the `processCsvAndSendEmails` function with the appropriate `pageSetting` object.

3. **Monitor Logs**:
   - Check the console logs for warnings, errors, and success messages.

---

## 📊 **Example Workflow**
1. **Input**:
   - `pageSetting` object with:
     - `invited_csv`: CSV URL.
     - `csv_send_at`: Scheduled time (optional).
     - `email_invite_template`: Email template ID.
     - `reviewInviteSmtpId`: SMTP settings.

2. **Process**:
   - Fetch and parse CSV.
   - Generate `file_hash`.
   - Check for existing records.
   - Send emails based on scheduling.

3. **Output**:
   - Emails sent to users.
   - Database updated with email sending status.
