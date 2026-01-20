## Istruzioni
Per ciascun numero indicato, determinare **la rappresentazione in virgola mobile secondo lo standard IEEE 754**, specificando:
- bit di **segno**,
- **esponente polarizzato**,
- **mantissa**,
- parola binaria finale **[Segno | Esponente | Mantissa]**.

---

## Esercizi

1. Dato il numero **−42.625**, determinarne la rappresentazione in **doppia precisione** (IEEE 754).

2. Dato il numero **15.75**, determinarne la rappresentazione in **singola precisione** (IEEE 754).

3. Dato il numero **−12.125**, determinarne la rappresentazione in **doppia precisione** (IEEE 754).

4. Dato il numero **0.375**, determinarne la rappresentazione in **singola precisione** (IEEE 754).

5. Dato il numero **256.0**, determinarne la rappresentazione in **doppia precisione** (IEEE 754).

6. Dato il numero **−50.0**, determinarne la rappresentazione in **singola precisione** (IEEE 754).

7. Dato il numero **−0.1875**, determinarne la rappresentazione in **doppia precisione** (IEEE 754).

8. Dato il numero **120.5**, determinarne la rappresentazione in **singola precisione** (IEEE 754).

9. Dato il numero **1024.0**, determinarne la rappresentazione in **doppia precisione** (IEEE 754).

10. Dato il numero **−1.25**, determinarne la rappresentazione in **singola precisione** (IEEE 754).

---

# Riepilogo Teorico – Standard IEEE 754

## 1. Conversione e Normalizzazione
- Convertire il **valore assoluto** del numero dalla base $10$ alla base $2$ (parte intera e parte frazionaria).
- Normalizzare il numero in forma: $1.m \times 2^e$
- Il bit $1$ prima della virgola è **implicito** e **non viene memorizzato**.

## 2. Bit di Segno (1 bit)
- $0$ → numero positivo  
- $1$ → numero negativo  

## 3. Esponente Polarizzato
L’esponente memorizzato ($e_p$) si ottiene come: $e_p = e + \text{Bias}$

- **Singola precisione**  
  - Bias = 127
  - Esponente codificato su **8 bit**

- **Doppia precisione**
  - Bias = 1023
  - Esponente codificato su **11 bit**

## 4. Mantissa
- La mantissa è costituita dai bit della parte frazionaria ($m$) dopo la normalizzazione.
- Deve essere completata con zeri a destra fino alla lunghezza richiesta.

  - **Singola precisione:** 23 bit  
  - **Doppia precisione:** 52 bit  

## 5. Struttura Finale del Numero
La rappresentazione binaria finale è composta come segue: $\text{Segno Esponente Mantissa}$

---

## Formati IEEE 754

| Formato             | Bit totali | Segno | Esponente | Mantissa | Bias |
|---------------------|------------|-------|-----------|----------|------|
| Singola precisione  | 32         | 1     | 8         | 23       | 127  |
| Doppia precisione   | 64         | 1     | 11        | 52       | 1023 |

---

# Risoluzioni

### 1. −42.625 — **Doppia precisione (64 bit)**

- Valore binario normalizzato:  
    $42.625_{10} = 101010.101_2 = 1.01010101 \times 2^5$
    
- Segno: $1$
    
- Esponente polarizzato:  
    $5 + 1023 = 1028_{10} = 10000000100_2$
    
- Mantissa (52 bit):  
    $0101010100000000000000000000000000000000000000000000$
    

Risultato:  
$1\ 10000000100\ 0101010100000000000000000000000000000000000000000000$

---

### 2. 15.75 — **Singola precisione (32 bit)**

- Valore binario normalizzato:  
    $15.75_{10} = 1111.11_2 = 1.11111 \times 2^3$
    
- Segno: $0$
    
- Esponente polarizzato:  
    $3 + 127 = 130_{10} = 10000010_2$
    
- Mantissa (23 bit):  
    $11111000000000000000000$
    

