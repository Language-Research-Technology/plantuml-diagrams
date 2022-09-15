```plantuml:ldaca-paradisec-architecture
@startuml
title: PARADISEC Architecture

package "Nabu" {
   database "Metadata Database"  {

   }

}

database "NFS Storage" {


}


@enduml
```

```plantuml:ldaca-conversation
@startuml

rectangle "@id: conversation.csv" as csv {


}

rectangle "@id: arcp://name,somedomain/csv_schema" as schema {

   
}

rectangle "@id: csv_schema.json" as json {

   
}

csv -> schema : conformsTo
schema -> json : sameAs
@enduml
```

```plantuml:ldaca-paradisec-workspaces
@startuml

cloud Workspaces as ws {


}

cloud "Archival Repositories" as ar {

}


ws -right-> ar : Data deposit
ar -left-> ws :  Data use


@enduml
```

```plantuml:ldaca-data-storage-options
@startuml
title: Data storage options\none crate, many parts (Collection A)\nvs many crates (Collection B)

database "OCFL repository" {
rectangle "Object" as c1 {
   [Collection A] as ca 
   [obj1a]
   [obj2a]
   [obj3a]
}
rectangle "Collection B" as cb {

}
rectangle obj1b {

}
rectangle obj2b {
   
}
rectangle obj3b {
   
}

obj1a -> ca: memberOf
obj2a -> ca: memberOf
obj3a -> ca: memberOf


obj1b -> cb: memberOf
obj2b -> cb: memberOf
obj3b -> cb: memberOf


}


@enduml
```


```plantuml:ldaca-atap-architecture
@startuml
skinparam defaultFontSize 18
title: ATAP Architecture

rectangle "Text Analytics Platform" as atap {
rectangle "Notebook 1" as n1 {

}

rectangle "Notebook 1" as n2 {
    
}
}
database "OCFL Repository" as repo {


}

rectangle "API" as oni {

}

rectangle "Access control" as ac {

}



actor researcher

researcher -down-> n1 : Use Notebook
n1 -down-> oni : Fetch Metadata 
n1 -down-> oni : Fetch data streams
oni -> ac : Authenticate each request
oni <--down-- repo : Index  (w/ access licenses)
oni -down-> repo : Fetch
@enduml
```

```plantuml:ldaca-ro-crate-profile
@startuml

rectangle "RO-Crate Profile (this document)" {
rectangle " #Collection" as procoll
rectangle "    #Object" as proobj
}


rectangle "Collection" as coll {
}
rectangle "Object" as obj {
}
rectangle "File" as file {
}
coll -----> coll : hasMember
coll -> coll : memberOfq


coll -down-> obj : hasMember
obj -up-> coll : memberOf
coll -up-> procoll : conformsTo
obj -right-> proobj : conformsTo
obj -down-> file : hasPart


@enduml
```


```plantuml:ldaca-self-contained-corpus
@startuml
title: Self contained corpus crate with all resources
package "ROCrate: My Corpus" {
   rectangle "RepositoryCollection: MyCorpus" as cc {

   }
   rectangle "RepositoryObject, Dialogue: #1" as o1 {

   }
   rectangle "RepositoryObject, Dialogue: #2" as o2 {

   }
   rectangle "File, Audio:  files/audio1.wav" as a1
   rectangle "File, OthographicTranscription: files/csv1.csv" as csv1
   rectangle "Dataset: files" as files



   rectangle "RepositoryCollection: Collection A" as c2 {

   }
   cc -down-> c2 : hasMember

   c2 -down-> o1 : hasMember
   c2 -down-> o2 : hasMember

   o1 -down-> a1 : hasPart
   o1 -down-> csv1 : hasPart


   cc -down-> files : hasPart
   files -down-> csv1 : hasPart
   files -down-> a1 : hasPart


}

@enduml
```

