# Flash Advanced PawnShop System - Script Documentation

Purchase:
- [Escrowed](https://flashscripts.tebex.io/package/6707709)
- [OpenSource](https://flashscripts.tebex.io/package/6707714)
Preview: [Youtube](https://www.youtube.com/watch?v=BlAy_2ZRS_U)

## Framework Support
- ESX Legacy
- QBox Core
- QBCore

## Dependencies
Required resources for proper functioning:
- oxmysql
- ox_lib
- ox_inventory

## Installation Guide

### Step 1: Resource Setup
1. Download the `flash-pawnshop` resource
2. Place it in your server's resources directory
3. Add `ensure flash-pawnshop` to your `server.cfg`

### Step 2: Database Installation
Execute the following SQL queries in your database:

```sql
CREATE TABLE IF NOT EXISTS `pawnshop_prices` (
    `item_name` VARCHAR(50) NOT NULL,
    `shop_id` INT NOT NULL,
    `current_price` INT NOT NULL,
    `base_price` INT NOT NULL,
    `money_type` VARCHAR(20) NOT NULL DEFAULT 'cash',
    `last_update` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    `sales_count` INT NOT NULL DEFAULT 0,
    PRIMARY KEY (`item_name`, `shop_id`)
);

CREATE TABLE IF NOT EXISTS `pawnshop_sales` (
    `id` INT AUTO_INCREMENT,
    `shop_id` INT NOT NULL,
    `item_name` VARCHAR(50) NOT NULL,
    `quantity` INT NOT NULL,
    `price` INT NOT NULL,
    `money_type` VARCHAR(20) NOT NULL DEFAULT 'cash',
    `sale_time` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (`id`),
    INDEX `idx_shop_item_time` (`shop_id`, `item_name`, `sale_time`)
);
```

## Configuration Guide

### Basic Configuration
The main configuration file (`config.lua`) allows you to customize various aspects of the pawnshop system:

```lua
Config.Framework = "auto"    -- Framework detection ("auto", "esx", "qb", "qbx")
Config.Debug = false        -- Enable/disable debug mode
Config.VersionCheck = true  -- Enable/disable version checking
```

### Inventory System
```lua
Config.Inventory = "auto"   -- "auto", "ox_inventory", "qs-inventory"
Config.ImgURL = "nui://ox_inventory/web/images/"  -- Image URL for inventory items
```

### Price System Configuration
```lua
Config.FluctuationIncrease = 10  -- Maximum price increase percentage
Config.FluctuationDecrease = 10  -- Maximum price decrease percentage
Config.PriceUpdateTime = 15      -- Minutes between price updates
Config.QualityMultiplier = false -- Enable/disable quality-based pricing
Config.AllowSellAnywhere = false -- Allow selling at any pawnshop
Config.SeparateShopPrices = true -- Enable separate price tracking per shop
```

### Target System Configuration
Choose and configure your preferred targeting system:

```lua
Config.Target = "TextUI"    -- "ox", "qb", or "TextUI"

Config.TargetSettings = {
    TextUI = {
        label = "[E] Open Pawnshop"
    },
    ox = {
        icon = "fas fa-store",
        label = "Open Pawnshop",
        distance = 2.0
    },
    qb = {
        icon = "fas fa-store",
        label = "Open Pawnshop",
        distance = 2.0
    }
}
```

### Product Lists
First, define your product lists in the `Config.ProductLists` section:
```lua
Config.ProductLists = {
    ["jewelry"] = {
        [1] = {
            productName = "diamond_ring",
            productLabel = "Diamond Ring",
            productPrice = "500",
            moneyType = "money"
        },
        -- Add more jewelry items...
    },
    ["food"] = {
        [1] = {
            productName = "burger",
            productLabel = "Burger",
            productPrice = "500",
            moneyType = "money"
        },
        -- Add more food items...
    }
}
```

### Pawnshop Configuration
Then configure your pawnshops to use these product lists:
```lua
Config.Pawnshops = {
    [1] = {
        CategoryLabel = "Downtown Pawnshop",
        Blip = {
            enable = true,
            blipSprite = 605,
            blipDisplay = 4,
            blipScale = 0.7,
            blipColour = 2,
            blipDisplayName = "Downtown Pawnshop"
        },
        Ped = {
            model = 's_m_o_busker_01',
            location = vector3(182.584625, -1319.7626, 29.313599),
            heading = 240.94488,
            interactionRange = 2.0,
        },
        ProductList = {"jewelry", "food"} -- Shop will sell items from both lists
    },
    [2] = {
        CategoryLabel = "Vinewood Pawnshop",
        -- ... other configuration ...
        ProductList = {"food"} -- Shop will only sell food items
    }
}
```

### Features
- Define product lists once and reuse them across multiple shops
- Each shop can use multiple product lists
- Prices are tracked per shop, even when using the same product lists
- Easy to maintain and update product lists centrally
- Separate price tracking per shop with `SeparateShopPrices` option
- Automatic sales count tracking for better price calculations

## Commands
The script includes useful commands for players and administrators:

```lua
Config.PricesCommand = {
    enabled = true,
    name = "prices",
    help = "View current pawnshop prices"
}
```

## Blacklist System
Prevent specific items from being sold:

```lua
Config.BlacklistedItems = {
    'money',
    'cash',
    'black_money',
}
```

## Money Types
Configure different types of currency:

```lua
Config.MoneyTypes = {
    ["money"] = {
        label = "Cash",
        icon = "fa-dollar-sign"
    },
    ["black_money"] = {
        label = "Dirty Money",
        icon = "fa-sack-dollar"
    }
}
```

## Support
For additional support or questions:
- Join our Discord: [Flash Development Discord](https://discord.gg/AhUYns4A9R)
- Report issues through our support ticket system
- Check our documentation updates regularly
