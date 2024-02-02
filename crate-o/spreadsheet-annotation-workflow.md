
@startuml

!include_many ../data-management-style.txt
title: Annotating someone else's archival collections\n(for de-colonization or other purposes)

start  


: Export the collection to a spreadsheet ;
     
note left
- Built in export eg UQ eSpace
- Harvest using OAI-PMH (most repo systems)
- via API
- Web scraping (last resort)
end note

partition #mistyrose "Workspace Activity" {
if (Format is Crate-O ready) then (no)
:Change Headers, calculate fields;

else (yes)

endif


: Add annotations using Crate-O;
note left
Spreadsheet and/or 
web GUI data entry
end note

: Save collection annotations as RO-Crate ;
}
if (Publish) then  (yes) 
partition #palegreen "Repository Deposit" {
    if (Where?) then (Self contained scholarly work)
        : Zenodo or similar ;
        note left
        RO-Crates are self-
        contained and have a
        built in web view.
        Anyone can publish
        their own annotation set
        end note
    else (As a collection)
        : DIY or shared Discipline Repo;
        note right
        We're working on lightweight 
        ways to do this building on 
        Tim Sherratt's work 
        eg with Datasette
        end note

    endif
}
else (no)
:save locally;
endif

@enduml