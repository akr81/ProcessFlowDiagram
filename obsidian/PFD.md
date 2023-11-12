```puml
!include pfd_tool.pu
!$numbered = 1
!$target_length = 15
!$newline_at_space = 0
left to right direction
skinparam linetype polyline
skinparam linetype ortho

$entities("\
out,\
in1#pink,\
in2,\
c:in3\
")

$p("2 3 4 > 1", "process#skyblue")
```