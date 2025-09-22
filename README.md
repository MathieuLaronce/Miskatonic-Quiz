flowchart TD
    A([Utilisateur saisit identifiant + mot de passe])
    B[API reçoit les données]
    C{Identifiant trouvé dans SQLite ?}
    D{Mot de passe correct ?}
    E{Rôle = Enseignant ?}
    F{Rôle = Élève ?}
    G{Rôle = Admin ?}

    H[Création token JWT / session]
    I[Utilisateur authentifié]

    J[Créer / Modifier / Supprimer questions]
    K[Validation saisie]
    L[Stockage MongoDB]
    M[Génération quiz aléatoire / export CSV]
    N[Réponse aux questions]
    O[Soumission des réponses via API]

    P[Gestion utilisateurs / rôles]
    Q[Stockage SQLite]
    R[Suppression / modification questions / quiz]
    S[Stockage MongoDB]

    T([Accès refusé])
    Z([STOP])

    %% Flux principal
    A --> B
    B -->|Non| Z
    B -->|Oui| C
    C -->|Non| Z
    C -->|Oui| D
    D -->|Non| Z
    D -->|Oui| H
    H --> I
    I --> E

    %% Branche Enseignant
    E -->|Oui| J
    J --> K --> L --> M --> N --> O --> Z

    %% Branche Élève
    E -->|Non| F
    F -->|Oui| N
    N --> O --> Z

    %% Branche Admin
    F -->|Non| G
    G -->|Oui| P
    P --> Q --> R --> S --> Z

    %% Si aucun rôle
    G -->|Non| T --> Z
