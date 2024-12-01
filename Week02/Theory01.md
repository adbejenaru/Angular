În Angular, **lifecycle hooks** (momentele ciclului de viață) sunt metode speciale care permit intervenția în anumite etape din ciclul de viață al unei componente sau directive. Aceste hook-uri sunt esențiale pentru a controla modul în care componenta este creată, actualizată sau distrusă.

Iată o explicație detaliată pentru fiecare dintre cele mai importante **lifecycle hooks**:

---

### **1. `ngOnInit`**
- **Când se apelează**: O singură dată, imediat după ce Angular a terminat inițializarea componentelor și a legăturilor datelor (`data binding`).
- **Rol**: Este folosit pentru inițializarea logicii componentei sau pentru a aduce date dintr-un serviciu.
- **Exemplu**:
  ```typescript
  ngOnInit(): void {
    console.log('Componenta a fost inițializată!');
    this.loadData();
  }
  ```

---

### **2. `ngOnChanges`**
- **Când se apelează**: De fiecare dată când se modifică una dintre proprietățile decorate cu `@Input`.
- **Rol**: Permite detectarea modificărilor venite de la componentele părinte.
- **Exemplu**:
  ```typescript
  @Input() data: string;

  ngOnChanges(changes: SimpleChanges): void {
    console.log('S-au schimbat datele:', changes.data.currentValue);
  }
  ```

---

### **3. `ngDoCheck`**
- **Când se apelează**: De fiecare dată când Angular verifică modificările în componentă (în timpul procesului de detectare a schimbărilor - *change detection*).
- **Rol**: Permite implementarea logicii personalizate pentru a observa schimbări pe care Angular nu le detectează automat.
- **Exemplu**:
  ```typescript
  ngDoCheck(): void {
    console.log('Se execută detectarea modificărilor.');
  }
  ```

---

### **4. `ngAfterContentInit`**
- **Când se apelează**: După ce Angular a introdus conținut în interiorul componentei, în cazul folosirii `<ng-content>`.
- **Rol**: Este util pentru a interacționa cu conținutul proiectat.
- **Exemplu**:
  ```typescript
  ngAfterContentInit(): void {
    console.log('Conținutul proiectat a fost inițializat.');
  }
  ```

---

### **5. `ngAfterContentChecked`**
- **Când se apelează**: După ce Angular a verificat conținutul proiectat.
- **Rol**: Este folosit pentru a observa dacă există schimbări ulterioare în conținutul proiectat.
- **Exemplu**:
  ```typescript
  ngAfterContentChecked(): void {
    console.log('Conținutul proiectat a fost verificat.');
  }
  ```

---

### **6. `ngAfterViewInit`**
- **Când se apelează**: După ce Angular a inițializat complet vizualizarea componentei și a tuturor componentelor copil.
- **Rol**: Util pentru a interacționa cu elementele DOM ale componentei după ce acestea au fost randate.
- **Exemplu**:
  ```typescript
  @ViewChild('inputElement') input: ElementRef;

  ngAfterViewInit(): void {
    this.input.nativeElement.focus();
    console.log('Vizualizarea componentei a fost inițializată.');
  }
  ```

---

### **7. `ngAfterViewChecked`**
- **Când se apelează**: După fiecare verificare a vizualizării componentei și a copiilor săi.
- **Rol**: Este folosit pentru a verifica dacă vizualizarea este sincronizată cu starea componentei.
- **Exemplu**:
  ```typescript
  ngAfterViewChecked(): void {
    console.log('Vizualizarea componentei a fost verificată.');
  }
  ```

---

### **8. `ngOnDestroy`**
- **Când se apelează**: Imediat înainte ca Angular să distrugă componenta.
- **Rol**: Este folosit pentru curățarea resurselor, dezabonarea de la observabile sau eliminarea ascultătorilor de evenimente.
- **Exemplu**:
  ```typescript
  ngOnDestroy(): void {
    console.log('Componenta este pe cale să fie distrusă.');
  }
  ```

---

### **Ordinea Apelării Lifecycle Hooks**
Ciclul de viață al unei componente Angular urmează această ordine:
1. **`ngOnChanges`** (dacă există proprietăți `@Input`)
2. **`ngOnInit`**
3. **`ngDoCheck`**
4. **`ngAfterContentInit`**
5. **`ngAfterContentChecked`**
6. **`ngAfterViewInit`**
7. **`ngAfterViewChecked`**
8. **`ngOnDestroy`**

---

### **Cele Mai Utilizate Lifecycle Hooks**
1. **`ngOnInit`** - Pentru inițializarea logicii și încărcarea datelor.
2. **`ngOnDestroy`** - Pentru curățarea resurselor.
3. **`ngOnChanges`** - Pentru a reacționa la schimbările proprietăților `@Input`.
4. **`ngAfterViewInit`** - Pentru a interacționa cu elementele DOM după ce vizualizarea a fost randată.

Acestea sunt folosite cel mai frecvent pentru că răspund la cele mai comune scenarii din viața unei componente.
