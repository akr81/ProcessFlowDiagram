```puml
!include ../ProcessFlowDiagram/pfd_tool.pu
!$numbered = 1
left to right direction
skinparam linetype polyline
skinparam linetype ortho
skinparam nodesep 20
skinparam ranksep 20

$entities("\
out,\
in1#pink,\
in2,\
c:in3\
")

$p("2 3 4 > 1", "process#skyblue")
```