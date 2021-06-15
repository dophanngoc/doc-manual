### Install VScode
https://code.visualstudio.com/
### Install Java
https://www.java.com/ja/download/
### Install Graphvis
http://www.graphviz.org/download/
### Install PlanUML to vscode

![install plantuml to vscode](https://github.com/dophanngoc/doc-manual/blob/master/picture/noname77.png)

### Example:
#### Go to plantuml homepage to get syntax and features: https://plantuml.com/
1. Getting started with planuml

**Sample code 1:**
* Open vscode end paste the following code:
```
@startuml
scale 2
saturday are closed
sunday are closed

Project starts the 1st of january 2021
[Prototype design end] as [TASK1] lasts 19 days
[TASK1] is colored in Lavender/LightBlue
[Testing] lasts 14 days
[TASK1]->[Testing]

2021-01-18 to 2021-01-22 are named [End's committee]
2021-01-18 to 2021-01-22 are colored in salmon 
@endumll
```
* Save the file to local disk with *.pu* or *.wsd* tag name.
* Enter *Alt+D* or *F1* then enter *PlantUML: Preview Current Diagram*
![example 1](https://github.com/dophanngoc/doc-manual/blob/master/picture/noname80.png)
* To export the diagram to picture, Press *F1* then enter: *PlantUML: Export Current File Diagram*; *png*; the output should be at */Document/PlantUML/*

**Sample code 2:**
```
@startuml
scale 350 width
[*] --> NotShooting

state NotShooting {
  [*] --> Idle
  Idle --> Configuring : EvConfig
  Configuring --> Idle : EvConfig
}

state Configuring {
  [*] --> NewValueSelection
  NewValueSelection --> NewValuePreview : EvNewValue
  NewValuePreview --> NewValueSelection : EvNewValueRejected
  NewValuePreview --> NewValueSelection : EvNewValueSaved

  state NewValuePreview {
     State1 -> State2
  }

}
@enduml
```

![example 1](https://github.com/dophanngoc/doc-manual/blob/master/picture/noname81.png)
