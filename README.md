# Appointment Reminder Email Script

This script is designed to send reminder emails to people based on appointment data stored in a Google Sheet. The script reads the data from the sheet, checks if reminder emails need to be sent, and sends personalized emails to the respective recipients.

## How it works

1. The script starts by accessing the active Google Spreadsheet and the specific sheet containing the appointment data.

2. It defines the range of data to be read from the sheet, which includes columns for Name, Email, Vaccine, Date, Time, Message Type, and Email Sent status.

3. The script then fetches the message templates from a separate range in the sheet. These templates are stored with their corresponding message types.

4. It iterates over each row of appointment data in the sheet.

5. For each row, the script checks if an email has already been sent and if there is a valid message type associated with the appointment.

6. If the conditions are met, the script calculates the timestamp for 2 weeks before the appointment date.

7. If the current date is within 2 weeks of the appointment, the script proceeds to send a reminder email.

8. It personalizes the email message by replacing placeholders in the template with actual appointment details such as Vaccine, Date, and Time.

9. The script sends the personalized email to the recipient using the email address specified in the sheet.

10. After sending the email, the script marks the "Email Sent" column as true for that particular row in the sheet.

11. The script logs a message indicating that the email has been sent to the recipient.

## Automation

This script can be set to run automatically on a daily basis using a time-driven trigger in Google Apps Script. This ensures that reminder emails are sent consistently without manual intervention.

## Prerequisites

To use this script, you need:

- A Google account
- Access to Google Sheets and the specific sheet containing the appointment data
- The sheet should have columns for Name, Email, Vaccine, Date, Time, Message Type, and Email Sent
- Message templates should be defined in a separate range within the sheet, with message types and their corresponding messages

## Note

Make sure to replace "Sheet1" in the script with the actual name of your sheet.

Adjust the range for message templates based on your sheet's layout.

The script assumes that the appointment data starts from the second row of the sheet.
