import os
import json
import webbrowser
from datetime import datetime

MEMORY_FILE = "assistant_memory.json"

def load_memory():
    if not os.path.exists(MEMORY_FILE):
        return {}
    with open(MEMORY_FILE, "rr") as f:
        return json.load(f)

def save_memory(memory):
    with open(MEMORY_FILE, "w") as f:
        json.dump(memory, f, indent=4)

def show_time():
    now = datetime.now()
    print("Current Time:", now.strftime("%Y-%m-%d %H:%M:%S"))

def open_website(command):
    site = command.replace("open ", "").strip()
    if not site.startswith("http"):
        site = "https://" + site
    webbrowser.open(site)
    print(f"Opening {site}...")

def create_folder(command):
    folder_name = command.replace("create folder ", "").strip()
    os.makedirs(folder_name, exist_ok=True)
    print(f"Folder '{folder_name}' created.")

def remember_note(command, memory):
    note = command.replace("remember ", "").strip()
    memory["note"] = note
    save_memory(memory)
    print("I will remember that.")

def recall_note(memory):
    note = memory.get("note")
    if note:
        print("You told me to remember:", note)
    else:
        print("I don't remember anything yet.")

def assistant():
    print("=== AI Terminal Assistant ===")
    print("Type 'exit' to quit.\n")

    memory = load_memory()

    while True:
        command = input("You: ").lower()

        if command == "exit":
            print("Goodbye 👋")
            break
        elif "time" in command:
            show_time()
        elif command.startswith("open "):
            open_website(command)
        elif command.startswith("create folder "):
            create_folder(command)
        elif command.startswith("remember "):
            remember_note(command, memory)
        elif "recall" in command:
            recall_note(memory)
        else:
            print("I don't understand that command.")

if __name__ == "__main__":
    assistant()
