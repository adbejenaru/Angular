### **Task: Move Logic from `StudentsComponent` to `StudentService`**

---

#### **Objective:**
Refactor the `StudentsComponent` to delegate all logic related to managing the students list (e.g., adding students) to the `StudentService`. This ensures better separation of concerns and reusability.

---

### **Step-by-Step Guide**

#### **Step 1: Identify Logic to Move**
The following logic from the `StudentsComponent` should be moved to the `StudentService`:
1. Adding a new student, including:
   - Validation (check for empty fields and duplicate IDs).
   - Adding the student to the `students` array.

#### **Step 2: Create Methods in the Service**
Move the logic for adding students to the `StudentService`. Update the service to include the following methods:

##### **Service Code (`StudentService`):**
```typescript
import { Injectable } from '@angular/core';
import {Student} from './student';
import {HttpClient} from '@angular/common/http';
import {Observable} from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class StudentService {
  private students: Student[] = [
    {id: 1, name: 'Vasile', email: 'vasile@gmail.com'},
    {id: 2, name: 'Virginia', email: 'virginia@gmail.com'},
  ];

  // Retrieve all students
  getStudents(): Student[] {
    return [...this.students]; // Return a copy to prevent direct modification
  }

  // Add a new student
  addStudent(newStudent: Student): void {
    newStudent.id = Number(newStudent.id);

    if (!newStudent.id || !newStudent.name || !newStudent.email) {
      alert('All fields are required!');
      return;
    }

    const duplicate = this.students.some(student => student.id === newStudent.id);
    if (duplicate) {
      alert('A student with this ID already exists!');
      return;
    }

    this.students.push({...newStudent});
    alert('Student added successfully!');
  }
}
```

---

#### **Step 3: Refactor the Component**
Update the `StudentsComponent` to delegate all logic to the service.

##### **Component Code (`StudentsComponent`):**
```typescript
import {Component, OnInit} from '@angular/core';
import {Student} from '../student';
import {StudentService} from '../student.service';

@Component({
  selector: 'app-students',
  standalone: false,

  templateUrl: './students.component.html',
  styleUrl: './students.component.css'
})
export class StudentsComponent implements OnInit {
  students: Student[] = [];
  newStudent: Student = {id: 0, name: '', email: ''};
  editStudent: Student | null = null; // Holds student being edited

  constructor(private studentService: StudentService) {
  }

  ngOnInit(): void {
    this.students = this.studentService.getStudents();
  }

  addStudent(): void {
    this.studentService.addStudent(this.newStudent);
    this.students = this.studentService.getStudents(); // Refresh the list
    this.newStudent = {id: 0, name: '', email: ''}; // Reset the form
  }
}
```

---

#### **Step 4: Update the Template**
Update the template to handle error messages dynamically. No changes are needed for the form itself, but you should add a section to display errors.

##### **Template Code (`students.component.html`):**
```html
<h2>Student List</h2>
<table>
  <thead>
  <tr>
    <th>ID</th>
    <th>Name</th>
    <th>Email</th>
    <th>Actions</th>
  </tr>
  </thead>
  <tbody>
  <tr *ngFor="let student of students">
    <td>{{ student.id }}</td>
    <td>{{ student.name }}</td>
    <td>{{ student.email }}</td>
    <td>
      <button (click)="startEdit(student)">Edit</button>
      <button (click)="deleteStudent(student.id)">Delete</button>
    </td>
  </tr>
  </tbody>
</table>


<h3 >Add New Student</h3>
<form (ngSubmit)="addStudent()">
  <label>
    ID: <input [(ngModel)]="newStudent.id" name="id" type="number" required>
  </label>
  <label>
    Name: <input [(ngModel)]="newStudent.name" name="name" required>
  </label>
  <label>
    Email: <input [(ngModel)]="newStudent.email" name="email" required>
  </label>
  <button type="submit">Add Student</button>
</form>


```

---

#### **Step 5: Test the Changes**

1. **Initial State:**
   - The table should display the initial hardcoded students.

2. **Add a Valid New Student:**
   - Fill in all fields with valid data.
   - The student should be added, and the table should update.

3. **Add a Student with a Duplicate ID:**
   - Enter an ID that already exists.
   - An error message should appear: `A student with this ID already exists!`

4. **Add a Student with Missing Fields:**
   - Leave one or more fields empty.
   - An error message should appear: `All fields are required.`

---

#### **Benefits of This Refactor**
1. **Separation of Concerns:**
   - The service handles all the logic for managing students, making it reusable.
   - The component focuses only on user interaction and UI updates.

2. **Code Reusability:**
   - The `StudentService` can now be reused in other components if needed.

3. **Improved Maintainability:**
   - Any updates to the student management logic (e.g., validation) only need to be made in the service.

---

Let me know if you need further guidance! 😊
