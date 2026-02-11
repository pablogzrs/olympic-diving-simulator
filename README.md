# 🏊 International Diving Competition Simulator

C++ simulation of an Olympic-style diving competition with probabilistic scoring system, difficulty ratings, and automated medal determination for 5 international athletes.

## 📋 Overview

Realistic diving competition simulator featuring 5 rounds of competition, 7-judge scoring system, difficulty-based performance probabilities, and automatic podium calculation. Simulates real Olympic diving rules and scoring mechanics.

## ✨ Features

- **5-Round Competition** - Complete tournament simulation
- **Probabilistic Scoring** - Difficulty affects success probability
- **7-Judge Panel** - Official Olympic scoring format
- **Difficulty Ranges** - 1.4 to 4.8 difficulty scale
- **Automated Medal System** - Gold, silver, bronze determination
- **Realistic Athletes** - 5 international competitors
- **Detailed Score Tables** - Round-by-round breakdown

## 🏅 Competitors

| Athlete | Country | Code |
|---------|---------|------|
| Alejandra Orozco | 🇲🇽 México | AO |
| Paola Espinoza | 🇲🇽 México | PE |
| Chen Yiwen | 🇨🇳 China | CY |
| Sarah Bacon | 🇺🇸 USA | SB |
| Melissa Wu | 🇦🇺 Australia | MW |

## 🎲 Probability Model

### Difficulty Distribution
```
1.4-1.9: 5% chance (easiest)
2.0-2.5: 10% chance
2.5-3.0: 15% chance
3.0-3.5: 30% chance (most common)
3.5-4.0: 20% chance
4.0-4.5: 15% chance
4.5-4.8: 5% chance (hardest)
```

### Scoring Logic
**Higher difficulty = Higher risk/reward:**
- Easy dives (1.4-1.9): 80% chance of good score (8.0-10.0)
- Medium dives (3.0-3.5): 60% chance of good score
- Hard dives (4.5-4.8): 15% chance of perfect score, 35% chance of failure

## 🛠️ Technologies

- **Language:** C++ (C++11 or later)
- **Concepts:** Arrays, functions, probability, randomization
- **Design Pattern:** Matryoshka (nested function calls)

## 🏗️ Architecture

**"Matryoshka" Function Design:**
```
generarNum1al100()
    ↓
rangosDificultad()
    ↓
calificacionPrimerJuez()
    ↓
generarCalificacionesCompletas()
    ↓
ejecutarRondaVariasVeces()
    ↓
main() → medallero()
```

Each function builds upon the previous one, creating a nested structure.

## 🚀 Compilation & Execution

**Compile:**
```bash
g++ -o diving clavadosDefinitivo.cpp -std=c++11
```

**Run:**
```bash
./diving
```

**Windows:**
```bash
g++ -o diving.exe clavadosDefinitivo.cpp
diving.exe
```

## 📊 Output Format

### Round Results
```
 Clavado | Dificultad | Juez 1 | Juez 2 | Juez 3 | Juez 4 | Juez 5 | Juez 6 | Juez 7 | Total
--------------------------------------------------------------------------------------------------------
    1         3.2        7.5      8.0      7.5      8.5      8.0      7.0      8.0      56.7
    2         2.8        9.0      8.5      9.0      9.5      9.0      8.5      9.0      72.1
    ...
```

### Final Medal Ceremony
```
¡La ganadora es Chen Yiwen, puntuando 347.8 puntos!
China se lleva la medalla de oro

¡El segundo lugar es para Sarah Bacon, puntuando 332.5 puntos!
Estados Unidos se lleva la medalla de plata

¡El tercer lugar es para Alejandra Orozco, puntuando 318.2 puntos!
México se lleva la medalla de bronce
```

## 🔬 Key Functions

### Core Probability Functions
```cpp
int generarNum1al100()
// Generates random number 1-100 for probability checks

float rangosDificultad(int input)
// Maps probability to difficulty rating (1.4-4.8)

float calificacionPrimerJuez(float dificultad)
// Generates score based on difficulty (0.0-10.0)
```

### Scoring Functions
```cpp
void generarCalificacionesCompletas(float array[])
// Generates all 7 judge scores + difficulty + total

void ejecutarRondaVariasVeces(float AO[], float PE[], ...)
// Runs complete round for all 5 athletes
```

### Display & Results
```cpp
void formatoTable(float R1[], float R2[], ...)
// Prints formatted score table for athlete

int medallero(float total1, float total2, ...)
// Determines gold, silver, bronze winners
```

## 📐 Scoring Calculation

**Official Olympic Diving Formula:**
```
1. Each judge scores 0.0 to 10.0 (increments of 0.5)
2. Drop highest and lowest scores
3. Sum remaining 5 scores
4. Multiply by difficulty rating
5. Result = Round score
```

**Total Score:**
```
Final Score = Sum of all 5 round scores
```

## 🎯 Difficulty-Performance Correlation

| Difficulty | Perfect (9-10) | Good (7-9) | Medium (5-7) | Poor (0-5) |
|-----------|---------------|------------|--------------|------------|
| 1.4-1.9   | 20%          | 60%        | 15%          | 5%         |
| 2.5-3.0   | 10%          | 45%        | 30%          | 15%        |
| 3.5-4.0   | 5%           | 25%        | 45%          | 25%        |
| 4.5-4.8   | 5%           | 10%        | 50%          | 35%        |

## 🔧 Customization

**Modify competitor list:**
```cpp
string participantes[10] = {
    "Your Athlete 1", "Your Athlete 2", ...,
    "Country 1", "Country 2", ...
};
```

**Adjust difficulty probabilities:**
```cpp
// In rangosDificultad() function
if (input < 6 && input > 0)  // Change percentage ranges
    {dificultadElegida = rand() % 6 + 14;}
```

**Change number of rounds:**
```cpp
// In main(), duplicate round code blocks
float calificacionesR6AO[9] = {0.0, ...};
// Add to formatoTable() and medallero() calculations
```

## 📝 Implementation Notes

**Array Structure:**
```
calificacionesR1AO[9]:
[0] = Difficulty
[1] = Judge 1 score
[2] = Judge 2 score
...
[7] = Judge 7 score
[8] = Total round score
```

**Medal Algorithm:**
1. Find maximum score → Gold (track index)
2. Find maximum excluding gold index → Silver
3. Find maximum excluding gold & silver → Bronze
4. Index maps to athlete name and country

## 🎓 Academic Context

**Course:** Programming Fundamentals / Computational Thinking  
**Concepts Demonstrated:**
- Function decomposition
- Array manipulation
- Probability simulation
- Conditional logic
- Iterative algorithms

## ⚠️ Known Limitations

- Fixed number of competitors (5)
- Fixed number of rounds (5)
- Difficulty ranges hardcoded
- No tie-breaking logic
- Static athlete roster

## 🔍 Technical Highlights

- **Nested function architecture** (Matryoshka design)
- **Probabilistic distributions** for realistic outcomes
- **Array-based data management**
- **Automated scoring calculations**
- **Index-based winner determination**

## 🏊 Example Run

```
Bienvenidas y bienvenidos al concurso internacional de clavados 2024
Comenzará la primera ronda...
[Scores generated for all 5 athletes]

Comenzará la segunda ronda...
[...]

¡Hemos llegado al final del evento!
[Full score table displayed]

¡La ganadora es...!
```

---

**Author:** @pablo  
**Created:** October 31, 2024  
**Language:** C++11  
**Type:** Probabilistic Simulation
