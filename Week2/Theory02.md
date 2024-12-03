![image](https://github.com/user-attachments/assets/8b516f28-eae3-4a91-9ca0-bf211b5a0f63)


---

### **1. Ce este un Observable?**
Un **Observable** este un model folosit în Angular pentru a reprezenta fluxuri de date care pot fi consumate pe măsură ce ele devin disponibile. Observables sunt o parte importantă a RxJS (Reactive Extensions for JavaScript), o bibliotecă pentru programare reactivă.

Un **Observable** emite date în timp (sincron sau asincron), iar un **Observer** este un „abonat” care „ascultă” și reacționează la aceste date.

---

### **Explicația exemplului**

#### **a) Serviciu (`Service`)**
Codul din serviciu:
```typescript
getMovies(): Observable<Movie[]> {
  return this.http.get<Movie[]>(this.moviesUrl);
}
```

- **Observable<Movie[]>**: Această metodă returnează un **Observable** care emite un array de obiecte de tip `Movie`. Acest lucru înseamnă că datele nu sunt disponibile imediat, ci vor fi furnizate când serverul răspunde la cererea HTTP.
- **this.http.get<Movie[]>**: Angular utilizează `HttpClient` pentru a efectua apeluri HTTP. Acest apel returnează un Observable pe care componenta îl poate consuma ulterior.
- `this.moviesUrl`: URL-ul către endpoint-ul API de unde se obțin datele.

#### **b) Componentă (`Component`)**
Codul din componentă:
```typescript
this.movieService.getMovies().subscribe(
  (movies: Movie[]) => this.movies = movies,
  (error: any) => this.errorMessage = <any>error
);
```

1. **Subscribe**:
   - `this.movieService.getMovies()` apelează metoda din serviciu, care returnează un Observable.
   - Metoda `.subscribe()` este utilizată pentru a „asculta” datele emise de Observable.
   - Parametrii lui `subscribe`:
     - Primul parametru: o funcție callback care primește datele atunci când Observable emite valori (în acest caz, lista de filme).
     - Al doilea parametru: o funcție callback care gestionează erorile (de exemplu, dacă apelul HTTP eșuează).

2. **Procesare date**:
   - `this.movies = movies`: Datele recepționate (array-ul de filme) sunt salvate într-o variabilă locală `movies` pentru a fi afișate în interfață.
   - `this.errorMessage = <any>error`: Dacă apare o eroare, aceasta este stocată într-o variabilă pentru a fi afișată utilizatorului.

---

### **Fluxul de date**
1. Componenta apelează metoda `getMovies()` din serviciu.
2. Serviciul efectuează un apel HTTP folosind `HttpClient` și returnează un **Observable**.
3. Componenta se **abonează** (`subscribe`) la Observable.
4. Când serverul răspunde, Observable emite datele:
   - Dacă răspunsul este de succes, lista de filme este salvată în `this.movies`.
   - Dacă răspunsul eșuează, mesajul de eroare este salvat în `this.errorMessage`.

---

### **De ce folosim Observables?**
1. **Asincronism**: Apelurile HTTP sunt asincrone, iar Observables facilitează gestionarea acestui comportament.
2. **Flexibilitate**: Observables oferă operatori pentru transformarea, combinarea sau filtrarea datelor.
3. **Comportament reactiv**: Permite reactualizarea automată a datelor în interfață pe măsură ce ele devin disponibile.

---

### **Pe scurt:**
- **Observable**: Flux de date care emite valori în timp.
- **Observer**: Cod care se abonează la fluxul de date pentru a reacționa la acestea.
- **Subscribe**: Permite observatorului să recepționeze datele și să gestioneze eventualele erori.

Acest exemplu demonstrează cum se utilizează Observables pentru a accesa date dintr-un API și pentru a le afișa în interfață, gestionând atât succesul, cât și erorile.
