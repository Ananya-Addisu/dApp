# To-Do App  

![Chromia](https://img.shields.io/badge/Blockchain-Chromia-blue) ![Next.js](https://img.shields.io/badge/Framework-Next.js-000000) ![License](https://img.shields.io/badge/License-MIT-green)  

A blockchain-powered task management system ensuring **immutability**, **security**, and **transparency** via Chromia Made by [Ananya Addisu Workineh](https://sidraq.t.me/)

---

## **Overview**  
### **Purpose**  
A decentralized application (dApp) for managing tasks on the Chromia blockchain, designed to:  
- Ensure **data integrity** through immutable ledger storage.  
- Provide **end-to-end security** via cryptographic authentication.  
- Enable **cross-chain interoperability** for future extensibility.  

### **Key Features**  
| Feature | Description |  
|---------|-------------|  
| **User Registration** | Secured by Chromia’s `ft4` auth descriptors (multi-sig/EVM-compatible). |  
| **Task CRUD** | Create, Read, Update, Delete tasks with blockchain-verified ownership. |  
| **Rate Limiting** | Prevents spam with 10 transactions/minute per account. |  
| **Cross-Chain Readiness** | Built to support future cross-chain transfers. |  

### **Target Audience**  
- **Developers**: Explore blockchain integration patterns.  
- **End Users**: Manage tasks in a trustless, decentralized environment.  

---

## **Installation**  
### **Prerequisites**  
- Node.js v18+  
- PostgreSQL v14+  
- Docker (for local node setup)  
- Chromia CLI (`chr`)  

### **Backend Setup**  
1. Clone the repository:  
   ```bash  
   git clone https://github.com/Ananya-Addisu/dapp.git  
   cd dapp/ daap backend  
   ```  
2. Install dependencies:  
   ```bash  
   npm install  
   ```  
3. Start PostgreSQL via Docker:  
   ```bash  
   docker-compose up -d  
   ```  
4. Deploy the smart contract:  
   ```bash  
   chr node start && chr deploy  
   ```  

### **Frontend Setup**  
1. Clone the frontend repository:  
   ```bash  
   git clone https://github.com/Ananya-Addisu/dapp.git  
   cd dapp/frontend 
   ```  
2. Install dependencies:  
   ```bash  
   npm install  
   ```  
3. Run the development server:  
   ```bash  
   npm run dev  
   ```  

---

## **Architecture**  
### **System Workflow**  
```mermaid
sequenceDiagram
    participant User
    participant "Chromia Node" as ChromiaNode
    participant Rell
    participant "Dapp table state" as DappTableState

    User->>ChromiaNode: Query 'get_all_tasks'
    ChromiaNode->>Rell: Query Rell
    Rell->>DappTableState: Query for all tasks
    DappTableState -->>DappTableState: Execute query
    DappTableState-->>Rell: Return results for all tasks
    Rell-->>ChromiaNode: Return results for all tasks
    ChromiaNode-->>User: Return results for all tasks
```

```mermaid
sequenceDiagram
    participant User
    participant "Chromia Node" as ChromiaNode
    participant Rell
    participant "Dapp table state" as DappTableState
    participant "Blockchain transactions" as BlockchainTransactions

    User->>ChromiaNode: Signs operation/transaction 'create_task'
    note over User,ChromiaNode: Args: Title, Author, ISBN
    ChromiaNode->>ChromiaNode: Validates transaction to add new task data
    ChromiaNode->>Rell: Applies transaction "tx create_task isbn, title, author"
    Rell->>DappTableState: Calls to update the state of the operation
    DappTableState->>DappTableState: Inserts task in table
    DappTableState-->>Rell: Transaction confirmed or rejected
    Rell-->>ChromiaNode: Confirms task transaction committed
    ChromiaNode-->>User: Confirms task transaction committed
```
---
## **Project Structure**  
### **Frontend/Backend Layout**  

```bash  
└── ananya-addisu-dapp/
    ├── Readme.md
    ├── dapp backend/
    │   ├── chromia.yml
    │   ├── docker-compose.yml
    │   ├── package-lock.json
    │   ├── package.json
    │   ├── build/
    │   │   └── to_do_project.xml
    │   └── src/
    │       ├── development.rell
    │       ├── main.rell
    │       ├── lib/
    │       │   └── ft4/
    │       │       ├── module.rell
    │       │       ├── version.rell
    │       │       ├── accounts/
    │       │       │   ├── module.rell
    │       │       │   ├── linking/
    │       │       │   │   └── module.rell
    │       │       │   └── strategies/
    │       │       │       ├── module.rell
    │       │       │       ├── open/
    │       │       │       │   └── module.rell
    │       │       │       └── transfer/
    │       │       │           ├── module.rell
    │       │       │           ├── fee/
    │       │       │           │   └── module.rell
    │       │       │           ├── open/
    │       │       │           │   └── module.rell
    │       │       │           └── subscription/
    │       │       │               └── module.rell
    │       │       ├── admin/
    │       │       │   ├── module.rell
    │       │       │   └── crosschain/
    │       │       │       └── module.rell
    │       │       ├── assets/
    │       │       │   ├── module.rell
    │       │       │   └── locking/
    │       │       │       └── module.rell
    │       │       ├── auth/
    │       │       │   └── module.rell
    │       │       ├── core/
    │       │       │   ├── accounts/
    │       │       │   │   ├── auth_basic.rell
    │       │       │   │   ├── auth_descriptor_rule_expression.rell
    │       │       │   │   ├── auth_descriptor_rule_validation.rell
    │       │       │   │   ├── auth_external.rell
    │       │       │   │   ├── auth_flags_config.rell
    │       │       │   │   ├── module.rell
    │       │       │   │   ├── rate_limit.rell
    │       │       │   │   ├── linking/
    │       │       │   │   │   ├── model.rell
    │       │       │   │   │   └── module.rell
    │       │       │   │   └── strategies/
    │       │       │   │       ├── auth_message.rell
    │       │       │   │       ├── module.rell
    │       │       │   │       ├── open/
    │       │       │   │       │   └── module.rell
    │       │       │   │       └── transfer/
    │       │       │   │           ├── config.rell
    │       │       │   │           ├── config_utils.rell
    │       │       │   │           ├── fee_config.rell
    │       │       │   │           ├── module.rell
    │       │       │   │           ├── queries.rell
    │       │       │   │           ├── fee/
    │       │       │   │           │   ├── config.rell
    │       │       │   │           │   ├── module.rell
    │       │       │   │           │   └── queries.rell
    │       │       │   │           ├── open/
    │       │       │   │           │   └── module.rell
    │       │       │   │           └── subscription/
    │       │       │   │               ├── config.rell
    │       │       │   │               ├── module.rell
    │       │       │   │               └── queries.rell
    │       │       │   ├── admin/
    │       │       │   │   └── module.rell
    │       │       │   ├── assets/
    │       │       │   │   ├── asset.rell
    │       │       │   │   ├── extensions.rell
    │       │       │   │   ├── history.rell
    │       │       │   │   ├── module.rell
    │       │       │   │   ├── transfer.rell
    │       │       │   │   └── locking/
    │       │       │   │       ├── functions.rell
    │       │       │   │       └── module.rell
    │       │       │   ├── auth/
    │       │       │   │   ├── authentication.rell
    │       │       │   │   ├── login.rell
    │       │       │   │   └── module.rell
    │       │       │   ├── crosschain/
    │       │       │   │   ├── blockchain.rell
    │       │       │   │   ├── extensions.rell
    │       │       │   │   ├── module.rell
    │       │       │   │   └── transfer.rell
    │       │       │   └── prioritization/
    │       │       │       ├── module.rell
    │       │       │       └── default/
    │       │       │           └── module.rell
    │       │       ├── crosschain/
    │       │       │   └── module.rell
    │       │       ├── external/
    │       │       │   ├── accounts/
    │       │       │   │   ├── module.rell
    │       │       │   │   ├── operations.rell
    │       │       │   │   ├── queries.rell
    │       │       │   │   └── strategies/
    │       │       │   │       ├── module.rell
    │       │       │   │       ├── operations.rell
    │       │       │   │       └── queries.rell
    │       │       │   ├── admin/
    │       │       │   │   ├── module.rell
    │       │       │   │   ├── operations.rell
    │       │       │   │   └── crosschain/
    │       │       │   │       └── module.rell
    │       │       │   ├── assets/
    │       │       │   │   ├── module.rell
    │       │       │   │   ├── operations.rell
    │       │       │   │   ├── queries.rell
    │       │       │   │   └── locking/
    │       │       │   │       ├── module.rell
    │       │       │   │       └── queries.rell
    │       │       │   ├── auth/
    │       │       │   │   ├── module.rell
    │       │       │   │   ├── operations.rell
    │       │       │   │   └── queries.rell
    │       │       │   └── crosschain/
    │       │       │       ├── module.rell
    │       │       │       ├── operations.rell
    │       │       │       └── queries.rell
    │       │       ├── prioritization/
    │       │       │   ├── module.rell
    │       │       │   └── default/
    │       │       │       └── module.rell
    │       │       ├── test/
    │       │       │   └── utils.rell
    │       │       └── utils/
    │       │           ├── pagination.rell
    │       │           └── utils.rell
    │       ├── registration/
    │       │   ├── module.rell
    │       │   └── test/
    │       │       └── regestration_test.rell
    │       ├── test/
    │       │   ├── test_operations.rell
    │       │   └── to_do_project_test.rell
    │       └── to_do/
    │           ├── auth.rell
    │           ├── dto.rell
    │           ├── model.rell
    │           ├── module.rell
    │           ├── operations.rell
    │           └── queries.rell
    └── frontend/
        ├── README.md
        ├── eslint.config.mjs
        ├── next-env.d.ts
        ├── next.config.ts
        ├── package-lock.json
        ├── package.json
        ├── postcss.config.mjs
        ├── tailwind.config.ts
        ├── tsconfig.json
        ├── public/
        │   └── todo.PNG
        └── src/
            ├── app/
            │   ├── globals.css
            │   ├── layout.tsx
            │   ├── page.tsx
            │   ├── components/
            │   │   └── NavBar.tsx
            │   ├── create/
            │   │   └── page.tsx
            │   ├── list/
            │   │   └── page.tsx
            │   └── register/
            │       └── page.tsx
            ├── components/
            │   ├── AddTodo.tsx
            │   └── TodoItem.tsx
            ├── hooks/
            │   └── useTodos.ts
            └── types/
                └── todo.ts
  
```  

### **File Responsibility Matrix**  
| File/Directory              | Type       | Key Responsibilities                              |  
|-----------------------------|------------|---------------------------------------------------|  
| `docker-compose.yml`        | Config     | PostgreSQL container setup, port mapping, volumes |  
| `chromia.yml`               | Config     | Blockchain node settings, FT4 auth configurations |  
| `src/to_do/operations.rell` | Code       | Task lifecycle management (create/update/delete)  |  
| `src/registration/test/`    | Test       | EVM signature validation and account creation     |  
| `src/test/to_do_project_test.rell` | Test | Full task lifecycle and query validation          |  


---

## **Core Components**  

### **Data Models**  
#### **User Entity (`src/to_do/model.rell`)**  
```rell  
entity user {  
    key user_id: byte_array;     // Unique identifier (EVM address hash)  
    mutable name: text;          // Display name (max 50 characters)  
    key account;                 // Linked FT4 account (foreign key)  
}  
```  

| Field   | Type       | Properties | Description                   |
| ------- | ---------- | ---------- | ----------------------------- |
| user_id | byte_array | key        | Unique identifier for user    |
| name    | text       | mutable    | User's name                   |
| account | account    | key        | Associated blockchain account |
#### **Task Entity (`src/to_do/model.rell`)**  
```rell  
entity task {  
    index user;                  // Task owner reference  
    key task_id: byte_array;     // UUIDv4 identifier  
    mutable task_title: text;    // Task name (non-empty)  
    mutable task_description: text?; // Optional details  
    mutable due_date: timestamp; // Deadline (future timestamp)  
    mutable is_completed: boolean = false;  
    created_at: timestamp = op_context.last_block_time;  
    mutable updated_at: timestamp = op_context.last_block_time;  
}  
```  

| Field            | Type       | Properties                   | Description                |
| ---------------- | ---------- | ---------------------------- | -------------------------- |
| user             | user       | index                        | Reference to user entity   |
| task_id          | byte_array | key                          | Unique identifier for task |
| task_title       | text       | mutable, key                 | Title of the task          |
| task_description | text       | mutable, key                 | Description of the task    |
| is_completed     | boolean    | mutable, default: false      | Task completion status     |
| created_at       | timestamp  | default: block_time          | Task creation timestamp    |
| updated_at       | timestamp  | mutable, default: block_time | Last update timestamp      |
| due_date         | timestamp  | mutable                      | Task due date              |
| to_date          | timestamp  | mutable, default: block_time | Task target date           |
#### **Task DTO (`src/to_do/dto.rell`)**
```rell
struct task_dto {
    task_id: byte_array;
    task_title: text;
    task_description: text;
    due_date: timestamp;
    is_completed: boolean;
    created_at: timestamp;
    updated_at: timestamp;
} 
```

| Field            | Type       | Properties | Description             |
| ---------------- | ---------- | ---------- | ----------------------- |
| task_id          | byte_array | -          | Task identifier         |
| task_title       | text       | -          | Title of the task       |
| task_description | text       | -          | Description of the task |
| due_date         | timestamp  | -          | Task due date           |
| is_completed     | boolean    | -          | Completion status       |
| created_at       | timestamp  | -          | Creation timestamp      |
| updated_at       | timestamp  | -          | Last update timestamp   |

---

### **Authentication & Authorization**  
#### **FT4 Session Flags**  
```mermaid  
flowchart LR  
    A[EVM Signature] --> B{Flags A+T}  
    B -->|Valid| C[Create Account]  
    C --> D[Issue MySession Token]  
    D --> E[Task Operations]  
```  

| Flag         | Scope                     | Granting Mechanism                    |  
|--------------|---------------------------|---------------------------------------|  
| **A**        | Account Creation          | `single_sig_auth_descriptor()` in `registration_test.rell` |  
| **T**        | Task Management           | FT4 account strategy in `module.rell` |  
| **MySession**| CRUD Operations           | `auth.add_auth_handler()` in `auth.rell` |  

#### **Auth Handler Implementation (`src/to_do/auth.rell`)**  
```rell  
@extend(auth.auth_handler)  
function () = auth.add_auth_handler(  
    flags = ["MySession"],  // Session permissions  
    strategy = auth.strategy.session_based()  
)  
```  

---

### **Task Operations**  
#### **Operation Specifications (`src/to_do/operations.rell`)**  

| Operation        | Parameters                                  | Validation Rules                     |  
|------------------|---------------------------------------------|---------------------------------------|  
| `create_task`    | `user_id`, `task_id`, `title`, `desc`, `due_date` | - Title non-empty<br>- `due_date` ≥ current block time |  
| `update_task`    | `task_id`, `new_title`, `new_desc`, `new_due`     | - Task exists<br>- User owns task    |  
| `delete_task`    | `task_id`                                   | - Valid task ID                      |  

#### **Transaction Flow**  
```mermaid  
flowchart LR  
    U[User] --> S(Sign with EVM Key)  
    S --> V{Validate Fields}  
    V -->|Valid| T[[Submit to Chromia Nodes]]  
    T --> C{Consensus Reached?}  
    C -->|Yes| UD[Update PostgreSQL State]  
    C -->|No| FL[Reject: Log Error]  
    UD --> NT[Task Recorded in Blockchain]  
```  

---

### **Query System**  
#### **Query Types (`src/to_do/queries.rell`)**  

| Query                      | Filters                       | Output Format (`task_dto`)       |     |
| -------------------------- | ----------------------------- | -------------------------------- | --- |
| `get_user_tasks`           | User ID                       | All tasks sorted by `due_date`   |     |
| `get_user_tasks_by_status` | User ID + `is_completed` flag | Filtered tasks with status match |     |
| `get_overdue_tasks`        | User ID + `due_date < now()`  | Incomplete tasks past deadline   |     |

#### **Query Workflow**  
```mermaid  
flowchart TD  
    A[User] --> B(Invoke Query)  
    B --> C[Check PostgreSQL Cache]  
    C --> D[Verify Against Blockchain Ledger]  
    D --> E{Consistent?}  
    E -->|Yes| F[Return Results]  
    E -->|No| G[Trigger State Sync]  
```  

---
## **Smart Contract (Backend)**  
### **Core Modules**  
1. **Authentication (`auth.rell`)**  
   ```rell  
   operation create_user(name: text, auth_descriptor: auth_descriptor) {  
     require(name.length > 0, "Invalid name");  
     create user(name, auth_descriptor);  
   }  
   ```  
   - Validates EVM signatures and multi-sig policies.  

2. **Task Operations (`operations.rell`)**  
   - Enforces ownership checks for task updates/deletes.  

### **Data Models**  
```rell  
entity user {  
  key id: integer;  
  name: text;  
  account: account;  
}  

entity task {  
  key id: integer;  
  user: user;  
  title: text;  
  description: text;  
  status: boolean;  
  created_at: timestamp;  
}  
```  

---

## **Frontend**  
### **Component Structure**  
| Component | Purpose |  
|-----------|---------|  
| `register/page.tsx` | EVM wallet-based user registration |  
| `useTodos.ts` | Manages task state with localStorage sync |  
| `TodoItem.tsx` | Renders task cards with edit/delete actions |  

### **Authentication Flow**  
1. User signs in via MetaMask.  
2. Frontend submits signature to Chromia for verification.  
3. Session token stored in `localStorage` upon success.  

---

## **Security**  
### **Protocols**  
- **Input Validation**: Rejects empty/malformed payloads.  
- **Rate Limiting**: `rl_state` enforces 10 TX/minute per account.  
- **Auth Descriptors**: Multi-sig policies for critical operations.  

---

## **Testing**  
### **Backend Tests**  
```bash  
chr test  # Runs unit tests for smart contracts  
```  
**Key Test Cases**:  
- Invalid auth descriptor rejection.  
- Task creation with empty title prevention.  

### **Frontend Tests**  
```bash  
npm run cypress  # Launches E2E test suite  
```  

---

## **Deployment**  
### **Steps**  
1. Start Chromia node and retrieve `blockchainRid`:  
   ```bash  
   chr node start --wipe  
   ```  
2. Update `.env`:  
   ```env  
   NEXT_PUBLIC_BRID=<blockchainRid>  
   ```  
3. Configure `next.config.js`:  
   ```javascript  
   const nextConfig = {  
     output: "export",  
     images: { unoptimized: true },  
     basePath: `/web_query/${process.env.NEXT_PUBLIC_BRID}/web_static`,  
   };  
   ```  
4. Build and deploy:  
   ```bash  
   pnpm build && chr node update  
   ```  
5. Access at:  
   ```  
   http://localhost:7740/web_query/<blockchainRid>/web_static  
   ```  

