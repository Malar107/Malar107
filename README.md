import json
from datetime import datetime

class Task:
    def __init__(self, name, due_date, priority, description=''):
        self.name = name
        self.due_date = due_date
        self.priority = priority
        self.description = description
        self.completed = False

    def __repr__(self):
        status = 'Completed' if self.completed else 'Pending'
        return f"{self.name} - Due: {self.due_date} - Priority: {self.priority} - {status}"

class ToDoList:
    def __init__(self):
        self.tasks = []

    def add_task(self, name, due_date, priority, description=''):
        task = Task(name, due_date, priority, description)
        self.tasks.append(task)
        print(f"Task '{name}' added.")

    def view_tasks(self, sort_by='due_date'):
        if sort_by == 'due_date':
            tasks = sorted(self.tasks, key=lambda x: datetime.strptime(x.due_date, '%Y-%m-%d'))
        elif sort_by == 'priority':
            tasks = sorted(self.tasks, key=lambda x: x.priority)
        else:
            tasks = self.tasks
        for task in tasks:
            print(task)

    def update_task(self, task_name, **kwargs):
        task = next((t for t in self.tasks if t.name == task_name), None)
        if task:
            task.name = kwargs.get('name', task.name)
            task.due_date = kwargs.get('due_date', task.due_date)
            task.priority = kwargs.get('priority', task.priority)
            task.description = kwargs.get('description', task.description)
            print(f"Task '{task_name}' updated.")
        else:
            print(f"Task '{task_name}' not found.")

    def delete_task(self, task_name):
        task = next((t for t in self.tasks if t.name == task_name), None)
        if task:
            self.tasks.remove(task)
            print(f"Task '{task_name}' deleted.")
        else:
            print(f"Task '{task_name}' not found.")

    def mark_as_completed(self, task_name):
        task = next((t for t in self.tasks if t.name == task_name), None)
        if task:
            task.completed = True
            print(f"Task '{task_name}' marked as completed.")
        else:
            print(f"Task '{task_name}' not found.")

    def save_tasks(self, file_name='tasks.json'):
        with open(file_name, 'w') as file:
            tasks_data = [task.__dict__ for task in self.tasks]
            json.dump(tasks_data, file)
        print("Tasks saved.")

    def load_tasks(self, file_name='tasks.json'):
        try:
            with open(file_name, 'r') as file:
                tasks_data = json.load(file)
                self.tasks = [Task(**task) for task in tasks_data]
            print("Tasks loaded.")
        except FileNotFoundError:
            print("No saved tasks found.")

# Example usage:
if __name__ == '__main__':
    todo_list = ToDoList()
    todo_list.add_task('Buy groceries', '2024-07-14', 1)
    todo_list.add_task('Complete project', '2024-07-13', 2, 'Finish the to-do list project')
    todo_list.view_tasks()
    todo_list.mark_as_completed('Buy groceries')
    todo_list.view_tasks()
    todo_list.save_tasks()
    todo_list.load_tasks()<!---
Malar107/Malar107 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
