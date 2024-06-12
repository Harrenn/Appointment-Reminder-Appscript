# Appointment Reminder Email Script

This script sends reminder emails to people based on appointment data stored in a Google Sheet. It reads the data from two separate sheets - one containing the appointment details, and another containing the email message templates.

## How it works 

1. The script accesses the active Google Spreadsheet.

2. It gets the "datas" sheet containing the appointment details.

3. The appointment data is read from the 2nd row onwards, spanning 7 columns - Name, Email, Vaccine, Date, Time, Message Type, Email Sent. 

4. The script then accesses the "Messages" sheet containing the email templates. 

5. The message templates are mapped to their corresponding message types for easy lookup later.

6. The script iterates through each appointment data row. 

7. For each row, it checks if a reminder email has already been sent for that appointment. 

8. It also checks if there is a valid message type defined for that appointment.

9. If above conditions are met, it calculates the date timestamp for 2 weeks before the appointment.

10. If current date falls within 2 weeks of appointment, the script sends a reminder email.

11. It replaces placeholders in the template with actual data - Vaccine, Date, Time. 

12. Email is sent to the address specified in the "datas" sheet.

13. That row is marked as email sent in the sheet after sending email.

14. The script logs confirmation that the email has been sent successfully.

## Automation

The script can be automated to run daily using time-driven triggers in Apps Script.

## Prerequisites

- Google account 
- Google Sheet with "datas" and "Messages" sheets
- "datas" sheet should have the necessary appointment columns
- Templates defined in "Messages" sheet

## Notes

- Appointment data starts from 2nd row
- Adjust sheet names and ranges as per your data

