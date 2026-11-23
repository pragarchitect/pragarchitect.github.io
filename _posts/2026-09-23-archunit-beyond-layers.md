---
layout: post
title: "ArchUnit beyond layers: rules I actually use"
categories: [architecture, java, ArchUnit]
---

In my [Java Magazine article]({{ site.baseurl }}/architecture-as-code-java-magazine/) I showed how you can use [ArchUnit](https://www.archunit.org/) to check a layered architecture. It is the classic example: the controller may call the service, the service may call the repository, and nobody goes the other way.

That example is good for explaining the idea. But after the article, a few people asked me: "Okay, but what else? What do you check in real projects?"

Good question. So here are the rules I actually use, or have used, in Java projects. With the code, and with the reason why. Because a rule without a reason will be deleted by the first developer who is annoyed by it. And they will be right.

{% include pullquote.html quote="An architecture rule without a reason will be deleted by the first developer who is annoyed by it." %}

## Quick setup

If you didn't use ArchUnit before: add the JUnit 5 dependency and write a test class.

```xml
<dependency>
    <groupId>com.tngtech.archunit</groupId>
    <artifactId>archunit-junit5</artifactId>
    <version>1.3.0</version>
    <scope>test</scope>
</dependency>
```

```java
@AnalyzeClasses(packages = "com.example.shop",
                importOptions = ImportOption.DoNotIncludeTests.class)
class ArchitectureTest {

    @ArchTest
    static final ArchRule some_rule = ...;
}
```

`@AnalyzeClasses` imports the classes once, and every `@ArchTest` field is a rule. The `DoNotIncludeTests` option is important, otherwise your test code will break your rules all the time.

Now the rules.

## 1. The domain doesn't know about frameworks

In a hexagonal or clean architecture, the domain should be plain Java. No Spring, no JPA, no Jackson. That keeps it easy to test and easy to understand, and it means a framework upgrade doesn't touch your business logic.

```java
@ArchTest
static final ArchRule domain_is_framework_free =
    noClasses().that().resideInAPackage("..domain..")
        .should().dependOnClassesThat()
        .resideInAnyPackage(
            "org.springframework..",
            "jakarta.persistence..",
            "com.fasterxml.jackson..")
        .because("the domain must stay plain Java, see ADR-0002");
```

Notice the `because`. When the test fails, the developer sees this message. And it points to the ADR where the decision is explained. That link between ADR and test is, for me, the most valuable part of this whole approach.

## 2. Ports and adapters, in the right direction

The next step in a hexagonal setup: adapters may use the application layer, but the application layer may not use adapters. It only talks to ports (interfaces).

```java
@ArchTest
static final ArchRule hexagonal =
    onionArchitecture()
        .domainModels("..domain.model..")
        .domainServices("..domain.service..")
        .applicationServices("..application..")
        .adapter("web", "..adapter.web..")
        .adapter("persistence", "..adapter.persistence..")
        .adapter("messaging", "..adapter.messaging..");
```

ArchUnit has `onionArchitecture()` built in. It also checks that adapters don't call each other. That one catches more than you expect: a REST controller that directly uses a JPA repository "just for this one query".

## 3. No cycles between modules

Cyclic dependencies between packages are the start of a big ball of mud. Module A uses B, B uses C, C uses A. Now you can't change anything without touching everything.

```java
@ArchTest
static final ArchRule no_cycles =
    slices().matching("com.example.shop.(*)..")
        .should().beFreeOfCycles();
```

This takes every first-level package under `shop` as a slice and checks that there are no cycles between them. On an existing codebase this rule will probably fail the first time. More about that below.

## 4. Modules only talk through their API

In a modular monolith, each module has a public API and internal stuff. Other modules should only use the API. Otherwise you get a monolith with nice package names, but still a monolith.

My convention: every module has an `api` package. Everything else is internal.

```java
@ArchTest
static final ArchRule modules_use_only_public_api =
    noClasses().that().resideInAPackage("com.example.shop.orders..")
        .should().dependOnClassesThat(
            resideInAPackage("com.example.shop.billing..")
                .and(resideOutsideOfPackage("com.example.shop.billing.api..")))
        .because("modules communicate only via their api package");
```

The `resideInAPackage` and `resideOutsideOfPackage` here are static imports from `JavaClass.Predicates`. Combining them with `and` says exactly what we want: anything in billing, except the api package.

This works, but you would need one rule per pair of modules. A more generic way is to write a custom condition. Or, if you use Spring, have a look at Spring Modulith, which does this kind of check for you and uses ArchUnit underneath.

## 5. Naming conventions that carry meaning

I am not a big fan of naming rules for the sake of naming rules. But some names carry architectural meaning. If a class is called `...Controller`, I expect it in the web adapter. If it is called `...Repository`, I expect it in persistence.

```java
@ArchTest
static final ArchRule controllers_in_web_adapter =
    classes().that().haveSimpleNameEndingWith("Controller")
        .should().resideInAPackage("..adapter.web..");

@ArchTest
static final ArchRule entities_stay_in_persistence =
    classes().that().areAnnotatedWith(Entity.class)
        .should().resideInAPackage("..adapter.persistence..")
        .because("JPA entities must not leak into the domain, see ADR-0002");
```

The second one is my favourite. JPA entities that end up in the domain or, even worse, in the REST response, are a very common problem. This rule stops it.

## 6. No field injection

Small one, but it saves discussions in code review. Constructor injection makes dependencies visible and classes easier to test.

```java
@ArchTest
static final ArchRule no_field_injection =
    noFields().should().beAnnotatedWith(Autowired.class)
        .because("use constructor injection");
```

ArchUnit also has a ready-made `GeneralCodingRules.NO_CLASSES_SHOULD_USE_FIELD_INJECTION`. There are more of those general rules, like no `System.out`, no generic exceptions, no `java.util.logging`. Worth a look.

## 7. Time is a dependency

This one comes from experience. Code that calls `LocalDateTime.now()` directly is hard to test. And bugs around time (end of month, daylight saving, end of year) are very real.

```java
@ArchTest
static final ArchRule use_clock =
    noClasses().that().resideInAPackage("..domain..")
        .should().callMethod(LocalDateTime.class, "now")
        .orShould().callMethod(LocalDate.class, "now")
        .because("inject a java.time.Clock so time can be controlled in tests");
```

## Using it on an existing codebase: freezing

Here is the part that makes ArchUnit usable in the real world, not only on greenfield projects.

When you add a rule like "no cycles" to a ten year old codebase, it will fail with three hundred violations. You can't fix them all now. But you also don't want to skip the rule, because then new violations come in every week.

ArchUnit has a solution: `FreezingArchRule`.

```java
@ArchTest
static final ArchRule no_cycles_frozen =
    FreezingArchRule.freeze(
        slices().matching("com.example.shop.(*)..")
            .should().beFreeOfCycles());
```

The first time it runs, it stores all existing violations in a file. After that, the test only fails on *new* violations. When someone fixes an old one, it is removed from the file. So the number of violations can only go down.

{% include pullquote.html quote="Freezing turns 'we will never fix this' into 'it will not get worse, and slowly better'." %}

One practical note: by default ArchUnit will not create the store file by itself. Add `freeze.store.default.allowStoreCreation=true` to an `archunit.properties` file in your test resources, run the test once, and then you can set it back to false.

Commit the violation store to the repository, so everyone works with the same baseline. And look at it now and then in a retrospective. A file that gets smaller every month is quite motivating.

## Some advice from practice

**Start with two or three rules.** Not twenty. Pick the rules that protect the decisions you care most about. Add more when you see a real problem coming back.

**Always add `because`.** And link to the ADR. When a rule fails, the developer should understand why, without asking you.

**Discuss rules with the team.** A rule that the architect added alone is a rule the team will try to get around. A rule the team agreed on is a rule the team will defend. Put new rules in a pull request and let people react.

**Be okay with deleting rules.** If a rule fails all the time and everybody adds exceptions, maybe the rule is wrong. Or the decision behind it changed. Then update the ADR, and remove the rule. That is not failure, that is architecture that evolves.

**Run it in the pipeline.** ArchUnit tests are normal unit tests, so they run with your build. That is the whole point: fast feedback, every time, without anyone needing to remember.

## Together with C4 and ADRs

I always come back to the same three things. C4 shows how the system is built. ADRs explain why. ArchUnit makes sure the code keeps following those decisions. Each one alone is useful. Together, they form a loop: decide, document, verify.

Most architecture doesn't fail because of bad decisions. It fails because good decisions slowly get forgotten. A few well chosen ArchUnit rules make sure they aren't.
