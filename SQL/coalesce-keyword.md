# COALESCE keyword

Give a table which has foerign keys in two other tables which are then joined together in a query or view, in cases where both child tabels store similar columns which should be shown as one in the query result, COALESCE can be used to return the first non-null value.

```sql
    SELECT mtpl.someprop2, 
           inbound.someprop
           COALESCE(mtpl.VehicleOwnerLegalNumber, inbound.OwnerLegalNumber) as VehicleOwnerLegalNumber
    FROM MTPL mtpl
    LEFT JOIN Inbound inbound on inbound.MtplId = mtpl.Id 
```