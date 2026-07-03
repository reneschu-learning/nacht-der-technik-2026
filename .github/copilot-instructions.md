# Richtlinien für React + Vite + TypeScript-Projekte

Diese Anweisung gilt für die gesamte React/TypeScript-Entwicklung in diesem Vite-basierten Projekt.

## Projekt-Setup & Build

- **Build-Tool**: Nur Vite. Verwende `npm run dev` für die lokale Entwicklung und `npm run build` für die Produktion.
- **Kein Testing**: Füge KEIN Jest, Vitest, React Testing Library oder andere Test-Frameworks hinzu.
- **Einstiegspunkt**: TypeScript mit TSX-Unterstützung. Root-Komponente in `src/main.tsx` oder ähnlich.
- **Keine komplexen Toolchains**: Halte Abhängigkeiten minimal, nur das, was Vite + React benötigt.

## Komponentenstruktur

### Dateiorganisation
```
src/
├── main.tsx          # Einstiegspunkt
├── App.tsx           # Root-Komponente
├── components/       # Wiederverwendbare Komponenten (optional)
│   ├── Header.tsx
│   ├── Footer.tsx
│   └── ...
├── styles/           # CSS-Dateien (eine pro Komponente oder global)
│   ├── global.css
│   ├── App.css
│   └── ...
└── types/            # TypeScript-Typen (optional)
```

### Nur funktionale Komponenten
- Verwende **ausschliesslich funktionale Komponenten** mit React Hooks.
- Verwende niemals Klassenkomponenten.
- Halte die Hook-Nutzung einfach (Grundlagen von useState, useEffect, useContext).

**Beispiel**:
```tsx
import React, { useState } from 'react';
import './MyComponent.css';

export function MyComponent() {
  const [count, setCount] = useState(0);
  
  return (
    <div className="my-component">
      <h1>Counter: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

## Styling mit reinem CSS

- **Keine externen CSS-Frameworks** (kein Tailwind, Bootstrap usw.).
- **Keine CSS-in-JS-Bibliotheken** (keine styled-components, emotion usw.).
- Schreibe reines CSS in `.css`-Dateien.
- Verwende **eine CSS-Datei pro Komponente** oder eine globale `global.css` für gemeinsame Styles.
- Importiere CSS direkt in den Komponentendateien.

### CSS Best Practices
- Verwende **semantische Klassennamen** basierend auf dem Zweck von Komponente oder Element (z. B. `.card`, `.header`, `.button-primary`).
- Bevorzuge **CSS Custom Properties (Variablen)** für Farben, Abstände und Schriftarten:
  ```css
  :root {
    --primary-color: #3498db;
    --spacing-small: 8px;
    --spacing-medium: 16px;
    --font-family: 'Arial', sans-serif;
  }
  ```
- Nutze **modernes CSS** (Flexbox, Grid usw.).
- Vermeide tiefe Verschachtelung; halte die Spezifität niedrig.

**Beispiel**:
```css
/* MyComponent.css */
.my-component {
  padding: var(--spacing-medium);
  background-color: var(--primary-color);
  border-radius: 8px;
}

