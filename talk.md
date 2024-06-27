---
title: barKod - Idea to Product, Architectural Perspective
theme: dracula
revealOptions:
  transition: "fade"
---

## Idea to Product: Architectural Perspective

**Srđan Đorđević**

[shellmonk.io](https://shellmonk.io)

![NTPNS](/assets/ntp_small.png) ![BarKod](/assets/barkod_logo.png)

---

## $ whoami

```json
{
  "name": "Srđan Đorđević",
  "age": 34,
  "ocupation": "Senior Software Engineer",
  "works_at": "Pollard Digital Solutions",
  "location": "Novi Sad",
  "origin": "Paraćin"
}
```

---

## Introduction

- What is software architecture? <!-- .element: class="fragment" data-fragment-index="1" -->
- Why do we need it and how to utilize it? <!-- .element: class="fragment" data-fragment-index="2" -->
- Tips, tricks and tools to avoid disaster <!-- .element: class="fragment" data-fragment-index="3" -->

---

## What is software architecture?

> "The craft of software architecture manifests in the ability of architects to analyze business and domain requirements along with other important factors to find a solution that balances all concerns optimally" - **Neil Ford**

---

## It wasn't alway like this

![Waterfall vs Agile](/assets/waterfall-vs-agile.jpg)

---

## Agile SDLC

![SDLC](/assets/sdlc.png)

---

## 1. Discovery

- Clients often have no idea what they want <!-- .element: class="fragment" data-fragment-index="1" -->
- Communication is the key <!-- .element: class="fragment" data-fragment-index="2" -->
- Fast iterations <!-- .element: class="fragment" data-fragment-index="3" -->
- Identifying functional requirements <!-- .element: class="fragment" data-fragment-index="4" -->

---

### Prototyping

- Clients love prototypes <!-- .element: class="fragment" data-fragment-index="1" -->
  - Prototypes are great communication tool
- Tools for prototyping <!-- .element: class="fragment" data-fragment-index="2" -->
  - [Figma](https://www.figma.com/) <3
  - NoCode / LowCode tools ([Bubble](https://bubble.io/))
- Teach your clients the difference between prototype and finished product as soon as possible! <!-- .element: class="fragment" data-fragment-index="3" -->

---

## Document everything

- Ask questions! Ask questions! Ask questions!
- If it's not written, it never happened

<video width="550" autoplay loop muted>
    <source src="/assets/everything-meme.webm" type="video/webm">
</video>

---

## 2. Design

![Life of an engineer](/assets/architecture.jpeg)

---

## Architecture != System Design

![GigaChad](/assets/gigachad.jpg)

---

## Architectural Decisions

> "The perfect kind of architecture decision is the one which never has to be made" - **Robert C. Martin**

- Architectural decisions are <!-- .element: class="fragment" data-fragment-index="1" --> **hard** <!-- .element: class="fragment" data-fragment-index="1" -->
  - Wrong decisions cost a lot <!-- .element: class="fragment" data-fragment-index="2" -->
- How do I evaluate options? <!-- .element: class="fragment" data-fragment-index="3" -->
  - You'll never have enough information

---

## Architectural characteristics and (N)FRs

- Functional vs non-functional requirements (-ilities) <!-- .element: class="fragment" data-fragment-index="1" -->
  - accessibility <!-- .element: class="fragment" data-fragment-index="2" -->
  - elasticity <!-- .element: class="fragment" data-fragment-index="3" -->
  - reliability <!-- .element: class="fragment" data-fragment-index="4" -->
  - security <!-- .element: class="fragment" data-fragment-index="5" -->
  - scalability <!-- .element: class="fragment" data-fragment-index="6" -->
  - ... (100 rows omitted) <!-- .element: class="fragment" data-fragment-index="7" -->
- [Wikipedia - Non-functional requirement](https://en.wikipedia.org/wiki/Non-functional_requirement) <!-- .element: class="fragment" data-fragment-index="7" -->

---

## Chosing the right ones

![Architecture Characteristic Worksheet](/assets/arch-char-worksheet.png)

---

## Protip: Evolvability

- Evolutionary architecture
- Fitness functions
  - ArchUnit, NetArchTest, etc.

Note: Spring modular monolith

---

## Code Example

```java
// ArchUnit example
@Test
public void layeredArchitectureTest() {
  JavaClasses jc = new ClassFileImporter().importPackages("com.barkod.talk");
  Architectures.LayeredArchitecture layeredArchitecture =
    layeredArchitecture()
      .consideringAllDependencies()
      .layer("Controller").definedBy("..controller..")
      .layer("Service").definedBy("..service..")
      .layer("Persistence").definedBy("..persistence..")
      .whereLayer("Controller").mayNotBeAccessedByAnyLayer()
      .whereLayer("Service").mayOnlyBeAccessedByLayers("Controller")
      .whereLayer("Persistence").mayOnlyBeAccessedByLayers("Service");

  layeredArchitecture.check(jc);
}
```

---

### Architectural styles

![Architectural Styles](/assets/arch-styles.png)

---

### Further design

- Digging deeper <!-- .element: class="fragment" data-fragment-index="1" -->
- Choose a style <!-- .element: class="fragment" data-fragment-index="2" -->
- Identify patterns <!-- .element: class="fragment" data-fragment-index="3" -->
- Document everything <!-- .element: class="fragment" data-fragment-index="4" -->
  - Version control systems are our friends (Git)
  - Diagrams as code (PlantUML)
  - **C4**

---

## C4 (not the explosive)

![C4](/assets/c4.png)

---

## Code example: PlantUML code

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

LAYOUT_WITH_LEGEND()

title System Context diagram for Internet Banking System

Person(customer, "Personal Banking Customer", "A customer of the bank, with personal bank accounts.")
System(banking_system, "Internet Banking System", "Allows customers to view information about their bank accounts, and make payments.")

System_Ext(mail_system, "E-mail system", "The internal Microsoft Exchange e-mail system.")
System_Ext(mainframe, "Mainframe Banking System", "Stores all of the core banking information about customers, accounts, transactions, etc.")

Rel(customer, banking_system, "Uses")
Rel_Back(customer, mail_system, "Sends e-mails to")
Rel_Neighbor(banking_system, mail_system, "Sends e-mails", "SMTP")
Rel(banking_system, mainframe, "Uses")
@enduml
```

---

## Code example: Rendered

![PlantUML example rendered](/assets/c4_example.png)

---

## 3. Development

- Keep an eye on changes and requirements <!-- .element: class="fragment" data-fragment-index="1" -->
- Again, fitness function <!-- .element: class="fragment" data-fragment-index="2" -->
- **Architecture decision log** <!-- .element: class="fragment" data-fragment-index="3" -->
  - Architecture decision records (ADRs) <!-- .element: class="fragment" data-fragment-index="3" -->

---

## ADR template example

```md
# Decision record template by Michael Nygard

This is the template in [Documenting architecture decisions - Michael Nygard](http://thinkrelevance.com/blog/2011/11/15/documenting-architecture-decisions).
You can use [adr-tools](https://github.com/npryce/adr-tools) for managing the ADR files.

In each ADR file, write these sections:

# Title

## Status

What is the status, such as proposed, accepted, rejected, deprecated, superseded, etc.?

## Context

What is the issue that we're seeing that is motivating this decision or change?

## Decision

What is the change that we're proposing and/or doing?

## Consequences

What becomes easier or more difficult to do because of this change?
```

---

## Duties of the architect in development phase

- Keep clients happy <!-- .element: class="fragment" data-fragment-index="1" -->
- Keep developers happy <!-- .element: class="fragment" data-fragment-index="2" -->
- Own sources of truth about the system <!-- .element: class="fragment" data-fragment-index="3" -->
- Steer development in the right direction <!-- .element: class="fragment" data-fragment-index="4" -->
- Try not to die <!-- .element: class="fragment" data-fragment-index="5" -->

---

## 4. Testing

- When the testing strategy is set correctly, it saves you in the long run <!-- .element: class="fragment" data-fragment-index="1" -->
- Don't be afraid to utilize fitness functions <!-- .element: class="fragment" data-fragment-index="2" -->

---

## Testing pyramid

![Testing pyramid](/assets/test_pyramid.jpg)

---

## Keep clients in the loop

- Clients want to feel in control <!-- .element: class="fragment" data-fragment-index="1" -->
- Being honest builds confidence <!-- .element: class="fragment" data-fragment-index="2" -->

---

## 5. Release

- Good CI/CD helps you sleep at night <!-- .element: class="fragment" data-fragment-index="1" -->
- Utilize diagrams to communicate with \*Ops people <!-- .element: class="fragment" data-fragment-index="2" -->
- Have release environments rock solid <!-- .element: class="fragment" data-fragment-index="3" -->
- Align with your clients <!-- .element: class="fragment" data-fragment-index="4" -->

---

## 6. Maintenance

- If you did everything correctly until now, maintenance will be a piece of cake <!-- .element: class="fragment" data-fragment-index="1" -->
- Keep an eye on the architectural characteristics <!-- .element: class="fragment" data-fragment-index="2" -->
- Develop methods for monitoring <!-- .element: class="fragment" data-fragment-index="3" -->
- Good observability is golden <!-- .element: class="fragment" data-fragment-index="4" -->

---

## Wrapping it All Up

> "It's dark and hell is hot", **DMX**

- Communication is the key
- Architecture is hard and keeps evolving
- Documentation is your friend
- Keep it lean and clean

---

## Thank you!

---

## Questions?

---

## More Info and References

![QR Code](/assets/qr.png)

[https://github.com/shellmonk/idea-to-product-talk](https://github.com/shellmonk/idea-to-product-talk)

---

<section data-background-color="white">
    <img src="/assets/upitnik-qr.png" width=500 >
</section>