```plantuml:ldaca-complete-corpus-collection
@startuml
title: Complete corpus collection metadata-only crate w/ links to object packages
package "name My Corpus" as mc {
   rectangle "@type: [Dataset, RepositoryCollection]\nconformsTo: ./#Collection" as cc {

   }
}

package "ROCrate: Collection A" as cAc {

   rectangle "@type:  [Dataset, RepositoryCollection]\nconformsTo: ./#Collection" as ca {

   }
  
}


package "ROCrate: Collection B" as cBc {

   rectangle "@type:  [Dataset, RepositoryCollection]\nconformsTo: ./#Collection" as cb {

   }
  
}


package "RO Crate: Interview #1" as ec2 {
  rectangle "@type: RepositoryObject\nconformsTo: ./#Object\nmodailty: Speech\nlingusiticGenre: Interview" as eo1 {

   }
   rectangle "@type: [File, PrinaryText]:  files/audio1.wav" as ea1
   rectangle "@type: [File, Annotation]: files/csv1.csv" as ecsv1
   }



eo1 -down-> ea1 : hasPart
eo1 -down-> ecsv1 : hasPart

eo1 -up-> ca : memberOf
ca -up-> cc : memberOf
cb -up-> cc : memberOf

note right of cc : Repository Collection:\n- MUST contain descriptive metadata\n  for the corpus/collection\n- MAY contain hasMember relationships\n  listing child Collections or Objects

 
note bottom of ec2 : Individual object with file data

@enduml
```

```plantuml:ldaca-ro-crate-profile-this-document
@startuml

rectangle "RO-Crate Profile (this document)" {
rectangle  " #Corpus"  as procorp
rectangle " #Collection" as procoll
rectangle "    #Object" as proobj
}

rectangle "Corpus" as corp {
}
rectangle "Collection" as coll {
}
rectangle "Object" as obj {
}
rectangle "File" as file {
}
corp -down-> coll : hasMember
coll --> coll : hasMember
coll -down-> obj : hasMember
corp ---down---> obj : hasMember
note top of corp: pcdm:Collection / RO-Crate RepositoryCollection 
note left of coll : pdcm:Collection / RO-Crate RepositoryCollection
note left of obj : pdcm:Object / RO-Crate RepositoryObject

note right of file : schema:MediaObject / RO-Crate File
corp -up-> procorp : conformsTo
coll --up---> procoll : conformsTo
obj -right-> proobj : conformsTo
obj -down-> file : hasFile


@enduml
```

```plantuml:ldaca-ro-crate-profile-this-document
@startuml
skinparam defaultFontSize 18

title: Super Simple License Server w/ Github Authentication & Authorization
actor       researcher     as res
entity    repository    as rep
entity    "License Server"    as gls
entity "https://doi.org/x/some-notebook" as c
database "groups" as gps
res -> rep : Can I have https://doi.org/x/some-notebook?
rep -> c : What's your licence?
c -> rep : Accessible to github org MyTroveData 
rep -> rep : Is that URL on the list of trusted sites?
rep -> rep : Yep
rep -> res : Authenticate yourself to license service
res -> gls : Logs user in using Github  
gls-> gps : User in MyTroveData org?
res -> gls : Yep
gls -> res : (To web browser) Here's a token for your browser that will let the repo get your group-memberships
res -> rep : (via browser) Here's a token
rep -> gls : I have this token
gls -> rep : yep
rep -> c : fetch
rep -> res : Here's the RO-Crate
@enduml

@startuml
skinparam defaultFontSize 18

title: Simple repository access 
actor       researcher     as res
entity    repository    as rep
entity "Dataset:\n   https://doi.org/x/some-dataset\nHeld in repository" as c
entity    "CILogon"    as ci

res -> rep : Can I have https://doi.org/x/some-crate?
rep -> c : Internal repository lookup:\nwhat's your licence?
c -> rep : Accessible to people in group G1 on CILogon 
rep -> rep : Is that on my list of trusted sites?
rep -> rep : Yep
rep -> res : Authenticate yourself over at CILogon
res -> ci : Logs in 
ci -> rep : Calls repo back "User is a member of G1, G2, G3"
rep -> rep : OK, user is a member of G1
rep -> c : fetch
rep -> res : Here's the data
@enduml
```
```plantuml:ldaca-simple-repository-access
@startuml
skinparam defaultFontSize 18

title: Simple repository access 
actor       researcher     as res
entity    repository    as rep
entity "https://doi.org/x/some-dataset" as c
entity    "CILogon"    as ci

res -> rep : Can I have https://doi.org/x/some-crate?
rep -> c : Internal repository lookup:\nwhat's your licence?
c -> rep : Accessible to people in group G1 on CILogon 
rep -> rep : Is that on my list of trusted sites?
rep -> rep : Yep
rep -> res : Authenticate yourself over at CILogon
res -> ci : Logs in 
ci -> rep : Calls repo back "User is a member of G1, G2, G3"
rep -> rep : OK, user is a member of G1
rep -> c : fetch
rep -> res : Here's the data
@enduml
```

