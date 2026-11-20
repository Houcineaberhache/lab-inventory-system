# Laboratory Equipment Inventory Management System

*Gestion de l'inventaire du matériel d'un laboratoire*

A cloud-based web application to manage laboratory equipment (computers, monitors, printers, projectors, technical devices), built as a **Cloud Computing mini project** on **Microsoft Azure**.

> **Key goal:** inventory data must survive application redeployment. The application (Azure App Service) and the data (Azure SQL Database) are separate cloud resources, so redeploying the code never deletes the data.

---

## Features

- View all laboratory equipment
- Add new equipment
- Edit equipment information
- Change equipment status: `Available`, `In Use`, `Under Maintenance`, `Out of Service`
- Delete equipment
- Dashboard with inventory statistics
- Search by name, filter by category and status
- Persistent storage in Azure SQL Database

## Architecture

Three-layer architecture:

```
User -> Web Browser -> Azure App Service (F1) -> Backend -> Azure SQL Database
```

| Layer | Role | Technology |
|---|---|---|
| Presentation | Web interface | HTML, CSS, Bootstrap |
| Application | Business logic / API | **TODO: FastAPI (Python) or ASP.NET Core, confirm with team** |
| Data | Persistent storage | Azure SQL Database (free tier) |

## Cloud Resources (Azure)

| Resource | Name |
|---|---|
| Resource Group | `RG-LabInventory` |
| App Service Plan | `ASP-LabInventory` (Free F1) |
| Web App | `lab-inventory-app` |
| SQL Server | `lab-inventory-server` |
| SQL Database | `LaboratoryInventoryDB` |

Names may change during deployment.

## Repository Structure

```
lab-inventory-system/
├── backend/      # Backend source code
├── frontend/     # User interface
├── database/     # SQL scripts (schema, sample data)
├── infra/        # Azure configuration notes and screenshots
├── docs/         # Project documentation, test reports
├── .env.example  # Template for environment variables
└── README.md
```

## Database

- Database: `LaboratoryInventoryDB`
- Main table: `Equipment` (`Id`, `Name`, `Category`, `Description`, `SerialNumber` (unique), `Status`, `Location`, `PurchaseDate`, `CreatedAt`)
- The `Status` column is restricted by a `CHECK` constraint to the four allowed values.
- SQL scripts are in the `database/` folder.

## Configuration

Credentials are **never** committed to this repository.

- **Locally:** copy `.env.example` to `.env` and fill in your values (`.env` is git-ignored).
- **On Azure:** set the same variables in **App Service -> Environment variables**.

| Variable | Description |
|---|---|
| `DB_SERVER` | Azure SQL server host name |
| `DB_NAME` | Database name |
| `DB_USER` | SQL username |
| `DB_PASSWORD` | SQL password |

## Run Locally

> **TODO:** complete once the backend stack is chosen.

```bash
# 1. Clone
git clone https://github.com/<owner>/lab-inventory-system.git
cd lab-inventory-system

# 2. Configure environment
cp .env.example .env     # Windows PowerShell: Copy-Item .env.example .env

# 3. Install dependencies and start the backend
# (commands depend on the chosen stack)
```

## Deployment

1. Create the Azure resources (see `infra/`).
2. Configure the SQL Server firewall and environment variables.
3. Connect this repository to Azure App Service (Deployment Center) or deploy from the IDE.
4. Open the public URL: `https://lab-inventory-app.azurewebsites.net`

## Persistence Demonstration

1. Deploy the application.
2. Add three items (Dell Computer, HP Monitor, Epson Projector).
3. Set the Dell Computer to `Under Maintenance`.
4. Change a small element (e.g. dashboard title) and **redeploy**.
5. Reopen the app: all records and statuses are still there.

## Git Workflow

- `main`: stable, deployed code
- `dev`: integration branch
- `feature/<name>`: one branch per task, merged into `dev` through a Pull Request

## Team

| Member | Role | Responsibility |
|---|---|---|
| Houcine | Cloud Infrastructure Engineer | Azure resources, networking, environment variables |
| Fatima | Database Developer | Schema, SQL scripts, sample data |
| Mohammed | Backend Developer | API, CRUD, database integration |
| Aya | Frontend Developer | Dashboard, forms, responsive UI |
| Abdo | Integration, Testing and Documentation | Repository, integration, tests, presentation |

## Security

- No credentials in the repository
- Connection strings stored in Azure environment variables
- Parameterized queries / ORM only
- Input validation on all forms
- Unique serial numbers enforced by the database
- HTTPS for all access

## License

Academic project, for educational purposes.
