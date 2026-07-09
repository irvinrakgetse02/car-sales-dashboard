# Data Model

The dashboard uses a star-schema-style relational model built in Power Query / Power BI:

## Tables

**Dealers**
| Column | Type |
|---|---|
| DealerID | Whole Number (key) |
| DealerName | Text |
| City | Text |
| Country | Text |

**Models**
| Column | Type |
|---|---|
| ModelID | Whole Number (key) |
| Brand | Text |
| Model | Text |
| Segment | Text |
| EngineSize (L) | Decimal |
| Fuel | Text |
| Price (USD) | Currency |
| Profit (USD) | Calculated (Price × 10%) |

**Sales**
| Column | Type |
|---|---|
| SaleID | Whole Number (key) |
| Date | Date |
| DealerID | Whole Number (FK → Dealers) |
| ModelID | Whole Number (FK → Models) |
| Quantity | Whole Number |
| TotalPrice | Calculated (Quantity × Model Price) |
| Total Profit | Calculated (Quantity × Model Profit) |

## Relationships

- `Sales[DealerID]` → `Dealers[DealerID]` (many-to-one)
- `Sales[ModelID]` → `Models[ModelID]` (many-to-one)

## Key Measures (suggested DAX)

```dax
Total Revenue = SUM(Sales[TotalPrice])

Total Profit = SUM(Sales[Total Profit])

Total Qty = SUM(Sales[Quantity])

Qty % of Total = DIVIDE([Total Qty], CALCULATE([Total Qty], ALL(Models)))

Revenue % by City = DIVIDE([Total Revenue], CALCULATE([Total Revenue], ALL(Dealers)))
```