```plantuml:ldaca-sketch-potential-authorization
@startuml
skinparam defaultFontSize 18

title: A sketch of POTENTIAL authorization and authentication required by FAIR principle A1.2 
actor       researcher     as res
entity    repository    as rep
entity    "Group Licence Server"    as gls
entity "https://doi.org/x/some-crate" as c
database "groups" as gps
res -> rep : Can I have https://doi.org/x/some-crate?
rep -> c : What's your licence?
c -> rep : Accessible to ppl who agree to SS licence
rep -> rep : Is that URL on the list of trusted sites?
rep -> rep : Yep
rep -> res : Authenticate yourself to the the group-licence-server
res -> gls : Logs in 
gls-> gps : User in SS?
gps -> gls : Nope
gls -> res : Do you agree to these SS license terms?
res -> gls : Yep
gls -> gps : Add user to SS license group (TTL 100days)
gls -> res : (To web browser) Here's a token for your browser that will let the repo get your group-memberships
res -> rep : (via browser) Here's a token
rep -> gls : I have this token
gls -> rep : yep
rep -> c : fetch
rep -> res : Here's the RO-Crate
@enduml
```





```plantuml:ldaca-alveo-rescue
@startuml
title: Alveo rescue
database "Alveo" {


rectangle "Object 1" {
}

rectangle "..." as asdkj {
}


rectangle "Object n" {
}

}




rectangle "Conversion Script" {

}

"Conversion Script" <-up- "Object 1" : Read

"Conversion Script" --down--> "RO-Crate Object 1" : Deposit

rectangle "OCFL Repository"  {

rectangle "RO-Crate Object 1" {
}

rectangle "..." {
}


rectangle "RO-Crate Object n" {
}

}
@enduml
```

```plantuml:ldaca-data-triangle
@startuml
title: LDaCA data triage
start

if (Data is in a service) then (yes)
 
  if (Service can be sustainably Managed) then (yes)
   
         if (Service needs to  be migrated) then (yes)
            :Organize Migration;
         else (no)
         endif
         :Set up Harvester;
         stop
  else (no)
       
  endif
    
endif
@enduml
```
```plantuml:ldaca-persistent-ids
@startuml
title: LDaCA Persistent IDs (PIDs)

start

if (Data has a custodial Organization) then (yes)
 
  if (Org has PID policy & governance) then (yes)
         :Use existing policy;
         stop
  else (no)
      :Use LDaCA defaults;
      :ARCP IDs for context, demographics, collectionEvents etc;
      while (Can Identify responsible party within ORG to manage PIDs) is (no)
            if (Content has existing PIDs) then (yes)
                :Use exisiting PIDs;
            else (no)
               :ARCP IDs for collections, objects and files;
            endif
       endwhile (yes)
       switch (Type of deposit)
            case ( Completed speech study or corpus )
               :DOI at study/collection level;
               :ARCP IDs for objects and files;
            case ( Continuing deposit of objects ) 
               :DOI at object level;
               :ARCP IDs for files;
         endswitch
       
   

  endif
else (no)
   :Continue negotiations;
   stop
endif


     
 



stop
@enduml
```
```plantuml:ldaca-data-publications
@startuml
title: Data Publications

package "Git Service (eg github)" as git {
   package "Study or Method Repository" as repo {
   package "RO-Crate Metadata" {
      rectangle "name: My Thesis Data\nauthor: https://orcid.org/12345\n" {
         rectangle "hasPart" {
            rectangle "Notebook 1" {

            }
            rectangle "Notebook 2" {
               
            }
         }


      }

   }
   package "Notebook 1" as n1 {

   }
   package "Notebook 2" as n2 {
      
   }
   
   }

   
}



@enduml
```

