# API Specification for Virtual Ecosystem

## 1. Village

All API calls for entities of the village.

### 1.1. Village Overview - `/village/` (GET)
Returns an overview of the village

**Response:**

```json
    { 
        "num_buildings": "int",  
        "num_villagers": "int"
    } 
```

### 1.2. Create villager - `/villager` (POST)
Creates a new villager, unassigned, with nourishment (0-100)

**Request:** 
```json
[
    { 
        "age": "int",
        "nourishment": "int" 
    }
] 
```

**Response:**  

```json
    {
        "Villager(s) successfully created"
    }
``` 

### 1.3 Remove villager - `/villager` (DELETE)
Kills the oldest amount of villagers depending on amount passed in

**Request:** 
```json
[
    { 
        "amount": "int" 
    }
] 
```

**Response:**  

```json
    {
        "success": "boolean"
    }
``` 


### 1.4. Build structure - `/village/build_building` (POST) 

**Request:** 

```json
[
    { 
        "resource_name": "string", 
        "amount": "int",
        "building_id": "int" 
    }
]
``` 

**Response:**

```json
{
    "success": "boolean"
}
``` 

### 1.5. Adjust storage `/village/fill_inventory` (PUT) 
Fill inventory of specific building(s)

**Request:** 

```json
[
    { 
        "building_id": "int", 
        "resource_name": "string", 
        "amount": "int" 
    }
]
```

**Response:** 

```json
{
    "success": "boolean"
}
``` 

### 1.6. View village inventory `/village/village_inventory/` (GET) 
Gets inventory across all buildings

**Response:** 

```json
[
    { 
        "resource_name": "string", 
        "amount": "int" 
    }
] 
```

### 1.7. View Catalog '/village/catalog' (GET)
Gets the catalog of valid buildings available to build

**Response:**

```json
{
    "buildings": "list(string)",
    "costs": "list(int)",
    "funds": "int"
}
```

### 1.8. Update Villager '/village/villager_update' (POST)
Updates villager population after decisions are made based on food and water income of that year

**Response:**

"Villagers consumed food and water"

## 2. Eco

All API calls for the ecosystem.


### 2.1. View plants - `/eco/plants/{biome_id}` (GET)  

Returns the id and nourishment of plants in the requested biome.

**Response:** 

```json
[
    { 
        "id": "int",
        "entity_type": "string", 
        "nourishment": "int" 
    }
]
``` 
 

### 2.2. Spawn entity - `/eco/entity` (POST)
Takes in a list of entities to be spawned in the requested biome. Will only create new entities if they don't already exist in the specified biome. Returns early if no entities are provided.

**Request:** 

```json
[
    { 
        "nourishment": "int",
        "entity_type": "string",
        "biome_id": "int"
    }
]
``` 

**Response:** 

```json
{
    "success": "boolean"
}
``` 

### 2.3. View prey in biome -`/eco/prey/{biome_id}` (GET)
Grabbing id and nourishment (0-100) of prey given a specific biome

**Response:** 

```json
[
    { 
        "id": "int",
        "entity_type": "string",
        "nourishment": "int" 
    }
]
``` 

### 2.4. View predators - `/eco/predator/{biome_id}` (GET)
Grabbing id and nourishment (0-100) of predators given a specific biome 

**Response:** 

```json
[
    { 
        "id": "int",
        "entity_type": "string",
        "nourishment": "int" 
    }
] 
```

### 2.5. Eco Overview - `/eco/` (GET)
Returns a general overview of the ecosystem with each biome listed once, showing all entities and their nourishment levels within that biome.

**Response:** 

```json
[
    {
        "biome_id": "int",
        "biome_name": "string",
        "entities": "string"
    }
] 
```

### 2.6. Post Biome Counts - `/eco/biomes` (POST)
Posts the biome counts from flood fill. Creates new biome entries based on the counts.

**Response:** 

```json
{
    "id": "int",
    "entity_type": "string",
    "nourishment": "int"
}
```

### 2.7. Update Nourishment - `/eco/entity/nourishment` (PUT)
Updates nourishment values for specific entities by their IDs. Ensures no negative IDs are allowed.

**Request:** 

```json
[
    {
        "id": "int",
        "nourishment": "float"
    }
] 
```

**Response:** 

```json
{
    "message": "Nourishment updated successfully"
}
```

### 2.8. View water in biome -`/eco/water/{biome_id}` (GET)
Grabbing id and nourishment (0-100) of water given a specific biome

**Response:** 

```json
[
    { 
        "id": "int",
        "entity_type": "string",
        "nourishment": "int" 
    }
]
``` 

### 2.9. View trees in biome -`/eco/trees/{biome_id}` (GET)
Grabbing id and nourishment (0-100) of trees given a specific biome

**Response:** 

```json
[
    { 
        "id": "int",
        "entity_type": "string",
        "nourishment": "int" 
    }
]
``` 

### 2.10. Clean -`/eco/clean/` (DELETE)
Deletes any entities with nourishment 0 or below

**Response:** 

```json
{ 
    "message": "Successfully Deleted Enities With Nourishment 0 Or Below and Biomes With no Entities"
}
``` 

### 2.11. Check Disaster -`/eco/disaster` (POST)
Has a chance to cause a random disaster, increases chance of disaster by 2% until one occurs. When a disaster occurs, up to 5 villagers can be killed.

**Response:** 

if disaster occurred:

```json
{
    "message": "Disaster occurred: {'disaster_type'}",
    "Villagers Killed": "int"
}
``` 

if no disaster occurred:

```json
{
    "message": "No disaster occurred",
    "current_probability": "float",
    "days_without_disaster": "int"
}
```

## 3. Jobs

Used for assigning jobs

### 3.1. Get job list - `jobs/` (GET)

**Response:**

```json
[
    {
        "job_name" : "string",
        "villagers_assigned" : "int"
    }
]
```

### 3.2. Assign job - `jobs/assignments` (PUT)

**Request:** 

```json
[
    {   
        "job_name": "string",
        "villager_id": "int" 
    }
]
``` 

**Response:** 

```json
[
    { 
        "villager_id": "int", 
        "villager_job_id": "int", 
        "villager_building_id": "int" 
    }
]
``` 

## 4. Admin

Used for resseting the game

### 5.1. Reset World - `/admin/reset` (PUT) 
Resets everything to original start of the game state 

**Response:** 

```json
{
    "success": "boolean"
}
``` 
