**Description**
  This is a Python script that automates the login process for multiple Skype accounts using Selenium and Tkinter. The script reads credentials from a text file, launches a new Chrome instance for each account, and logs in to Skype using the provided email and password.

**Features**
  - Supports multiple account login using a single text file
  - Uses Selenium for web automation and Tkinter for GUI
  - Handles timeouts and retries login attempts if necessary
  - Closes the browser window if the account requires verification

**Usage**
  - Create a text file containing your Skype credentials in the format email:password per line.
  - Run the script and select the text file when prompted.
  - The script will launch a new Chrome instance for each account and log in to Skype.

**Requirements**
  - Python 3.x
  - Selenium
  - Tkinter
  - Chrome browser (make sure ChromeDriver is installed and in your system's PATH)

**Known Issues**
  - The script may not work if Skype changes its login page layout or functionality.
  - The script does not handle cases where the account is locked or requires additional verification steps.

**Author**
Muhammad El-Shaikh