.my-component button {
  padding: var(--spacing-small);
  font-size: 1rem;
  cursor: pointer;
}
```

## Responsives Design

- **Mobile-First-Ansatz**: Starte mit Mobile-Styles und ergänze Media Queries für grössere Bildschirme.
- Verwende **Breakpoints** für gängige Bildschirmgrössen:
  ```css
  /* Mobile: ab 320px (Standard) */
  /* Tablet: ab 768px */
  @media (min-width: 768px) { ... }
  /* Desktop: ab 1024px */
  @media (min-width: 1024px) { ... }
  ```
- Teste in **Chromium-basierten Browsern** (Chrome, Edge, Brave).
- Stelle sicher, dass Text gut lesbar ist, Buttons gut antippbar sind und Layouts sich sauber anpassen.

## Visuelle Qualität & UX

- **Zentriertes & ausgewogenes Layout**: Verwende Flexbox oder Grid, um Inhalte passend zu zentrieren.
- **Farbe & Kontrast**: Wähle eine angenehme Farbpalette mit ausreichend Kontrast für gute Lesbarkeit.
- **Typografie**: Verwende konsistente Schriftarten, Grössen und Zeilenhöhen.
- **Abstände**: Nutze konsistente Margins und Paddings (gerne mit CSS-Variablen).
- **Hover-/Focus-States**: Füge dezentes visuelles Feedback für interaktive Elemente hinzu.
- **Sanfte Übergänge**: Nutze CSS-Transitions für Interaktionen (z. B. Klicks und Hover-States).

**Beispiel**:
```css
button {
  background-color: var(--primary-color);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: darken(var(--primary-color), 10%);
  /* Hinweis: darken() ist Pseudocode; verwende direkt hex/rgb oder calc() */
}
```

## Assets & Daten

- **Keine externe Persistenz**: Halte Assets und Daten im Speicher oder hartkodiert im Komponentenstate.
- **Inline-Daten**: Verwende Mock-Daten in Komponenten oder in separaten Datendateien (z. B. `src/data.ts`).
- **Bilder**: Wenn Bilder genutzt werden, speichere sie in `public/` und referenziere sie mit `/` (z. B. `<img src="/image.png" />`).
- **Keine Backend-Calls**: Vermeide API-Aufrufe, sofern sie nicht explizit benötigt werden; Fokus ist die Frontend-UI.

## TypeScript

- Verwende **striktes TypeScript** (`strict: true` in `tsconfig.json`).
- Definiere Typen für Props:
  ```tsx
  interface MyComponentProps {
    title: string;
    onClick: () => void;
  }
  
  export function MyComponent({ title, onClick }: MyComponentProps) {
    return <button onClick={onClick}>{title}</button>;
  }
  ```
- Halte Typen einfach und lesbar; vermeide Over-Engineering.

## Entwicklungsablauf

1. **Dev-Server starten**: `npm run dev` - öffnet http://localhost:5173 (oder ähnlich).
2. **Hot Module Replacement (HMR)**: Änderungen werden automatisch im Browser aktualisiert.
3. **Für Produktion bauen**: `npm run build` - erzeugt optimierte Ausgabe in `dist/`.
4. **Build-Vorschau**: `npm run preview` - teste den Produktionsbuild lokal.

## Häufige Fehler vermeiden

- ❌ Test-Frameworks hinzufügen (Jest, Vitest, React Testing Library).
- ❌ Klassenkomponenten oder veraltete React-Muster verwenden.
- ❌ Komplexes State-Management (Redux, Zustand) ohne Begründung.
- ❌ CSS-Frameworks oder Preprozessoren (Tailwind, SCSS, Less) nutzen.
- ❌ Inline-Styles nutzen; stattdessen CSS-Dateien verwenden.
- ❌ Responsiveness ignorieren; teste auf mehreren Bildschirmgrössen.
- ❌ Schlechter Farbkontrast oder unlesbarer Text.

## Einsteigerfreundliche Denkweise

Da die Zielgruppe GitHub Copilot lernt:
- **Code einfach und lesbar halten**: Klarheit vor Cleverness.
- **Direkte, verständliche Benennung nutzen**: `count`, `isOpen`, `handleClick` statt kryptischer Abkürzungen.
- **Nicht offensichtliche Logik kommentieren**: Hilf Lernenden, das "Warum" zu verstehen.
- **Beispiele bereitstellen**: Zeige vollständige, lauffähige Code-Snippets als Referenz.
- **Vorzeitige Optimierung vermeiden**: Fokus zuerst auf Korrektheit und Klarheit.

---

**Nächste Schritte**: Nach dem Setup dieses Projekts kannst du Copilot zum Beispiel fragen:
- "Erstelle eine Header-Komponente mit Navigationsmenü"
- "Füge ein responsives Grid-Layout für eine Produktliste hinzu"
- "Style ein Formular mit Validierungs-Feedback"
