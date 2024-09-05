import tkinter as tk
from tkinter import filedialog
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException, NoSuchElementException
import threading
import time

def extract_credentials(file_path):
    credentials = []
    try:
        with open(file_path, 'r') as f:
            for line in f:
                email, password = line.strip().split(':')
                credentials.append((email, password))
    except Exception as e:
        print(f"Failed to extract credentials: {e}")
    return credentials

def log_credentials(email, status):
    with open("login_status.log", "a") as log_file:
        log_file.write(f"{email} - {status}\n")

class SkypeLogin:
    def __init__(self, email, password):
        options = webdriver.ChromeOptions()
        options.add_argument("--incognito")
        options.add_experimental_option("detach", True)  # Ensure browser window stays open
        self.driver = webdriver.Chrome(options=options)
        self.email = email
        self.password = password

    def login(self):
        try:
            self.driver.get("https://login.live.com/login.srf?...")  # URL shortened for readability
            WebDriverWait(self.driver, 30).until(EC.element_to_be_clickable((By.ID, "i0116"))).send_keys(self.email)
            self.driver.find_element(By.ID, "idSIButton9").click()
            
            password_field = WebDriverWait(self.driver, 30).until(EC.element_to_be_clickable((By.ID, "i0118")))
            password_field.send_keys(self.password)
            password_field.send_keys(u'\ue007')  # Send Enter key

            time.sleep(2)  # Wait for the "Yes" button to appear
            try:
                self.driver.find_element(By.XPATH, "//button[contains(text(), 'Yes')]").click()
            except NoSuchElementException:
                print(f"Could not find 'Yes' button for account {self.email}.")
                
            try:
                WebDriverWait(self.driver, 10).until(EC.element_to_be_clickable((By.XPATH, "//button[contains(text(), 'Verify your account')]")))
                log_credentials(self.email, "Verification needed")
            except TimeoutException:
                log_credentials(self.email, "Login successful")
        
        except TimeoutException as e:
            print(f"TimeoutException occurred: {e}")
            log_credentials(self.email, "Login failed")

# Global list to keep track of SkypeLogin instances
skype_logins = []

def login_thread(skype_login):
    skype_login.login()
    skype_logins.append(skype_login)

def select_file():
    file_path = filedialog.askopenfilename(title="Select a file", filetypes=[("Text files", "*.txt")])
    if file_path:
        credentials = extract_credentials(file_path)
        for email, password in credentials:
            print(f"Logging in to Skype with {email}...")
            skype_login = SkypeLogin(email, password)
            threading.Thread(target=login_thread, args=(skype_login,)).start()

def main():
    global root
    root = tk.Tk()
    root.title("Skype Login")
    root.withdraw()  # Hide the main window
    print("Select a file to start logging in...")
    select_file()
    
    # Print a message indicating that the script has finished processing
    print("The script has completed. The browser windows will remain open.")
    
    root.mainloop()

if __name__ == "__main__":
    main()
