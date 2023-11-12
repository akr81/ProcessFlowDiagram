```puml
!include crt_tool.pu
!$numbered = 1
!$target_length = 15
!$newline_at_space = 0
skinparam linetype polyline
skinparam linetype ortho

$entities("\
UDE #pink,\
cause1,\
cause2,\
cause3,\
")

$injection("\
,\
,\
")

$connect_result_from_causes("\
2 3 4 > 1,\
")
```