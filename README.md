# Voorraad Beheersysteem

## Overzicht
Deze applicatie is ontworpen voor effectief voorraad- en orderbeheer, gericht op verschillende gebruikersrollen zoals beheerders, verkoopmanagers, magazijnbeheerders, medewerkers en toekomstige klanten. Het systeem maakt rolgebaseerde toegang mogelijk om de operaties te stroomlijnen, voorraadniveaus te beheren, productcompatibiliteit te volgen, verkooprapporten te genereren en magazijnbeheer te vergemakkelijken.

## Functionele Eisen
- **Voorraadbeheer**: Huidige voorraadniveaus per product bekijken.
- **Product CRUD Acties**: Gebruikers kunnen producten toevoegen, bijwerken en verwijderen.
- **Prijscontrole**: Beheerders kunnen inkoop- en verkoopprijzen aanpassen.
- **Magazijn Tracking**: Locaties van artikelen worden in het magazijn gevolgd.
- **Productcompatibiliteit**: Verkoopmedewerkers kunnen compatibele producten zien.
- **Rolgebaseerde Permissies**: Toegangscontrole wordt verleend op basis van rollen (bijv. volledige toegang voor managers, beperkte toegang voor medewerkers).
- **Accessoire Toewijzing**: Compatibele accessoires kunnen aan producten worden toegewezen (bijv. afstandsbediening aan tv).
- **Verkooptracking**: Verkochte hoeveelheden per producttype bijhouden.
- **Categoriebeheer**: Beheer van categorieën zoals tv's, afstandsbedieningen, muurbeugels en CI-modules.
- **Medewerkergegevens**: Opslaan en weergeven van medewerkerinformatie, rollen en rechten.
- **Verkooprapportage**: Rapporten genereren op basis van beschikbare verkoopgegevens.
- **Magazijnbeheer**: Voorraad en productplaatsing volgen.
- **Binnenkomende Voorraad**: Inkomende voorraad bekijken (bestellingen in afwachting).
- **Compatibiliteit van Accessoires**: Gegevens opslaan over compatibele accessoires per product.
- **Actielogging**: Acties zoals toevoegingen of wijzigingen per gebruikersrol weergeven.

## Niet-Functionele Eisen
- **Gebruiksvriendelijke Interface**: Eenvoudig en intuïtief voor alle gebruikers.
- **Gegevensbeveiliging**: Veilige opslag van gegevens en gecontroleerde toegang.
- **Prestatieoptimalisatie**: Ondersteuning voor maximaal 30 gelijktijdige gebruikers.
- **Apparaatcompatibiliteit**: Toegankelijk op desktops en tablets.
- **Hoge Beschikbaarheid**: Zorg voor minimale downtime voor bedrijfscontinuïteit.
- **Schaalbaarheid**: Toestaan van toekomstige groei in producttypes en functies.
- **Responsief Ontwerp**: Aanpasbaar voor verschillende schermformaten.

## Klassenstructuur

De volgende klassendiagram geeft een gedetailleerd overzicht van de klassenstructuur van het systeem:

```mermaid
classDiagram
    %% Base Class: User
    class User {
        -userID: int
        -username: String
        -password: String
        +role: String
        +contactInfo: String
    }

    %% AdminUser Class
    class AdminUser {
        -purchaseRecords: List
        +adminPermissions: String
        +viewInventory(): void
        +manageEmployeeRecords(): void
        +updateInventoryPricing(): void
    }
    
    %% SalesManager Class
    class SalesManager {
        +salesReportAccess: String
        +stockOverviewAccess: String
        +viewSalesReport(): void
        +initiatePromotion(): void
        +checkStockLevels(): void
    }

    %% WarehouseManager Class
    class WarehouseManager {
        -warehouseLocationData: String
        -inventoryStatus: String
        +trackInventoryLocation(): void
        +manageWarehouseStock(): void
    }
    
    %% Employee Class
    class Employee {
        -department: String
        +permissions: String
        +performAssignedTask(): void
    }
    
    %% Client Class
    class Client {
        -clientID: int
        -orderHistory: List
        +placeOrder(): void
        +viewOrderHistory(): void
    }

    %% Product Class
    class Product {
        -productID: int
        +name: String
        +category: String
        +price: float
        -stockLevel: int
        +getProductDetails(): void
        +updateStockLevel(): void
    }
    
    %% Inventory Class
    class Inventory {
        -inventoryID: int
        -productList: List
        +location: String
        +addProduct(): void
        +removeProduct(): void
        +checkCompatibility(): Boolean
    }
    
    %% Order Class
    class Order {
        -orderID: int
        +date: Date
        -customerID: int
        -productList: List
        +totalPrice: float
        +createOrder(): void
        +calculateTotalPrice(): float
    }

    %% Inheritance Relationships
    AdminUser --|> User
    SalesManager --|> User
    WarehouseManager --|> User
    Employee --|> User
    Client --|> User

    %% Associations
    User "1" -- "many" Order : places
    Inventory "1" -- "many" Product : contains
    Order "1" -- "many" Product : includes
```

### Verklaring

- **Erfenis**: Specifieke gebruikersrollen (`AdminUser`, `SalesManager`, `WarehouseManager`, `Employee`, `Client`) erven van de basis klasse `User`.
- **Associaties**:
  - `User` heeft een één-op-veel relatie met `Order` omdat gebruikers meerdere bestellingen kunnen plaatsen.
  - `Inventory` bevat een verzameling `Product` items.
  - `Order` omvat meerdere `Product` items voor elke transactie.