```plantuml:ldaca-alveo
@startuml Alveo
title: Alveo model of licences

actor       researcher     as res
participant  Alveo    as rep
database "Index of holdings" as i
participant   "licence Server"    as gls


res -> rep : Log in (username/password)
rep -> gls : Get list of licences for user
gls -> rep : [licence-list]
rep -> i : Show browse screen (for data with licences in [licence-list])
i -> rep : <browse-data>
rep -> res : Serves page showing data user is licensed to see
@enduml

@startuml
database "Sydney Speaks" as ss {

rectangle "Content Dir 1" as d1 {

}
rectangle "Content Dir n" as d2 {

}
}

database "Horvath Collection" as hc {

rectangle "Content Dir 1" as h1 {

}
rectangle "Content Dir n" as h2 {

}
}



rectangle "Conversion Script" {

}

"Conversion Script" <-up- "d1" : Read

"Conversion Script" <-up- "h1" : Read


rectangle "OCFL Repository"  {

rectangle "RO-Crate Object 1" as ss1 {
}

rectangle "RO-Crate Object 2" as hc1 {
}

rectangle "..." {
}


rectangle "RO-Crate Object n" {
}

}


"Conversion Script" --down--> "RO-Crate Object ss/1" ss1 : Deposit
"Conversion Script" --down--> "RO-Crate Object hc/1"  hc1: Deposit
@enduml
```
```plantuml:ldaca-repository-services
@startuml
top to bottom direction
title: LDACA Repository Services


cloud "Conversion Services"  as cs {
rectangle "OCR" as ocr {

}
rectangle "Your service here"

}

cloud "Vocab Services" as vocab {
[Orcid]
[ROR]
[Language Ontologies] as lo
[Existing Entities] as ee

}

cloud "Workspaces" {
rectangle "From The Page" as ftp {

}

rectangle "Cloudstor" {

rectangle "File Sync n Share" {

}
rectangle "Swan" {

}

}


}


rectangle Authorization as auth {
database "Local Auth" as la {
}
[AAF]
[Google]
[Licence Authorizer] as lic

}



rectangle "Curation App" as app {

rectangle "Data Day Spa"  as spa {
rectangle "Uploaded Files" as files {

}
}

rectangle "Data Description" as describo {

}


database "Items" as itemdb {

}



}

database "Main Repository\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n" as repo {

}

rectangle Portal as port {
database "Index" as i {


}
rectangle "Web UI" as web {


}

rectangle "API" as api {
}

rectangle "Inbox" as inbox


}



repo -down-> vocab : Lookup
app --up--> auth : Authorise users
i ----> repo : Index content (with license info of course)
ee ----> repo : Look up collections / people etc
actor "Repo Manager" as rm
port --down--> lic : Check keys/credentials for EVERY access
spa -down-> cs : Batch conversion
describo -> itemdb : Make RO-Crate
auth ---down---> auth : Authenticate users
itemdb -down-> api : Fetch latest version of item 
itemdb ---> itemdb : Mint new item
itemdb --down--> inbox : Deposit item
spa --up--> Workspaces : Send data to asynchronous services for cleaning/analysis
spa --down--> itemdb : Populate item with text
rm -up-> inbox : Approve / reject
inbox -> repo : Deposit
@enduml
```
```plantuml:ldaca-aggregation
@startuml
top to bottom direction
title: LDaCA Aggregation

rectangle Authorization as auth {
database "Local Auth" as la {
}
[AAF]
[Google]
[Licence Authorizer] as lic

}

database "Repository 1\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n" as repo1 {

}

database "Repository 2\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n" as repo2 {

}

database "Repository 3\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n" as repo3 {

}

rectangle Portal as port {
database "Index" as i {


}
rectangle "Web UI" as web {


}

rectangle "API" as api {
}

rectangle "Inbox" as inbox


}


port --up--> auth : Authorise users
i ----> repo1 : Index content
i ----> repo2 : Index content 
i ----> repo3 : Index content 


@enduml
```

```plantuml:ldaca-step-1
@startuml
Title: Step 1 - Fetching data over the wire
rectangle Workbench {
rectangle "Notebook 1" as w1 {
}
}


rectangle Webserver {
rectangle "Crate 1 (open)" as c1 {
}
rectangle "Crate 2 (open)" as c2 {
}



}

w1 -> c1 : Fetch
@enduml
```
```plantuml:ldaca-step-2
@startuml
Title: Step 2 - Adding scalability
rectangle Workbench {
rectangle "Notebook 1" as w1 {
}
}


rectangle Webserver {
rectangle "Simple OCFL front end" as ui {

}

rectangle "OCFL Repo" as ocfl {



rectangle "Crate 1 (open)" as c1 {
}
rectangle "Crate 2 (open)" as c2 {
}
}



}

w1 -> ui : Fetch Crate 1
ui <-> ocfl : Fetch Crate 1
ui --> c1 : Fetch
@enduml
```

