# Todo-list-manager
#This is the project where we can create our own todo list using python programming
import os

# Function to add a task
def add_task(tasks, name, description, priority='low'):
    tasks.append({'name': name, 'description': description, 'priority': priority, 'status': 'pending'})

# Function to delete a task
def delete_task(tasks, name):
    for task in tasks:
        if task['name'] == name:
            tasks.remove(task)
            print(f"Task '{name}' deleted.")
            return
    print(f"Task '{name}' not found.")

# Function to update task details
def update_task(tasks, name, description=None, priority=None, status=None):
    for task in tasks:
        if task['name'] == name:
            if description:
                task['description'] = description
            if priority:
                task['priority'] = priority
            if status:
                task['status'] = status
            print(f"Task '{name}' updated.")
            return
    print(f"Task '{name}' not found.")

# Function to view tasks
def view_tasks(tasks, status=None, priority=None):
    filtered_tasks = []
    for task in tasks:
        if (not status or task['status'] == status) and (not priority or task['priority'] == priority):
            filtered_tasks.append(task)
    if filtered_tasks:
        print("Tasks:")
        for task in filtered_tasks:
            print(f"- Name: {task['name']}, Description: {task['description']}, Priority: {task['priority']}, Status: {task['status']}")
    else:
        print("No tasks found.")

# Function to save tasks to a file
def save_tasks_to_file(tasks, filename='tasks.txt'):
    with open(filename, 'w') as f:
        for task in tasks:
            f.write(f"{task['name']},{task['description']},{task['priority']},{task['status']}\n")
    print(f"Tasks saved to {filename}.")

# Function to load tasks from a file
def load_tasks_from_file(filename='tasks.txt'):
    tasks = []
    if os.path.exists(filename):
        with open(filename, 'r') as f:
            for line in f:
                name, description, priority, status = line.strip().split(',')
                tasks.append({'name': name, 'description': description, 'priority': priority, 'status': status})
        print(f"Tasks loaded from {filename}.")
    else:
        print(f"{filename} not found. Starting with an empty task list.")
    return tasks

# Command-line interface
def cli():
    tasks = load_tasks_from_file()  # Load tasks from file on startup
    while True:
        print("\nEnter command (add, delete, update, view, save, exit):")
        command = input().strip().lower()

        if command == 'add':
            name = input("Enter task name: ").strip()
            description = input("Enter task description: ").strip()
            priority = input("Enter task priority (low/medium/high): ").strip().lower()
            add_task(tasks, name, description, priority)
        
        elif command == 'delete':
            name = input("Enter task name to delete: ").strip()
            delete_task(tasks, name)
        
        elif command == 'update':
            name = input("Enter task name to update: ").strip()
            description = input("Enter new description (leave blank to keep current): ").strip()
            priority = input("Enter new priority (low/medium/high, leave blank to keep current): ").strip().lower()
            status = input("Enter new status (pending/completed, leave blank to keep current): ").strip().lower()
            update_task(tasks, name, description, priority, status)
        
        elif command == 'view':
            status = input("Filter by status (pending/completed/all, leave blank for all): ").strip().lower()
            priority = input("Filter by priority (low/medium/high, leave blank for all): ").strip().lower()
            view_tasks(tasks, status=status, priority=priority)
        
        elif command == 'save':
            save_tasks_to_file(tasks)
        
        elif command == 'exit':
            save_tasks_to_file(tasks)
            break
        
        else:
            print("Invalid command. Please try again.")

# Main function
if __name__ == '__main__':
    cli()
