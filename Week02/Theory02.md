### **Constructor în Angular**

În Angular, **constructorul** este o metodă specială dintr-o clasă TypeScript care este apelată atunci când se creează o instanță a clasei. În contextul componentelor Angular, constructorul este folosit pentru:
1. **Inițializarea clasei componente sau directivei**.
2. **Injectarea dependențelor** prin mecanismul de Dependency Injection (DI) al Angular.

---

### **Exemplu Simplu de Constructor în Angular**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  template: `<p>Exemplu componentă</p>`
})
export class ExampleComponent {
  constructor() {
    console.log('Constructor: Componentă creată');
  }
}
```

**Ce face constructorul:**
- Constructorul este apelat automat de Angular când instanțiază componenta.
- Poți folosi constructorul pentru a inițializa proprietăți sau pentru a apela metode.

---

### **Dependency Injection în Angular**

**Dependency Injection (DI)** este un design pattern care permite gestionarea dependențelor (servicii, clase, sau obiecte) și furnizarea acestora către componente, directive sau alte servicii, fără a le crea manual.

În Angular, DI:
1. **Asigură reutilizarea și gestionarea ușoară a dependențelor**.
2. **Îmbunătățește testabilitatea** prin înlocuirea dependențelor reale cu versiuni mock.
3. Este **gestionat de injectorul Angular**.

#### **Cum Funcționează DI în Angular**
- Angular folosește un **injector** care creează și gestionează instanțele obiectelor declarate în sistemul său de DI.
- Obiectele sunt declarate în `providers` și pot fi injectate în constructor folosind un decorator precum `@Injectable`.

---

### **Exemplu de Dependency Injection**

#### Serviciu pentru furnizarea datelor
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root', // Asigură că serviciul este disponibil la nivel global
})
export class DataService {
  getData(): string {
    return 'Datele de la serviciu';
  }
}
```

#### Componentă care folosește serviciul
```typescript
import { Component } from '@angular/core';
import { DataService } from './data.service'; // Importul serviciului

@Component({
  selector: 'app-data-binding',
  template: `<p>{{ data }}</p>`
})
export class DataBindingComponent {
  data: string;

  // Constructorul injectează DataService
  constructor(private dataService: DataService) {
    this.data = this.dataService.getData(); // Folosim serviciul pentru a obține date
  }
}
```

**Explicație:**
1. **Serviciul `DataService`** este marcat cu `@Injectable` și declarat în modulul principal sau folosește `providedIn: 'root'` pentru a fi disponibil global.
2. **Componentele care au nevoie de serviciu** îl primesc automat prin constructor, folosind cuvântul cheie `private` sau `public` pentru a defini variabila de clasă.

---

### **Avantajele Dependency Injection**
1. **Decuplare**: Componentele nu trebuie să creeze manual dependențele, ceea ce duce la o separare clară între componente și serviciile utilizate.
2. **Reutilizare**: Aceeași instanță a serviciului poate fi partajată între mai multe componente.
3. **Testabilitate**: Dependențele reale pot fi înlocuite cu versiuni mock pentru testare.
4. **Gestionare centralizată**: DI ajută la gestionarea mai bună a ciclului de viață al serviciilor.

---

### **Când Să Folosești Constructorul și DI?**

- **Constructor**: Folosit pentru a inițializa proprietăți și pentru a primi dependențe.
- **Dependency Injection**: Folosit pentru a furniza servicii externe, clase de utilități sau alte resurse necesare unei componente.

---

**Exemplu Complet**:

```typescript
@Injectable({
  providedIn: 'root',
})
export class LoggerService {
  log(message: string): void {
    console.log('LoggerService:', message);
  }
}

@Component({
  selector: 'app-example',
  template: `<p>Exemplu cu DI</p>`
})
export class ExampleComponent {
  constructor(private logger: LoggerService) {
    this.logger.log('Componentă creată!');
  }
}
```

**Rezultat:**
În consolă:
```plaintext
LoggerService: Componentă creată!
```