Risultato:  
$0\ 10000010\ 11111000000000000000000$

---

### 3. −12.125 — **Doppia precisione (64 bit)**

- Valore binario normalizzato:  
    $12.125_{10} = 1100.001_2 = 1.100001 \times 2^3$
    
- Segno: $1$
    
- Esponente polarizzato:  
    $3 + 1023 = 1026_{10} = 10000000010_2$
    
- Mantissa (52 bit):  
    $1000010000000000000000000000000000000000000000000000$
    

Risultato:  
$1\ 10000000010\ 1000010000000000000000000000000000000000000000000000$

---

### 4. 0.375 — **Singola precisione (32 bit)**

- Valore binario normalizzato:  
    $0.375_{10} = 0.011_2 = 1.1 \times 2^{-2}$
    
- Segno: $0$
    
- Esponente polarizzato:  
    $-2 + 127 = 125_{10} = 01111101_2$
    
- Mantissa (23 bit):  
    $10000000000000000000000$
    

Risultato:  
$0\ 01111101\ 10000000000000000000000$

---

### 5. 256.0 — **Doppia precisione (64 bit)**

- Valore binario normalizzato:  
    $256_{10} = 100000000_2 = 1.0 \times 2^8$
    
- Segno: $0$
    
- Esponente polarizzato:  
    $8 + 1023 = 1031_{10} = 10000000111_2$
    
- Mantissa (52 bit):  
    $0000000000000000000000000000000000000000000000000000$
    

Risultato:  
$0\ 10000000111\ 0000000000000000000000000000000000000000000000000000$

---

### 6. −50.0 — **Singola precisione (32 bit)**

- Valore binario normalizzato:  
    $50_{10} = 110010_2 = 1.10010 \times 2^5$
    
- Segno: $1$
    
- Esponente polarizzato:  
    $5 + 127 = 132_{10} = 10000100_2$
    
- Mantissa (23 bit):  
    $10010000000000000000000$
    

Risultato:  
$1\ 10000100\ 10010000000000000000000$

---

### 7. −0.1875 — **Doppia precisione (64 bit)**

- Valore binario normalizzato:  
    $0.1875_{10} = 0.0011_2 = 1.1 \times 2^{-3}$
    
- Segno: $1$
    
- Esponente polarizzato:  
    $-3 + 1023 = 1020_{10} = 01111111100_2$
    
- Mantissa (52 bit):  
    $1000000000000000000000000000000000000000000000000000$
    

Risultato:  
$1\ 01111111100\ 1000000000000000000000000000000000000000000000000000$

---

### 8. 120.5 — **Singola precisione (32 bit)**

- Valore binario normalizzato:  
    $120.5_{10} = 1111000.1_2 = 1.1110001 \times 2^6$
    
- Segno: $0$
    
- Esponente polarizzato:  
    $6 + 127 = 133_{10} = 10000101_2$
    
- Mantissa (23 bit):  
    $11100010000000000000000$
    

Risultato:  
$0\ 10000101\ 11100010000000000000000$

---

### 9. 1024.0 — **Doppia precisione (64 bit)**

- Valore binario normalizzato:  
    $1024_{10} = 10000000000_2 = 1.0 \times 2^{10}$
    
- Segno: $0$
    
- Esponente polarizzato:  
    $10 + 1023 = 1033_{10} = 10000001001_2$
    
- Mantissa (52 bit):  
    $0000000000000000000000000000000000000000000000000000$
    

Risultato:  
$0\ 10000001001\ 0000000000000000000000000000000000000000000000000000$

---

### 10. −1.25 — **Singola precisione (32 bit)**

- Valore binario normalizzato:  
    $1.25_{10} = 1.01_2 = 1.01 \times 2^0$
    
- Segno: $1$
    
- Esponente polarizzato:  
    $0 + 127 = 127_{10} = 01111111_2$
    
- Mantissa (23 bit):  
    $01000000000000000000000$
    

Risultato:  
$1\ 01111111\ 01000000000000000000000$

---