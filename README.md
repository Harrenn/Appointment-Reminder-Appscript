# Appointment-Reminder-Appscript
 Automated Email Reminder Script

This guide explains how to set up an automated email reminder script using Google Sheets and Google Apps Script. This script sends reminder emails to individuals based on data in a Google Sheet, two weeks before their scheduled appointment.

 Table of Contents
1. [Introduction](introduction)
2. [Setup Instructions](setup-instructions)
3. [Understanding the Google Sheet Structure](understanding-the-google-sheet-structure)
4. [Script Explanation](script-explanation)
5. [Running the Script](running-the-script)
6. [Setting Up an Automatic Trigger](setting-up-an-automatic-trigger)

 Introduction

This automated email reminder script reads data from a Google Sheet, checks for upcoming appointments, and sends a reminder email to the specified recipient 14 days before the appointment. After sending the email, it updates the sheet to mark the email as sent.

 Setup Instructions

 1. Create and Prepare the Google Sheet

1. Open Google Sheets.
2. Create a new sheet or use an existing one.
3. Rename the sheet to `"Copy of Prototype Appointment Automated Reminder Sheet"` for this example.

 2. Inserting Data and Templates

Use the following structure for your data:

 Main Data (starting from Row 2)
| Name       | Email                   | Vaccine | Date      | Time    | Message Type | Message sent? |
|------------|-------------------------|---------|-----------|---------|--------------|---------------|
| John Smith | john.smith@example.com  | MMR     | 6/22/2024 | 9:00 AM | Reminder1    | FALSE         |

 Message Templates (e.g., in columns I and J starting from Row 2)
|  I         | J                                                                                                |
|------------|--------------------------------------------------------------------------------------------------|
| Reminder1  | Hello [Name], we'd like to remind you of your appointment for [Vaccine] on [Date] at [Time].     |

 3. Open Apps Script

1. Open the Google Sheet.
2. Go to Extensions > Apps Script.
3. Copy and paste the script provided below into the script editor.

 Script Explanation

Below is the script with detailed comments explaining each part of the code:

/
  This script sends reminder emails to people based on data stored in a Google Sheet.
  It sends emails 2 weeks before their appointment date and marks the email as sent.
  The script can be set to run automatically on a daily basis using a time-driven trigger.
 /

function sendReminderEmails() {
  // Get the active Google Spreadsheet and the specific sheet by name
  const spreadsheet = SpreadsheetApp.getActiveSpreadsheet();
  const sheetName = "Copy of Prototype Appointment Automated Reminder Sheet";
  const sheet = spreadsheet.getSheetByName(sheetName); // Replace "sheetName" with your actual sheet name

  // Define the range of data to be read from the sheet
  // The data starts from the second row, first column, and spans 7 columns (Name, Email, Vaccine, Date, Time, Message Type, Email Sent)
  const dataRange = sheet.getRange(2, 1, sheet.getLastRow() - 1, 7);
  const data = dataRange.getValues(); // Get all data in this range

  // Fetch the message templates from the specified range
  // Message types and their corresponding messages are in columns I and J
  const messageTypeRange = sheet.getRange("I2:J10"); // Adjust this range based on your actual data layout
  const messageTypes = messageTypeRange.getValues().reduce((acc, val) => {
    if (val[0]) {
      acc[val[0]] = val[1]; // Store message type and its message in an object for easy lookup
    }
    return acc;
  }, {});

  // Iterate over each row of data
  for (let i = 0; i < data.length; i++) {
    // Destructure the row data into individual variables
    const [name, email, vaccine, dateStr, time, messageType, emailSent] = data[i];
    const date = new Date(dateStr); // Convert the date string to a Date object

    // Check if the email has not been sent yet and there is a valid message type
    if (!emailSent && messageType && messageTypes[messageType]) {
      // Calculate the timestamp for 2 weeks before the appointment date
      const reminderTimestamp = date.getTime() - (3.6e6  24  14); // 3.6e6 is milliseconds per hour, 24 hours, 14 days

      // Check if the current date is past the reminder timestamp (i.e., within 2 weeks before the appointment)
      if (Date.now() > reminderTimestamp) {
        // Replace placeholders in the message template with actual data
        // [Name]: Name, [Vaccine]: Vaccine, [Date]: Date, [Time]: Time
        let message = messageTypes[messageType]
          .replace("[Name]", name)
          .replace("[Vaccine]", vaccine)
          .replace("[Date]", date.toLocaleDateString())
          .replace("[Time]", time);

        // Send the email with the personalized message
        GmailApp.sendEmail(email, "Appointment Reminder", message);

        // Mark the email as sent in the sheet
        sheet.getRange(i + 2, 7).setValue(true); // i + 2 to account for header row and zero-based index
        Logger.log(`Email sent to {email}.`);
      }
    }
  }
}

 Understanding the Script:

- Initialization:
  - The script starts by getting the active Google Spreadsheet and the specific sheet where the data is located.

- Reading Data:
  - It defines the range of data to be read, starting from the second row since the first row typically contains headers.

- Fetching Templates:
  - The script fetches email message templates from a specified range and stores them in an object for easy lookup.

- Processing Each Row:
  - For each row, it checks if the email has been sent and if the message type is valid. 
  - It calculates the timestamp for two weeks before the appointment.
  - If the current date is within two weeks of the appointment, it personalizes the message using placeholders and sends the email.
  - It then marks the email as sent in the sheet and logs the action.

 Running the Script

1. Open the Google Sheets with your data.
2. Go to Extensions > Apps Script.
3. Replace any existing code with the provided script.
4. Click the play button to run the script.

 Setting Up an Automatic Trigger

To automatically run the script daily:

1. Go to the Apps Script Editor.
2. Click on the clock icon (Triggers) in the left sidebar.
3. Click on + Add Trigger.
4. Choose the `sendReminderEmails` function.
5. Set it to run on a daily schedule.

This will ensure the script runs daily, checking for any reminder emails to be sent.
