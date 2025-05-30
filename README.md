using System;

namespace ToDoList
{
    class Task
    {
        private string description;
        private string name;
        private int index;
        private string date;
        private string status;

        public Task(string description, string name, int index, string date)
        {
            this.description = description;
            this.name = name;
            this.index = index;
            this.date = date;
            this.status = "To Do";  
        }

        public string Description { get => description; set => description = value; }
        public string Name { get => name; set => name = value; }
        public int Index { get => index; set => index = value; }
        public string Date { get => date; set => date = value; }
        public string Status { get => status; set => status = value; }
    }

    class Node
    {
        internal Task TaskData { get; set; }
        internal Node Next { get; set; }

        public Node(Task task)
        {
            TaskData = task;
            Next = null;
        }
    }

    class TaskLinkedList
    {
        private Node head;
        private Node tail;

        internal Node Head { get => head; set => head = value; }
        internal Node Tail { get => tail; set => tail = value; }

        public TaskLinkedList()
        {
            Head = null;
            Tail = null;
        }

        public void InsertSorted(Task task)
        {
            Node newNode = new Node(task);

            if (Head == null || task.Date.CompareTo(Head.TaskData.Date) < 0)
            {
                newNode.Next = Head;
                Head = newNode;
                if (Tail == null) Tail = newNode;
                return;
            }

            Node current = Head;
            while (current.Next != null && task.Date.CompareTo(current.Next.TaskData.Date) > 0)
            {
                current = current.Next;
            }

            newNode.Next = current.Next;
            current.Next = newNode;

            if (newNode.Next == null)
                Tail = newNode;
        }

        public void Push(Task task)
        {
            task.Status = "In Progress";
            Node newNode = new Node(task);
            newNode.Next = Head;
            Head = newNode;

            if (Tail == null) Tail = newNode;
        }

        public Task Pop()
        {
            if (Head == null)
                return null;

            Task task = Head.TaskData;
            Head = Head.Next;

            if (Head == null) Tail = null;

            task.Status = "Completed";
            return task;
        }

        public Task RemoveFirst()  
        {
            if (Head == null)
                return null;

            Task task = Head.TaskData;
            Head = Head.Next;

            if (Head == null) Tail = null;

            return task;
        }

        public void Enqueue(Task task)
        {
            Node newNode = new Node(task);

            if (Head == null)
            {
                Head = newNode;
                Tail = newNode;
            }
            else
            {
                Tail.Next = newNode;
                Tail = newNode;
            }
        }

        public void Display()
        {
            Node current = Head;
            while (current != null)
            {
                Console.WriteLine($"ID: {current.TaskData.Index}, Name: {current.TaskData.Name}, Status: {current.TaskData.Status}, Date: {current.TaskData.Date}");
                current = current.Next;
            }
        }
    }

    class Program
    {
        static void Main()
        {
            TaskLinkedList toDoList = new TaskLinkedList();
            TaskLinkedList inProgressList = new TaskLinkedList();
            TaskLinkedList completedList = new TaskLinkedList();

            Task task1 = new Task("First Task", "Task A", 1, "2025-06-01");
            Task task2 = new Task("Second Task", "Task B", 2, "2025-05-15");
            Task task3 = new Task("Third Task", "Task C", 3, "2025-05-30");

            toDoList.InsertSorted(task1);
            toDoList.InsertSorted(task2);
            toDoList.InsertSorted(task3);

            Console.WriteLine("To-Do List:");
            toDoList.Display();

            Task taskToProgress = toDoList.RemoveFirst();  
            inProgressList.Push(taskToProgress);

            Console.WriteLine();
            inProgressList.Display();

            Task taskCompleted = inProgressList.Pop();
            completedList.Enqueue(taskCompleted);

            Console.WriteLine();
            completedList.Display();
        }
    }
}
