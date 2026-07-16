# Flow — Loop Engineering (marketingová automatika)

> Swimlane diagram funkčního řešení: dráhy = kdo/co daný krok dělá.
> Otázka, na kterou odpovídá: „Jak systém funguje a kdo vlastní který krok?"
> Generuje agent z KONTEXT.md a prvni-30-dnu.md. Poslední update: 12. 7. 2026

```mermaid
flowchart LR
    subgraph JA["👤 Já (Head of Marketing)"]
        direction TB
        I1["Instrukce + cíl<br>„z MD vznikne 5 výstupů,<br>každý projde validátorem“"]
        G1{"Human gate<br>(tréninkový režim)"}
        A1["Postupné otevírání<br>autonomie"]
    end

    subgraph AG["🤖 Agent"]
        direction TB
        S1["1 · Průzkum<br>(kontext, content-log)"]
        S2["2 · Plán"]
        S3["3 · Akce<br>(drafty 5 výstupů)"]
    end

    subgraph VAL["✅ Validátor (objektivní)"]
        direction TB
        V1{"Kontrola: fakta,<br>brand, formát"}
    end

    subgraph OUT["📣 Kanály + paměť"]
        direction TB
        P1["Publikace<br>(LinkedIn, newsletter…)"]
        M1["Zápis do content-log.md<br>(paměť smyčky)"]
    end

    I1 --> S1 --> S2 --> S3 --> V1
    V1 -->|"neprošlo"| S3
    V1 -->|"prošlo"| G1
    G1 -->|"schváleno"| P1
    G1 -->|"korekce"| S3
    P1 --> M1
    M1 -->|"učení z historie"| S1
    G1 -.->|"časem méně zásahů"| A1
    A1 -.->|"gate se otevírá"| P1

    classDef human fill:#fff3d6,stroke:#e0a83c,color:#3d3a33
    classDef agent fill:#e8eef7,stroke:#4a6fa5,color:#2c3a4a
    classDef valid fill:#f7e8e8,stroke:#a54a4a,color:#4a2c2c
    classDef out fill:#e8f2ec,stroke:#4a9d6e,color:#2c4a38
    class I1,G1,A1 human
    class S1,S2,S3 agent
    class V1 valid
    class P1,M1 out
```
