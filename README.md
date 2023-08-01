# Smart Waiver Project
**Project Overview**

This is an automation project I performed as volunteer work for non-profit organization Santa Cruz Mountains Trail Stewardship (SCMTS). SCMTS is a non-profit trail stewardship organization located in Santa Cruz County (CA). A major way they maintain trails is by leading volunteer events at local and State parks located throughout the county. 

Volunteer events are often run multiple times per week and require participants to sign unique waivers depending on the project site. The event size can range from 3 - 500+ volunteers depending on the type of event being run. Volunteer coordinators from SCMTS have to manually check an internal "Smart Waiver" database system that contains which volunteers have signed waivers and then email participants to get them to sign the waiver prior to the event. ***Therefore, the purpose of this project was to save SCMTS staff hours of time each week by creating an automated system to (1) check which volunteers have signed waivers and (2) email the volunteers which have not yet signed waivers.***    

### Resources
[smart_waiver.ipynb](smart_waiver.ipynb) - Contains code to check a CSV file from Smart Waiver for participants that signed waivers. If a name is found in the Smart Waiver CSV file, a box is checked in the SCMTS google sheet for that event. It also generates a new CSV file containing a list of participants that have not signed waivers (unmatched_names.csv). 

[smart_gmail.ipynb](smart_gmail.ipynb) - SCMTS staff can draft an email in a google doc file with a link to the events waiver. The code in this file will then check the names and emails found in the unmatched_names.csv file (this is the output CSV file generated from smart_waiver.ipynb) for volunteers that have not signed the waiver for the event. This code will then send emails to all participants in the unmatched_names.csv using the google_doc text as the subject and body for the email.

### Smart Waiver Notebook

#### Description:
This notebook provides a means to quickly check which volunteers have up-to-date waivers. It reads volunteer waiver data from a CSV file, compares it to data in a Google Sheet, and updates the Sheet by "checking off" volunteers who have signed the waivers. It also creates a CSV file for volunteers who did not sign, which is used for the automated email system set up in smart_gmail.ipynb.

#### Dependencies:
- pandas
- gspread
- oauth2client
- datetime

#### Instructions:
1. **Service Account Setup:**
   - Go to the [Google Developers Console](https://console.developers.google.com/).
   - Create a project, enable the Google Sheets API, and create credentials for a service account.
   - Download the JSON key file for the service account.
   - Share the target Google Sheet with the email address in your service account JSON file.

2. **Configuration:**
   - Update the `key_file_path` variable with the path to your service account JSON file.
   - Update the `sheet_name` variable with the name of your Google Sheet.
   - Place the smart waiver CSV file in your directory and update the `smartwaiver` variable with the file name.

3. **Running the Notebook:**
   - Run the entire notebook to process the waivers and update the Google Sheet.

#### Code Explanation:
- The code first authorizes access to Google Sheets.
- Reads volunteer waiver data from a given CSV file.
- Compares the waiver data with the existing data in the Google Sheet.
- Updates the Sheet by checking off volunteers who have signed the waivers in the past year.
- Records volunteers who did not sign the waiver and exports them to a CSV.

### Smart Gmail Notebook

#### Description:
This notebook automates sending personalized emails to volunteers who have not signed the waiver. It reads the email content from a Google Doc, retrieves the recipients' email addresses and names from a CSV file, and sends out personalized emails.

#### Dependencies:
- os
- base64
- pandas
- google-auth, google-auth-oauthlib, google-auth-httplib2
- google-api-python-client

#### Instructions:
1. **Google API Setup:**
   - Follow the [Python Quickstart](https://developers.google.com/gmail/api/quickstart/python) for the Gmail API.
   - Enable the Google Docs API as well.
   - Download the `credentials.json` file.

2. **Configuration:**
   - Update the `document_id` variable with the ID of your Google Doc containing the email template.
   - Place the CSV file with unmatched names and emails in your directory or use the CSV generated from the Smart Waiver Notebook.

3. **Running the Notebook:**
   - Run the entire notebook.
   - The first time you run it, you'll be prompted to authenticate with your Google account.

#### Code Explanation:
- The code first authenticates and establishes a connection to Gmail and Google Docs.
- It reads the email template (subject and body) from a specified Google Doc.
- It reads the names and emails from the specified CSV file.
- Sends personalized emails to each recipient in the CSV.

#### Email Template Format:
The Google Doc containing the email template must include specific keywords:
- `email_subject` - line that precedes the subject text.
- `email_body` - line that precedes the body text.
- `{name}` - placeholder in the body text where the recipient's name will be inserted.

### Final Notes:
- Both notebooks are designed to work together, with the Smart Waiver Notebook identifying the volunteers who haven't signed the waiver, and the Smart Gmail Notebook sending them email reminders.
- Ensure that you adhere to all relevant privacy and permission requirements when handling personal data.

By following the instructions and understanding the code explanations, users should be able to successfully set up and utilize this system using their Google accounts and personalized information.
