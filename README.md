import os

# File to store tasks
TASKS_FILE = "tasks.txt"

# Load existing tasks from file or create an empty list if the file does not exist
def load_tasks():
    if os.path.exists(TASKS_FILE):
        with open(TASKS_FILE, "r") as file:
            tasks = []
            for line in file:
                description, status = line.strip().split(" - ")
                tasks.append({"description": description, "completed": status == "Completed"})
            return tasks
    return []

# Save tasks to file
def save_tasks(tasks):
    with open(TASKS_FILE, "w") as file:
        for task in tasks:
            status = "Completed" if task["completed"] else "Pending"
            file.write(f"{task['description']} - {status}\n")

# Display all tasks
def view_tasks(tasks):
    if not tasks:
        print("No tasks found.")
        return
    for idx, task in enumerate(tasks, 1):
        status = "Completed" if task["completed"] else "Pending"
        print(f"{idx}. {task['description']} - {status}")

# Add a new task
def add_task(tasks):
    description = input("Enter task description: ")
    tasks.append({"description": description, "completed": False})
    save_tasks(tasks)
    print("Task added successfully.")

# Update a task
def update_task(tasks):
    view_tasks(tasks)
    try:
        task_number = int(input("Enter the task number to update: ")) - 1
        if 0 <= task_number < len(tasks):
            new_description = input("Enter new task description: ")
            tasks[task_number]["description"] = new_description
            save_tasks(tasks)
            print("Task updated successfully.")
        else:
            print("Invalid task number.")
    except ValueError:
        print("Please enter a valid number.")

# Mark a task as completed
def complete_task(tasks):
    view_tasks(tasks)
    try:
        task_number = int(input("Enter the task number to mark as completed: ")) - 1
        if 0 <= task_number < len(tasks):
            tasks[task_number]["completed"] = True
            save_tasks(tasks)
            print("Task marked as completed.")
        else:
            print("Invalid task number.")
    except ValueError:
        print("Please enter a valid number.")

# Delete a task
def delete_task(tasks):
    view_tasks(tasks)
    try:
        task_number = int(input("Enter the task number to delete: ")) - 1
        if 0 <= task_number < len(tasks):
            tasks.pop(task_number)
            save_tasks(tasks)
            print("Task deleted successfully.")
        else:
            print("Invalid task number.")
    except ValueError:
        print("Please enter a valid number.")

# Main menu function
def main():
    tasks = load_tasks()
    
    while True:
        print("\nTo-Do List Application")
        print("1. View Tasks")
        print("2. Add Task")
        print("3. Update Task")
        print("4. Mark Task as Completed")
        print("5. Delete Task")
        print("6. Exit")

        choice = input("Enter your choice (1-6): ")

        if choice == "1":
            view_tasks(tasks)
        elif choice == "2":
            add_task(tasks)
        elif choice == "3":
            update_task(tasks)
        elif choice == "4":
            complete_task(tasks)
        elif choice == "5":
            delete_task(tasks)
        elif choice == "6":
            print("Exiting the application. Goodbye!")
            break
        else:
            print("Invalid choice. Please select a valid option.")

if __name__ == "__main__":
    main()