```plantuml:ldaca-step-3
@startuml
Title: Step 3 - Adding discoverability
rectangle Workbench {
rectangle "Notebook 1" as w1 {
}
}

actor "Researcher 1" as r1


rectangle Webserver {
rectangle "OCFL search/discovery" as ui {

}

rectangle "OCFL Repo" as ocfl {



rectangle "Crate 1*" as c1 {
}
rectangle "Crate 2*" as c2 {
}
rectangle "Notebook 1*" as n1 {
}
rectangle "Notebook 2*" as n2 {
}
}

rectangle "Search results" as sr

note bottom of ocfl: "*" = Open licence

}

w1 -> ui : Fetch Crate 1
ui <-> ocfl : Fetch Crate 1
ui --> c1 : Fetch
r1 -> ui : Show me notebooks for wordcloud
ui -> sr : Show wordcloud notebooks

r1 -> sr : Invoke Notebook 1
sr -> Workbench : "Fire up Notebook 1 for Researcher 1
@enduml

@startuml
Title: Step 4 - Deposit
rectangle Workbench {
rectangle "Notebook 1" as w1 {
}
}

actor "Researcher 1" as r1

actor "Researcher 2" as r2

rectangle Webserver {
rectangle "OCFL search/discovery" as ui {

}

rectangle "OCFL Repo" as ocfl {



rectangle "Crate 1*" as c1 {
}
rectangle "Crate 2*" as c2 {
}
rectangle "Notebook 1*" as n1 {
}
rectangle "Notebook 2*" as n2 {
}

rectangle "My Wordcloud Notebook" as mwn {
}
}


note bottom of ocfl: "*" = Open licence

}

rectangle "Licence Authorizer" as auth {

}

r1 -down-> w1: Save as My wordcloud notebook
Workbench -down-> ocfl : Deposit *My Wordcloud Notebook* with single-user license
mwn -up-> w1 : Provenance -- derived from

r2 -> ui : Fire up My Word Cloud Notebook

ui ---> auth : Is r2 allowed to see mwn
auth -> ui : No
ui ----> r2 : NO!
@enduml
```
```plantuml:ldaca-step-3-1
@startuml

actor "Speaker 1" as s1
actor "Speaker 2" as s2

rectangle "RO-Crate Profile (this document)" {
rectangle "#Collection" as procoll
rectangle "#Object" as proobj
}

rectangle "Conversation" as c1 {

 
 rectangle "@type: [RepositoryObject]" 

}
rectangle "Audio File" as audio1 {
   rectangle "@id: [file.wav]\n@type: [File, PrimaryText]\nmodality: [Speech]\ncommunicationGenre: [Interview]\nresourceType: [PrimaryText]\nencodingFormat: [audio/x-wav]" 


}
rectangle "PDF File" as pdf1 {
   rectangle "@id: [file.pdf]\n@type: [File, Annotation]\nannotationType: [Transcription]\nmodality: [Orthography]\nresourceType: [DerivedText]\nencodingFormat: [text/pdf]" as pt 

}

rectangle "CSV File" as csv1 {
   rectangle "@id: [file.csv]\n@type: [File, Annotation, DerivedText]\nannotationType: [Transcription]\nmodality: [Orthography]\nresourceType: [DerivedText]\nencodingFormat: [text/csv]" 
}

c1 -down-> pdf1 : hasPart

pdf1 -up-> audio1 : annotationOf
c1 -down-> audio1 : hasPart
c1 -down-> csv1 : hasPart
csv1 -up-> audio1 : annotationOf
csv1 -up-> pdf1 : derivedFrom

c1 -left-> s1 : interviewer
c1 -right-> s2 : interviewee
c1 ->  proobj: conformsTo


@enduml
```
```plantuml:ldaca-step-3-2
@startuml

rectangle "Work (Class: Dataset)" as w {

}

@enduml
```
```plantuml:ldaca-step-3-3
@startuml

rectangle "Work (Class: Dataset)" as w {

}

actor "Contributor (Class: Person)" as c

w -> c : compiler (Class: Property)

@enduml
```
```plantuml:ldaca-step-3-4
@startuml

rectangle "Work" as w {

}

actor "Contributor" as c

w -> c : compiler 
w -> c : depositor 



@enduml
```
```plantuml:ldaca-step-3-5
@startuml

rectangle "Work" as w {

}

actor "Contributor " as c

w -> c : hasCompiler 
w <- c : compilerOf 


@enduml
```
```plantuml:ldaca-step-3-6
@startuml

rectangle "Work" as w {

}

actor "Contributor 1" as c1
actor "Contributor 2 " as c2

rectangle "CreateActionSpeak" as a1 {
   [date: 2022-01-02]
}

rectangle "UpdateActionDeposit" as a2 {
   [date: 2022-02-02]
}


rectangle "CreateActionRecord" as a3 {
   [date: 2022-02-02] as di
}

rectangle "Video Camera" as cam {

}
a1 -down-> w : result
a1 -up-> c1 : agent

a2 -down-> w : result
a2 -up-> c2 : agent

a3 -down-> w : result
a3 -up-> c2 : agent
a3 -down-> cam : instrument


@enduml
```