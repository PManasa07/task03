# task03
import tkinter as tk
from tkinter import ttk
import random
import string

class PasswordGeneratorApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Password Generator")

        # Create a frame for the UI elements
        self.frame = ttk.Frame(self.root, padding="10")
        self.frame.grid(row=0, column=0, sticky=(tk.W, tk.E, tk.N, tk.S))

        # Labels and Entry for username and password length
        self.username_label = ttk.Label(self.frame, text="Username:")
        self.username_label.grid(row=0, column=0, padx=5, pady=5)
        
        self.username_entry = ttk.Entry(self.frame)
        self.username_entry.grid(row=0, column=1, padx=5, pady=5)
        
        self.length_label = ttk.Label(self.frame, text="Password Length:")
        self.length_label.grid(row=1, column=0, padx=5, pady=5)
        
        self.length_entry = ttk.Entry(self.frame)
        self.length_entry.grid(row=1, column=1, padx=5, pady=5)
        
        # Button to generate password
        self.generate_button = ttk.Button(self.frame, text="Generate Password", command=self.generate_password)
        self.generate_button.grid(row=2, column=0, columnspan=2, pady=10)

        # Display area for the generated password
        self.password_label = ttk.Label(self.frame, text="Generated Password:")
        self.password_label.grid(row=3, column=0, columnspan=2, pady=5)

        self.password_display = ttk.Label(self.frame, text="")
        self.password_display.grid(row=4, column=0, columnspan=2, pady=5)

        # Buttons for username acceptance and reset
        self.accept_button = ttk.Button(self.frame, text="Accept", command=self.accept_credentials)
        self.accept_button.grid(row=5, column=0, pady=10)

        self.reset_button = ttk.Button(self.frame, text="Reset", command=self.reset_credentials)
        self.reset_button.grid(row=5, column=1, pady=10)

    def generate_password(self):
        try:
            username = self.username_entry.get()
            if not username:
                raise ValueError("Please enter a username.")
            
            length = int(self.length_entry.get())
            if length <= 0:
                raise ValueError("Password length must be a positive integer.")
            
            password = self.generate_random_password(length)
            self.password_display.config(text=f"Username: {username}, Password: {password}")
        except ValueError as ve:
            self.password_display.config(text=str(ve))

    def generate_random_password(self, length):
        characters = string.ascii_letters + string.digits + string.punctuation
        password = ''.join(random.choice(characters) for _ in range(length))
        return password

    def accept_credentials(self):
        credentials = self.password_display.cget("text")
        if credentials:
            print(credentials)
        else:
            print("Please generate a password first.")

    def reset_credentials(self):
        self.username_entry.delete(0, tk.END)
        self.length_entry.delete(0, tk.END)
        self.password_display.config(text="")

def main():
    root = tk.Tk()
    app = PasswordGeneratorApp(root)
    root.mainloop()

if __name__ == "__main__":
    main()
