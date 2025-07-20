# Password-Generator
# project 2
import tkinter as tk
from tkinter import messagebox
import random
import string

try:
    import pyperclip  # Copy to clipboard
except ImportError:
    import os
    os.system("pip install pyperclip")
    import pyperclip


# Function to generate password
def generate_password():
    length = length_var.get()
    use_upper = upper_var.get()
    use_lower = lower_var.get()
    use_digits = digit_var.get()
    use_symbols = symbol_var.get()

    if not (use_upper or use_lower or use_digits or use_symbols):
        messagebox.showwarning("Warning", "Please select at least one character type!")
        return

    characters = ""
    if use_upper:
        characters += string.ascii_uppercase
    if use_lower:
        characters += string.ascii_lowercase
    if use_digits:
        characters += string.digits
    if use_symbols:
        characters += string.punctuation

    password = ''.join(random.choices(characters, k=length))
    result_var.set(password)


# Function to copy password
def copy_to_clipboard():
    password = result_var.get()
    if password:
        pyperclip.copy(password)
        messagebox.showinfo("Copied", "Password copied to clipboard!")


# GUI Window Setup
root = tk.Tk()
root.title("Password Generator - Internship Project")
root.geometry("500x420")
root.resizable(False, False)
root.configure(bg="#f0f0f0")

# Variables
length_var = tk.IntVar(value=12)
upper_var = tk.BooleanVar(value=True)
lower_var = tk.BooleanVar(value=True)
digit_var = tk.BooleanVar(value=True)
symbol_var = tk.BooleanVar(value=True)
result_var = tk.StringVar()

# Title
tk.Label(root, text="🔐 Password Generator", font=("Helvetica", 18, "bold"), bg="#f0f0f0").pack(pady=10)

# Length
tk.Label(root, text="Select Password Length", font=("Arial", 12), bg="#f0f0f0").pack()
tk.Scale(root, from_=4, to=32, orient="horizontal", variable=length_var, length=300).pack()

# Options
tk.Checkbutton(root, text="Include Uppercase Letters (A-Z)", variable=upper_var, font=("Arial", 11), bg="#f0f0f0").pack(anchor="w", padx=40)
tk.Checkbutton(root, text="Include Lowercase Letters (a-z)", variable=lower_var, font=("Arial", 11), bg="#f0f0f0").pack(anchor="w", padx=40)
tk.Checkbutton(root, text="Include Numbers (0-9)", variable=digit_var, font=("Arial", 11), bg="#f0f0f0").pack(anchor="w", padx=40)
tk.Checkbutton(root, text="Include Symbols (!@#$)", variable=symbol_var, font=("Arial", 11), bg="#f0f0f0").pack(anchor="w", padx=40)

# Generate Button
tk.Button(root, text="Generate Password", font=("Arial", 12), bg="#4CAF50", fg="white", command=generate_password).pack(pady=12)

# Result Display
tk.Entry(root, textvariable=result_var, font=("Arial", 14), justify="center", state="readonly", width=35).pack(pady=5)

# Copy Button
tk.Button(root, text="Copy to Clipboard", font=("Arial", 11), bg="#2196F3", fg="white", command=copy_to_clipboard).pack(pady=5)

# Run App
root.mainloop()
