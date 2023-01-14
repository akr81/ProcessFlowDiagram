```puml
!include ../ProcessFlowDiagram/pfd_tool.pu
!$numbered = 1
skinparam linetype polyline
skinparam linetype ortho
skinparam nodesep 20
skinparam ranksep 20

$entities("\
UDE #pink,\
cause1,\
cause2,\
cause3,\
")

$connect_result_from_causes("\
1 < 2 3 4,\
")
```