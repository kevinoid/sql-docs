---
title: "Aliasing"
description: "Aliasing in Azure Synapse Analytics and Parallel Data Warehouse."
titleSuffix: Azure Synapse Analytics
ms.custom: "seo-lt-2019"
ms.date: "03/16/2017"
ms.prod: sql
ms.reviewer: ""
ms.technology: t-sql
ms.topic: conceptual
ms.assetid: 7b3a5c74-05cf-4385-8ee6-6176d003cb8a
author: MikeRayMSFT
ms.author: mikeray
monikerRange: ">= aps-pdw-2016 || = azure-sqldw-latest"
---

# Aliasing (Azure Synapse Analytics, Parallel Data Warehouse)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

Aliasing allows the temporary substitution of a short and easy-to-remember string in place of a table or column name in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] or [!INCLUDE[ssPDW](../../includes/sspdw-md.md)][!INCLUDE[DWsql](../../includes/dwsql-md.md)] queries. Table aliases are often used in JOIN queries because the JOIN syntax requires fully qualified object names when referencing columns.  

Aliases must be single words conforming to object naming rules. For more information, see "Object Naming Rules" in the [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)]. Aliases cannot contain blank spaces and cannot be enclosed in either single or double quotes.  

## Syntax

```tsql
object_source [ AS ] alias
```

## Arguments

*object_source*  
The name of the source table or column.  

AS  
An optional alias preposition. When working with range variable aliasing, the AS keyword is prohibited.  

*alias*
The desired temporary reference name for the table or column. Any valid object name can be used. For more information, see "Object Naming Rules" in the [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  

## Examples: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] and [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

The following example shows a query with multiple joins. Both table and column aliasing are demonstrated in this example.  

- Column Aliasing: Both columns and expressions involving columns in the select list are aliased in this example. `SalesTerritoryRegion AS SalesTR` demonstrates a simple column alias. `Sum(SalesAmountQuota) AS TotalSales` demonstrates  

- Table Aliasing: `dbo.DimSalesTerritory AS st` shows creation of the `st` alias for the `dbo.DimSalesTerritory` table.  

```tsql
-- Uses AdventureWorks

SELECT LastName, SUM(SalesAmountQuota) AS TotalSales, SalesTerritoryRegion AS SalesTR,  
    RANK() OVER (PARTITION BY SalesTerritoryRegion ORDER BY SUM(SalesAmountQuota) DESC ) AS RankResult  
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryRegion != N'NA'  
GROUP BY LastName, SalesTerritoryRegion;  
```

The AS keyword can be excluded, as shown below, but is often included for readability.  

```tsql
-- Uses AdventureWorks

SELECT LastName, SUM(SalesAmountQuota) TotalSales, SalesTerritoryRegion SalesTR,  
RANK() OVER (PARTITION BY SalesTerritoryRegion ORDER BY SUM(SalesAmountQuota) DESC ) RankResult  
FROM dbo.DimEmployee e  
INNER JOIN dbo.FactSalesQuota sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryRegion != N'NA'  
GROUP BY LastName, SalesTerritoryRegion;  
```

## Next steps

- [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)