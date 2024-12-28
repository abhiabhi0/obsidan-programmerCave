
## Requirements

* A pen is anything that can write.
* Pen can be Gel, Ball, Fountain, Marker.
* Ball Pen and Gel Pen have a Ball Pen Refill and a Gel Pen Refill respectively to write.
* A refil has a tip and an ink.
* Ink can be of different colour
* A fountain pen has an Ink.
* Refil has a radius. 
* For fountain pen, its tip has a radius.
* Each pen can write in a different way.
* Some pens write in the same way.
* Every pen has a brand and a name.
* Some pens may allow refilling while others might not.

## Entities and Attributes

* Pen
  * Brand
  * Name
  * Type (Gel, Ball, Fountain, Marker)
  * Price
*  Refill
  * Type (Ball, Gel)
  * Ink
  * Nib
* Ink
  * Colour
  * Brand
  * Type (Gel, Ball, Fountain)
* Nib
  * Radius
  * Type (Fountain, Ball, Gel)  

### Different types of pens
* Gel Pen
  * Type - `Gel`
  * Refill
    * Type - `Gel`
    * Nib - `Gel`
    * Ink
      * Type - `Gel`
    * Refillable - `Yes`

* Ball Pen
  * Type - `Ball`
  * Refill
    * Type - `Ball`
    * Nib - `Ball`
    * Ink
      * Type - `Ball`
    * Refillable - `Yes`

* Throwaway Pen
  * Type - `Throwaway`
  * Refill
    * Type - `Ball`
    * Nib - `Ball`
    * Ink
      * Type - `Ball`
    * Refillable - `No`

* Fountain Pen
  * Type - `Fountain`
  * Ink
    * Type - `Fountain`
  * NiB
    * Type - `Fountain`


## Avoiding LSP using abstract classes

```mermaid
classDiagram
    class Pen {
    <<abstract>>
    -String brand
    -String name
    -PenType type
    -double price
    -WritingStrategy writingStrategy
    +write() void
}

class RefillablePen {
    <<abstract>>
    -Refill refill
    +changeRefill(Refill) void
    +getRefill() Refill
    +isRefillable() boolean
}

class NonRefillablePen {
    <<abstract>>
    -Ink ink
    -Nib nib
    +changeInk(Ink) void
}

class GelPen {
    +write() void
    +changeRefill(Refill) void
    +getRefill() Refill
    +isRefillable() boolean
}

class BallPen {
    +write() void
    +changeRefill(Refill) void
    +getRefill() Refill
    +isRefillable() boolean
}

class FountainPen {
    +write() void
}

Pen <|-- RefillablePen
Pen <|-- NonRefillablePen
RefillablePen <|-- GelPen
RefillablePen <|-- BallPen
NonRefillablePen <|-- FountainPen

```

### Java Code
[Pen class with abstract classes](https://github.com/kanmaytacker/design-questions/tree/master/src/main/java/com/scaler/lld/pen/abstractclasses)

### Improvements
  * `Liskov Substitution Principle` is followed since `FountainPen` does not have a refill, and it throws an exception when `changeRefill` is called.
  * No field duplication in child classes.

### Problems
  * Behaviour is tied to the class hierarchy. Adding a new type of pen requires changing the class hierarchy.

## Avoiding LSP violation using interface

```mermaid
classDiagram
    class Pen {
        <<abstract>>
        -String brand
        -String name
        -PenType type
        -double price
        -WritingStrategy writingStrategy
        +write() void
    }

    class RefillPen {
        <<interface>>
        +changeRefill(Refill) void
        +getRefill() Refill
        +isRefillable() boolean
    }

    class GelPen {
        -Refill refill
        +write() void
        +changeRefill(Refill) void
        +getRefill() Refill
        +isRefillable() boolean
    }

    class BallPen {
        -Refill refill
        +write() void
        +changeRefill(Refill) void
        +getRefill() Refill
        +isRefillable() boolean
    }

    class FountainPen {
        -Ink ink
        -Nib nib
        +write() void
    }

    Pen <|-- GelPen : extends
    Pen <|-- BallPen : extends
    Pen <|-- FountainPen : extends

    RefillPen <|-- GelPen : implements
    RefillPen <|-- BallPen : implements
```


### Java Code
[Pen class with interface](https://github.com/kanmaytacker/design-questions/blob/master/src/main/java/com/scaler/lld/pen/withinterface)

### Problems
  * Field duplication in child classes.