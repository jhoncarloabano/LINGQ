using System;
using System.Collections.Generic;
using System.Linq;

public class EmployeeManagementSystem
{
    private List<Employee> employees = new List<Employee>();

    // Create: Add a new employee
    public void Create(Employee employee)
    {
        if (employees.Any(e => e.Id == employee.Id))
        {
            throw new ArgumentException("An employee with the same ID already exists.");
        }
        employees.Add(employee);
    }

    // Read: List all employees
    public List<Employee> ReadAll()
    {
        return employees;
    }

    // Read: Find an employee by ID
    public Employee ReadById(int id)
    {
        var employee = employees.FirstOrDefault(e => e.Id == id);
        if (employee == null)
        {
            throw new KeyNotFoundException("Employee not found.");
        }
        return employee;
    }

    // Update: Update an existing employee's details by ID
    public void Update(int id, Employee updatedEmployee)
    {
        var employee = ReadById(id);
        employee.Name = updatedEmployee.Name;
        employee.Position = updatedEmployee.Position;
        employee.Salary = updatedEmployee.Salary;
    }

    // Delete: Remove an employee by ID
    public void Delete(int id)
    {
        var employee = ReadById(id);
        employees.Remove(employee);
    }
}
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Position { get; set; }
    public decimal Salary { get; set; }

    public override string ToString()
    {
        return $"Id: {Id}, Name: {Name}, Position: {Position}, Salary: {Salary:C}";
    }
}
public class Program
{
    public static void Main(string[] args)
    {
        var employeeManagementSystem = new EmployeeManagementSystem();
        bool running = true;

        while (running)
        {
            Console.WriteLine("\nEmployee Management System");
            Console.WriteLine("1. Add Employee");
            Console.WriteLine("2. List Employees");
            Console.WriteLine("3. Find Employee by ID");
            Console.WriteLine("4. Update Employee");
            Console.WriteLine("5. Delete Employee");
            Console.WriteLine("6. Exit");
            Console.Write("Choose an option: ");

            string choice = Console.ReadLine();

            switch (choice)
            {
                case "1":
                    // Add Employee
                    var newEmployee = new Employee();
                    Console.Write("Enter Employee ID: ");
                    newEmployee.Id = int.Parse(Console.ReadLine());
                    Console.Write("Enter Employee Name: ");
                    newEmployee.Name = Console.ReadLine();
                    Console.Write("Enter Employee Position: ");
                    newEmployee.Position = Console.ReadLine();
                    Console.Write("Enter Employee Salary: ");
                    newEmployee.Salary = decimal.Parse(Console.ReadLine());
                    try
                    {
                        employeeManagementSystem.Create(newEmployee);
                        Console.WriteLine("Employee added successfully.");
                    }
                    catch (ArgumentException ex)
                    {
                        Console.WriteLine(ex.Message);
                    }
                    break;

                case "2":
                    // List Employees
                    Console.WriteLine("\nAll Employees:");
                    foreach (var emp in employeeManagementSystem.ReadAll())
                    {
                        Console.WriteLine(emp);
                    }
                    break;

                case "3":
                    // Find Employee by ID
                    Console.Write("Enter Employee ID to find: ");
                    int findId = int.Parse(Console.ReadLine());
                    try
                    {
                        var foundEmployee = employeeManagementSystem.ReadById(findId);
                        Console.WriteLine("Found Employee: " + foundEmployee);
                    }
                    catch (KeyNotFoundException ex)
                    {
                        Console.WriteLine(ex.Message);
                    }
                    break;

                case "4":
                    // Update Employee
                    Console.Write("Enter Employee ID to update: ");
                    int updateId = int.Parse(Console.ReadLine());
                    var updatedEmployee = new Employee();
                    Console.Write("Enter new Employee Name: ");
                    updatedEmployee.Name = Console.ReadLine();
                    Console.Write("Enter new Employee Position: ");
                    updatedEmployee.Position = Console.ReadLine();
                    Console.Write("Enter new Employee Salary: ");
                    updatedEmployee.Salary = decimal.Parse(Console.ReadLine());
                    try
                    {
                        employeeManagementSystem.Update(updateId, updatedEmployee);
                        Console.WriteLine("Employee updated successfully.");
                    }
                    catch (KeyNotFoundException ex)
                    {
                        Console.WriteLine(ex.Message);
                    }
                    break;

                case "5":
                    // Delete Employee
                    Console.Write("Enter Employee ID to delete: ");
                    int deleteId = int.Parse(Console.ReadLine());
                    try
                    {
                        employeeManagementSystem.Delete(deleteId);
                        Console.WriteLine("Employee deleted successfully.");
                    }
                    catch (KeyNotFoundException ex)
                    {
                        Console.WriteLine(ex.Message);
                    }
                    break;

                case "6":
                    // Exit
                    running = false;
                    Console.WriteLine("Exiting the Employee Management System.");
                    break;

                default:
                    Console.WriteLine("Invalid option. Please choose a valid option.");
                    break;
            }
        }
    }
}
