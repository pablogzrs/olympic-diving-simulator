# 🏊 International Diving Competition Simulator

C++ simulation of an Olympic-style diving competition with probabilistic scoring, difficulty-dependent performance, and automated medal determination for 5 athletes.

## 📋 Overview

Simulates 5 rounds of a diving competition. Each dive draws a difficulty rating, generates a score from a 7-judge panel, and produces a round score. After all rounds, totals are compared and gold, silver, and bronze are assigned.

## ✨ Features

- **5-Round Competition** — fixed tournament length
- **Difficulty-Dependent Scoring** — harder dives shift the score distribution downward
- **7-Judge Panel** — judge 1 is drawn independently; judges 2–7 deviate from judge 1
- **Automated Medal System** — gold, silver, bronze by total score
- **Round-by-Round Score Tables** — per-athlete breakdown

## 🏅 Competitors

| Athlete | Country | Code |
|---------|---------|------|
| Alejandra Orozco | 🇲🇽 México | AO |
| Paola Espinoza | 🇲🇽 México | PE |
| Chen Yiwen | 🇨🇳 China | CY |
| Sarah Bacon | 🇺🇸 USA | SB |
| Melissa Wu | 🇦🇺 Australia | MW |

## 🎲 Probability Model

### Difficulty distribution

`rangosDificultad()` maps a uniform draw from 1–100 onto seven difficulty bands:

```
 5%  → 1.4 – 1.9   (easiest)
10%  → 2.0 – 2.5
15%  → 2.5 – 3.0
30%  → 3.0 – 3.5   (most common)
20%  → 3.5 – 4.0
15%  → 4.0 – 4.5
 5%  → 4.5 – 4.7   (hardest)
```

Difficulty is drawn as an integer and divided by 10, so values are always one decimal place. The bands share endpoints (2.5, 3.0, 3.5, 4.0, 4.5 each appear in two bands); `calificacionPrimerJuez()` tests them in order with inclusive comparisons, so a boundary value is always handled by the easier of the two.

### Judge 1

`calificacionPrimerJuez()` selects from a fixed table of 21 possible scores (0.0 to 10.0 in 0.5 increments). It draws a second uniform 1–100 value and routes it through a seven-way split whose thresholds depend on the difficulty band. As difficulty rises, probability mass shifts toward the lower end of the table. The routing is hardcoded per band rather than derived from a formula.

### Judges 2–7

`calificacionSiguientesJueces()` takes judge 1's score and applies a random deviation:

```
20%  →  ±0        7%  →  +1.5
15%  →  +0.5      7%  →  -1.5
15%  →  -0.5      5%  →  +2
10%  →  +1        5%  →  -2
10%  →  -1        3%  →  +2.5
                  3%  →  -2.5
```

If the result falls outside 0.0–10.0, the judge returns judge 1's score unchanged. This means the panel is correlated by construction: all six remaining judges anchor to the same first score.

## 📐 Round Scoring

`puntuacionRonda()` computes the round score as:

```
round score = (sum of the 7 judge scores − highest judge score) × difficulty
```

Note this keeps six judges, not five — see Known Issues below.

**Total score** = sum of the 5 round scores.

## 🛠️ Technologies

- **Language:** C++ (C++11 or later)
- **Concepts:** arrays, function decomposition, probability, `rand()`/`srand()`
- **Design approach:** "Matryoshka" — each function wraps the previous one

## 🏗️ Function Chain

```
generarNum1al100()
    ↓
rangosDificultad()            → difficulty band
    ↓
calificacionPrimerJuez()      → judge 1
    ↓
calificacionSiguientesJueces()→ one of judges 2–7
    ↓
ronda()                       → fills judges 2–7 into the array
    ↓
puntuacionRonda()             → round score
    ↓
ejecutarRonda()               → one athlete, one round
    ↓
ejecutarRondaVariasVeces()    → all 5 athletes, one round
    ↓
main() → formatoTable() → medallero()
```

## 📝 Array Structure

Each round for each athlete uses a 9-element `float` array:

```
[0]   Difficulty
[1]   Judge 1 score
[2-7] Judge 2–7 scores
[8]   Round score
```

`ejecutarRonda()` fills `[1]`, then `[2..7]`, then `[8]`, and assigns `[0]` last.

## 🚀 Compilation & Execution

```bash
g++ -o diving clavadosDefinitivo.cpp -std=c++11
./diving
```

**Windows:**
```bash
g++ -o diving.exe clavadosDefinitivo.cpp
diving.exe
```

## 🔬 Key Functions

```cpp
int   generarNum1al100()                                  // uniform draw, 1–100
float rangosDificultad(int input)                         // probability → difficulty (1.4–4.7)
float calificacionPrimerJuez(float dificultad)            // judge 1, conditioned on difficulty
float calificacionSiguientesJueces(float primerJuez)      // one deviating judge
void  ronda(float primeraCalificacion, float calif[])     // fills judges 2–7
float puntuacionRonda(float calif[], float dificultad)    // round score
void  ejecutarRonda(string clavadista, string pais, float calif[])
void  ejecutarRondaVariasVeces(float c1[], ..., float c5[])
void  formatoTable(float R1[], ..., float R5[], string nombre)
int   medallero(float t1, float t2, float t3, float t4, float t5)
```

## 🏅 Medal Algorithm

1. Scan for the maximum total → gold, record its index
2. Scan again excluding the gold index → silver
3. Scan again excluding gold and silver → bronze
4. Index maps into a 10-element array holding 5 names followed by 5 countries

## 🔧 Customization

**Adjust difficulty probabilities** — in `rangosDificultad()`:
```cpp
if (input < 6 && input > 0)
    { dificultadElegida = rand() % 6 + 14; }   // change band edges and ranges
```

**Change the judge deviation spread** — in `calificacionSiguientesJueces()`:
```cpp
if (probabilidadAleatoria <= 20)
    { calificacionSiguienteJuez = primerJuez; }
```

**Change the number of rounds** — declare additional `calificacionesR6XX[9]` arrays in `main()` and extend `formatoTable()` and the total passed to `medallero()`.

## 🎓 Academic Context

**Course:** Programming Fundamentals / Computational Thinking
**Institution:** ITESM
**Created:** October 31, 2024
**Concepts:** function decomposition, array manipulation, probability simulation, conditional logic
