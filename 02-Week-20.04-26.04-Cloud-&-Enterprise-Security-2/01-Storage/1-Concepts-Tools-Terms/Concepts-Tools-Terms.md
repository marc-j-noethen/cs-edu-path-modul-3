# Storage

## 📊 Summary based on the 80/20 principle

### 1. Choosing the right storage or database model determines scalability and security
The core of the 80/20 principle is: understand the difference between unstructured data in storage services and structured data in databases. An Azure Storage Account is the entry point; redundancy protects your data.

### 2. Step-by-step core process
1. Classify your data: structured, semi-structured or unstructured.
2. Create a storage account as a management and security framework.
3. Choose the appropriate service: Blob, File, Queue, Table or a managed database.
4. Choose the right redundancy model to ensure data remains available even in the event of failures.

### 3. Interactive mode / Tool usage
Typically, you start with a storage account and then select the specific service. It is precisely this order that allows Azure to clearly separate the platform framework from data objects.

### 4. Key concepts with code examples
- **Storage account:** Central container for multiple Azure storage services.
- **Blob Storage:** Ideal for files, images, logs, backups and other unstructured data.
- **Azure SQL and Cosmos DB:** Suitable when data needs to be queried, filtered and processed consistently.
- **Redundancy:** LRS, ZRS or GRS determine how many copies Azure keeps of your data.

```bash
az storage account create \
  --name stmodul3demo \
  --resource-group rg-storage \
  --location westeurope \
  --sku Standard_LRS
```

### 5. Comparison: Structured vs. unstructured data
- Structured data follows fixed schemas and is well suited to relational databases.
- Unstructured data such as images, videos or logs are better suited to Blob Storage.
- Choosing the wrong storage type will later result in unnecessary costs, complexity and performance issues.

### 6. Why is this important / Benefits
Storage is never just about storage. Data management influences availability, backup, costs, search, access control and thus directly impacts the security architecture.

**Quick-start checklist**
- ☐ I can distinguish between structured and unstructured data.
- ☐ I know what an Azure Storage account is.
- ☐ I am familiar with Blob Storage and at least one Azure database option.
- ☐ I can explain why redundancy is important.

**Key point**
Choosing storage is an architectural decision: first understand the data type, then select the service and redundancy model.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| Azure Storage Account | Management framework for Azure storage services |
| Azure Blob Storage | Stores large amounts of unstructured data |
| Azure SQL Database | Managed relational database |
| Azure Cosmos DB | Globally scalable NoSQL database |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| Blob | Large binary object such as a file or image |
| LRS | Locally Redundant Storage with a local data copy |
| ZRS | Zone-Redundant Storage across multiple zones |
| GRS | Geo-Redundant Storage with a copy in another region |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| durability | Durability |
| redundancy | Redundancy |
| query | Query |
| backup | Backup |


