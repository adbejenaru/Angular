Hai să îmbunătățim aplicația ta **Student Management Application** conform cerințelor tale și să implementăm pas cu pas fiecare task.

---

### **Rezolvarea Task-urilor**

---

### **Task 1: Display Student List**

**Ce face?**  
Afișează lista de studenți utilizând un tabel și directivele `*ngFor` pentru a itera prin array-ul `students`.

Codul tău pentru acest task este deja implementat corect:

**`students.component.html`**:
```html
<h2>Student List</h2>
<table>
  <thead>
  <tr>
    <th>ID</th>
    <th>Name</th>
    <th>Email</th>
  </tr>
  </thead>
  <tbody>
  <tr *ngFor="let student of students">
    <td>{{ student.id }}</td>
    <td>{{ student.name }}</td>
    <td>{{ student.email }}</td>
  </tr>
  </tbody>
</table>
```

---

### **Task 2: Add New Student**

**Ce face?**  
Adaugă un formular pentru adăugarea unui student nou. Câmpurile formularului sunt legate de obiectul `newStudent` prin `[(ngModel)]`.

Formularul este corect configurat, dar să adăugăm și validarea unicității ID-ului (Task 4).

Modificăm `addStudent()` în `students.component.ts`:
```typescript
addStudent() {
  if (!this.newStudent.id || !this.newStudent.name || !this.newStudent.email) {
    alert('All fields are required!');
    return;
  }

  const duplicate = this.students.some(student => student.id === this.newStudent.id);
  if (duplicate) {
    alert('A student with this ID already exists!');
    return;
  }

  this.students.push({ ...this.newStudent });
  this.newStudent = { id: 0, name: '', email: '' }; // Reset form
}
```

---

### **Task 3: Reset Form After Adding**

Formularul este deja resetat prin atribuirea unui nou obiect gol în `addStudent()`:
```typescript
this.newStudent = { id: 0, name: '', email: '' }; // Reset form
```

---

### **Task 4: Validate Unique Student ID**

Validarea unicității ID-ului a fost adăugată în `addStudent()`:
```typescript
const duplicate = this.students.some(student => student.id === this.newStudent.id);
if (duplicate) {
  alert('A student with this ID already exists!');
  return;
}
```

---

### **Task 5: Use Interpolation**

Adăugăm un heading care afișează numărul total de studenți:
```html
<h2>Student List (Total: {{ students.length }})</h2>
```

---

### **Task 6: Highlight Empty Fields**

Folosim directiva `required` și un mesaj pentru validare.

Actualizăm formularul în `students.component.html`:
```html
<form (ngSubmit)="addStudent()">
  <div>
    <label>ID:</label>
    <input [(ngModel)]="newStudent.id" name="id" required />
    <span *ngIf="!newStudent.id && formSubmitted">ID is required!</span>
  </div>
  <div>
    <label>Name:</label>
    <input [(ngModel)]="newStudent.name" name="name" required />
    <span *ngIf="!newStudent.name && formSubmitted">Name is required!</span>
  </div>
  <div>
    <label>Email:</label>
    <input [(ngModel)]="newStudent.email" name="email" required />
    <span *ngIf="!newStudent.email && formSubmitted">Email is required!</span>
  </div>
  <button type="submit" (click)="formSubmitted = true">Add Student</button>
</form>
```

În `students.component.ts`, declarăm variabila `formSubmitted`:
```typescript
formSubmitted = false;
```

---

### **Task 7: Add Edit Functionality**

Adăugăm un buton "Edit" pentru fiecare student și populăm formularul cu detaliile studentului selectat.

1. **Adăugăm un buton în tabel:**
   ```html
   <tr *ngFor="let student of students">
     <td>{{ student.id }}</td>
     <td>{{ student.name }}</td>
     <td>{{ student.email }}</td>
     <td>
       <button (click)="editStudent(student)">Edit</button>
     </td>
   </tr>
   ```

2. **Implementăm funcția `editStudent()` în `students.component.ts`:**
   ```typescript
   editStudent(student: Student) {
     this.newStudent = { ...student };
   }
   ```

3. **Actualizăm `addStudent()` pentru a face diferența între adăugare și editare:**
   ```typescript
   addStudent() {
     if (!this.newStudent.id || !this.newStudent.name || !this.newStudent.email) {
       alert('All fields are required!');
       return;
     }

     const existingStudentIndex = this.students.findIndex(
       student => student.id === this.newStudent.id
     );

     if (existingStudentIndex !== -1) {
       this.students[existingStudentIndex] = { ...this.newStudent };
     } else {
       this.students.push({ ...this.newStudent });
     }

     this.newStudent = { id: 0, name: '', email: '' }; // Reset form
   }
   ```

---

### **Task 8: Add Delete Functionality**

1. **Adăugăm un buton "Delete" în tabel:**
   ```html
   <td>
     <button (click)="deleteStudent(student.id)">Delete</button>
   </td>
   ```

2. **Implementăm funcția `deleteStudent()` în `students.component.ts`:**
   ```typescript
   deleteStudent(id: number) {
     this.students = this.students.filter(student => student.id !== id);
   }
   ```

---

### **Bonus: Stilizare Tabel**

Adăugăm câteva stiluri pentru tabel în `students.component.css`:
```css
table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 10px;
  text-align: left;
  border: 1px solid #ddd;
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
```

---

### **Rezultatul Final**

1. Aplicația afișează lista de studenți.
2. Poți adăuga un student nou, cu validare pentru câmpuri goale și ID duplicat.
3. Formularul este resetat după adăugare.
4. Poți edita detaliile unui student sau să-l ștergi.

Dacă mai ai nevoie de explicații sau alte funcționalități, spune-mi! 😊
