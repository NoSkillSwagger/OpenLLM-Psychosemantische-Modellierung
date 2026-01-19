# Semantische Ähnlichkeit und Wortassoziationen in LLMs

Dieses Projekt untersucht, ob Large Language Models (LLMs) **semantische Verwandtschaften, Assoziationen und Bedeutungscluster** zeigen, die mit denen von Menschen vergleichbar sind.

Dazu werden LLM-Antworten systematisch mit etablierten **menschlichen Sprachdatensätzen** verglichen. Der Fokus liegt auf **freier Wortassoziation** und **semantischer Ähnlichkeitsbewertung** sowie auf dem Einfluss von **Modellfamilie** und **Modellgröße**.

---

## Forschungsfragen

- Bilden LLMs semantische Assoziationen ähnlich wie Menschen?
- Wo treten systematische Abweichungen zwischen Menschen und Modellen auf?
- Unterscheiden sich verschiedene Modellfamilien?
- Spielt die Modellgröße eine Rolle für die semantische Übereinstimmung?
- Welche Modellgrößen erlauben eine zuverlässige automatische Auswertung?

---

## Aufgaben & Datensätze

### 1. Freie Wortassoziation
**Aufgabe:**  
Zu einem gegebenen Stichwort werden bis zu drei einwortige Assoziationen generiert, geordnet nach Assoziationsstärke.

**Datensatz:**  
- **SWOW-18EN** (Small World of Words, De Deyne et al.)

**Untersuchte Teilmengen:**
- Top-100 Stichwörter mit stärkster menschlicher R1-Assoziation
- 100 zufällig ausgewählte Stichwörter

---

### 2. Semantische Ähnlichkeitsbewertung
**Aufgabe:**  
Bewertung der semantischen Ähnlichkeit eines Wortpaares auf einer Skala von **0 (völlig unzusammenhängend)** bis **10 (identische Bedeutung)**.

**Datensatz:**  
- **WordSim-353** (Finkelstein et al.)

---

## Untersuchte Modelle

Alle Modelle wurden lokal mit **Ollama** ausgeführt.

| Modellfamilie | Modellgrößen |
|--------------|--------------|
| **Qwen3** | 0.6B, 1.7B, 8B, 14B, 30B |
| **Gemma3** | 270M, 1B, 4B, 12B, 27B |

---

## Prompting-Strategie

Strikte Systemprompts stellten sicher, dass:
- deterministische Ausgabeformate eingehalten werden
- keine Erklärungen oder Begründungen ausgegeben werden
- die Ergebnisse maschinell auswertbar sind

Dies ermöglichte reproduzierbare und automatisierte Experimente über alle Modelle hinweg.

---

## Experimentelles Vorgehen

1. Einrichtung lokaler LLMs über Ollama  
2. Erste manuelle Tests der Prompts  
3. Aufbau automatisierter Pipelines für:
   - Freie Wortassoziation (SWOW-18EN Teilmengen)
   - Ähnlichkeitsbewertung (WordSim-353)
4. Datenerhebung mit LLMs  
5. Statistische Analyse und Vergleich mit menschlichen Referenzdaten  

---

## Ergebnisse: Freie Wortassoziation

### Zentrale Beobachtungen

- **Fehlerraten sinken mit steigender Modellgröße**
- Größere Modelle treffen **starke menschliche Assoziationen** häufiger
- Leistung sinkt bei Stichwörtern mit **stark gestreuten menschlichen Antworten**

### Wichtige Beobachtung

Standardmetriken gewichten **seltene menschliche Antworten** genauso stark wie dominante Assoziationen.  
Eine Beschränkung auf **hochfrequente (starke) menschliche Assoziationen** verbessert die Übereinstimmung zwischen Mensch und Modell deutlich.

### Zusammenfassung

- Hohe menschliche Übereinstimmung → hohe Modellübereinstimmung  
- Hohe menschliche Streuung → stärkere Modellabweichung  
- Aggregierte R1–R3-Ähnlichkeit bleibt weitgehend stabil  

---

## Ergebnisse: Semantische Ähnlichkeitsbewertung

### Zentrale Beobachtungen

- Größere Modelle vergeben **systematisch niedrigere Ähnlichkeitswerte** als Menschen
- Menschen komprimieren Bedeutungsräume stärker
- Größere Modelle differenzieren Bedeutungen feiner

### Beispiel: *„Schwert“ vs. „Messer“*

- **Menschen:**  
  > Beides sind Waffen → hohe Ähnlichkeit
- **LLMs:**  
  > Waffe vs. Haushaltsgegenstand, Größe, Kontext, Epoche → niedrigere Bewertung

---

## Abschließendes Fazit

**Zeigen LLMs menschähnliche semantische Strukturen?**

➡️ **Jein.**

- **Freie Wortassoziation:**  
  Größere Modelle zeigen häufig menschenähnliche Assoziationen, insbesondere bei starken menschlichen Assoziationen.
- **Ähnlichkeitsbewertung:**  
  Größere Modelle weichen stärker von menschlichen Bewertungen ab und treffen feinere semantische Unterscheidungen.

Insgesamt bilden LLMs **menschenähnliche Bedeutungsstrukturen** ab, gewichten und strukturieren diese jedoch **anders** als Menschen.

---

## Quellen

- De Deyne, S., et al. (2018). *Measuring the associative structure of English: The Small World of Words norms.*
- Finkelstein, L., et al. (2001). *Placing search in context: The concept revisited.* Proceedings of WWW 2001.
