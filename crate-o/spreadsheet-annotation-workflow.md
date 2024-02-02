
@startuml

!include_many ../data-management-style.txt
title: Annotating someone else's archival collections\n(for de-colonization or other purposes)

start  


: Export the collection to a spreadsheet ;
     
note right
- Built in export eg UQ eSpace
- Harvest using OAI-PMH (most repo systems)
- via API
- Web scraping (last resort)
end note

partition #mistyrose "Workspace Activity" {
if (Format is Crate-O ready) then (no)
:Change Headers, calcualate fields;

else (yes)

endif


: Add annotations using Crate-O;
note left
- Using Spreadsheet or
- Using GUI Editor
end note

: Save collection annotations as RO-Crate ;
}
partition #palegreen "Repository Deposit" {
if (How to publish) then (As a self contained scholarly work)

: Publish to Zeonodo or similar ;
note left
RO-Crates are self-
contained and have a
built in web view.
Anyone can publish
their own annotation set
end note
else (as a website)

: Run your own portal ;
note right
We're working on lightweight 
ways to do this building on 
Tim Sherratt's work 
eg with Datasette
end note
endif
}

@enduml