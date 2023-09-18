```puml
!import ../ProcessFlowDiagram/pfd_lib.zip
!include req_tool.pu
!$numbered = 1
!$target_length = 15
!$newline_at_space = 1
!$detailed = 1
skinparam linetype polyline
skinparam linetype ortho

$req("\
RequirementA#pink,Detailed description...,\
RequirementB, ,\
RequirementC, ,\
RequirementD, ,\
")

$block("\
ImplementAlpha,\
")

$connect_req("\
2 3 + 1,\
4 d 2,\
5 s 3,\
")
```