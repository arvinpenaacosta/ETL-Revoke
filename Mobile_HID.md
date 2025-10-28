%% Click & Write Manager – Complete User Flow
graph TD
    A[Page Loads] --> B{IndexedDB Supported?}
    B -->|No| C[Show Error Toast]
    B -->|Yes| D[Open DB Selector Modal]
    
    D --> E{Compatible DBs Exist?}
    E -->|No| F[Show Create DB Modal]
    E -->|Yes| G[Display List of DBs]
    
    G --> H[User Selects a DB]
    F --> H2[User Creates New DB]
    H2 --> H[DB Selected]
    
    H --> I[Enable All Buttons<br/>Save • View • Import • Export]
    
    I --> J[User Fills Click Value + Write Value]
    J --> K[Click Save/Update]
    K --> L[Record Saved<br/>Show Toast + Clear Form]
    
    I --> M[Click View/Edit Records]
    M --> N[Show Table<br/>with Radio Rows]
    
    N --> O[User Selects a Row]
    O --> P[Click Edit → Open Edit Modal]
    O --> Q[Click Delete → Confirm Dialog]
    
    P --> P1[Save Changes]
    P1 --> N
    Q --> Q1[Delete Record]
    Q1 --> N
    
    I --> R[Click Import CSV]
    R --> S[Parse CSV<br/>Skip if 'que' exists]
    S --> T[Add Valid Records]
    T --> U[Show Final Status Log<br/>+ Toast Summary]
    
    I --> V[Click Export CSV]
    V --> W[Download File<br/>DBname_date.csv]

    classDef action fill:#e3f2fd,stroke:#1976d2,stroke-width:2px,color:#1976d2;
    classDef decision fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;
    classDef modal fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#4a148c;
    classDef success fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    
    class A,D,F,G,H2,M,N,P,Q,R,V action
    class B,E decision
    class F modal
    class L,U,W success
