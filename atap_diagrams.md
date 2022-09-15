
@startuml

title: ATAP Ecosystem - components

skinparam cloud<<workspace>> {
    backgroundcolor Pink

}

skinparam cloud<<repository>> {
    backgroundcolor LightGreen


}


rectangle "ATAP Website" as aw {

}

cloud Workspaces as ws <<workspace>> {

database "Git Server" as git {

}

rectangle "Binder Hub"  as bh {
}

rectangle "Nectar Virtual Compute"  as nectar {
}

}

bh -down-> git : Fetch notebook
bh -down-> nectar : Launch Notebook
cloud "Archival Repositories" as ar <<repository>> {

rectangle "ATAP Data Portal" as adp {

}
database "ATAP repo" as a {


}

rectangle "Zenodo etc" as z {

}

rectangle "Institutional Data Repo" as z {

}

}

adp -left-> git : Index Notebooks

adp -down-> a : Index and serve
aw -down-> adp : Link
aw -down-> ws : Link
@enduml


@startuml

title: ATAP Ecosystem - Publishing


cloud Workspaces as ws {

database "Git Server" as git {

}



}

cloud "Archival Repositories" as ar {
rectangle "ATAP Data Portal" as adp {

}
database "ATAP repo" as a {


}

rectangle "Zenodo" as z {

}

rectangle "Institutional Data Repository" as idp {

}

rectangle "Institutional Data Repo" as z {

}

}

@enduml

@startuml
title: Full ATAP environment
rectangle "ATAP Website" as aw {

rectangle "Registry"  {

}

rectangle "Tutorials" {
 rectangle "Notebook 2 Tutorial" as  nt2  {

}

}

}

nt2 ---down--> tin2 : LAUNCH


cloud Workspaces as ws {

database "Git Server" as gs {
rectangle "Notebook 1" as  n1 {

}
rectangle "Notebook 2" as  n2 {

}

rectangle "Notebook 2 (modified)" as  n2m {

}



}

rectangle "Binder Hub"  as bh {

rectangle "Instance of N2" as  1in2 {

}


rectangle "Instance of N2" as  tin2 {

}
rectangle "Instance of N2 (modified)" as  2in2 {

}


}
}

cloud "Archival Repositories" as ar {
database "ATAP repo" as a {

rectangle "Notebook Registry" as nr {
rectangle "Notebook 1" as  n1r {

}
rectangle "Notebook 2" as  n2r {

}
}

}

database "LDaCA repo\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n" as l {
}

}


ws --right--> ar : Data deposit
a --left--> ws :  Data use
note bottom of gs : Eg github / gitlab
ar --> gs : Harvest metadata & index
n2r ----> 1in2 : LAUNCH!!
n2 ----> 2in2 : LAUNCH!!
2in2 ---> n2m: Push
@enduml