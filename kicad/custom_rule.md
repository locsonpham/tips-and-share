# Custom rules
## Zone clearance with board-edge
```
(version 1)
(rule "Clearance zone to board-edge"
  (constraint edge_clearance (min 10mil))
  (condition "A.Type =='Zone' "))

```