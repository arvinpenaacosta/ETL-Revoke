# Click & Toast Manager – Architecture & Flow

```mermaid
flowchart TD
    %% ==== HTML ELEMENTS ====
    Header["header"] --> DBSelectBtn["DB-Select Button"]
    Header --> DBLabel["#current-db-display"]
    Container["div.container"] --> BtnContainer["#button-container"]
    ToastStack["#toast-stack"]

    %% ==== JS MODULES ====
    subgraph DatabaseSelector ["DatabaseSelector (db-selector.js)"]
        DBModal["#db-select-modal"]
        DBCreateModal["#db-create-modal"]
        DBList["#db-list-container"]
        DBSelectBtnJS["showSelectionModal()"]
        DBConfirm["confirm()"]
        DBCreate["create()"]
        DBDelete["deleteSelected()"]
        DBScan["scan()"]
        DBHasStore["hasStore()"]
    end

    subgraph MainApp ["Main App (inline <script type=\"module\">)"]
        Init["init()"]
        UpdateDBDisplay["updateDBDisplay()"]
        HandleDB["handleDbSelected()"]
        LoadButtons["loadButtons()"]
        ShowToast["showToast()"]
        ToastAppend["toastStack.appendChild(toast)"]
    end

    %% ==== USER INTERACTIONS ====
    click DBSelectBtn --> DBSelectBtnJS
    DBSelectBtnJS --> DBScan
    DBScan --> DBHasStore
    DBHasStore --> DBList
    DBList --> DBConfirm
    DBConfirm --> HandleDB
    DBConfirm --> UpdateDBDisplay

    click DBSelectBtn --> DBCreate
    DBCreate --> DBCreateModal
    DBCreateModal --> DBConfirm

    click DBSelectBtn --> DBDelete
    DBDelete --> DBScan

    %% ==== BUTTON LOADING ====
    HandleDB -->|dbName| LoadButtons
    LoadButtons --> IDBOpen["indexedDB.open()"]
    IDBOpen -->|success| GetAll["store.getAll()"]
    GetAll -->|records| CreateBtn["for each record → button"]
    CreateBtn --> BtnContainer
    CreateBtn -->|onclick| ShowToast
    ShowToast --> ToastAppend
    ToastAppend --> ToastStack

    %% ==== AUTO RELOAD ====
    Focus["window focus"] --> LoadButtons

    %% ==== STYLING ====
    style Header fill:#007bff,stroke:#fff,color:#fff
    style ToastStack fill:#333,stroke:#fff,color:#fff
    style DBModal fill:#fff,stroke:#ddd
    style DBCreateModal fill:#fff,stroke:#ddd
