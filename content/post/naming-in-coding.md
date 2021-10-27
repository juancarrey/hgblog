+++
title = "How to name variables and code in general"
date = "2021-10-27"
description = "Naming things during coding is hard, for most people, specially for begginers, this is a list of smells and best practices for naming stuff on code."
tags = [
"naming", "coding", "programming"
]
author = "Juan Carrey"
+++

Naming things during programming is not easy, but should not be as hard as it appears to be. 
I believe most of the times names should come straight forward, it may not be the perfect name, but it's good enough to get going.

The goal of a good naming strategy is to make the code readable at first sight, that means, without having to go jumping around code blocks to figure out what is it doing. 
It makes sense to do that when there is a bug, because it would mean that the name of the method or the function is not really doing what is supposed to, so there should be a miss match between what it says it does and what it really does.

If the naming is good, it really helps finding bugs
If the naming is badm it does the oposite, it encourages developers to add bugs because they have to figure out what it is supposed to do in the first place.


## Things to name

There are several things to name during the creation of a product, such as: variables, functions, methods, types, entities, modules, projects among others.

We, developers need to focus on most of them except domain entities (which we should rely on business and domain experts) and project itself (which is usually related the product/company name).

The good thing is that we have to take care of the things that are easy to name and easy to refactor, and the bad news is that we should take care of renaming the hard ones to name and hardest to rename.

Domain entities are a big important part of the product, they are the core of what we do. Their names may change at the begining of a greenfield, but should stay put once matured, which is good for us.


## Naming strategy

There is levels of quality on naming stuff, and like everything in life, you should never start aiming for perfect, but for good enough.

How should we name stuff ? Let's dive in some principles:

 * Single Responsability of variables, methods and functions
 * Declare the intent of what the thing does
 * Declare the contents of what the variable holds
 * Try to use concise names rather than generic ones that super-sets what a variable contain (life-form vs human vs person vs kid vs teenager)
 * Name the intent, encapsulate implementation (what instead of how)
 
Let's see some of these applied in examples:

### Insanely bad naming

Using acronyms and very short names for variables makes it real hard to read and reason about the code, one will have to read the contents beyond variable names and their intent.

Never ever use these type of namig. You will see it mostly on lambdas to make them shorter or onliner. Please, avoid.

```
  const p1 = new Person("bro");
  const p2 = new Person("sis");
  
  const p = [p1, p2];
  const p3 = p.filter(gt21);
  
  fn gt21(a: Person): bool { return a.age > 21 }
```

### Bad naming

Using very generic naming stating what the type is, but not what it is holding or their intent. These are really common, and makes 
total sense for the person writing it, but makes no sense when you are reading it.

Any variable that states what it is but not what it holds or what the intent is, is a code smell, and not a good one.

```
  const person1 = new Person("bro");
  const person2 = new Person("sis");
  const people = [ person1, person2 ];
  const people2 = people.filter(greaterThan21);
  
  fn greaterThan21(person: Person): bool { return person.age > 21 }
```

Avoid function or methods names that explain when the function or method is executed, this is pretty common on javascript for callbacks:

```
  const onClick = { .... };
  
  <Compnent onClick={onClick} />
```

Avoid using the type for names of things:

```
 const [peopleList, setPeopleList] = useState([]);
 cosnt dataList = useQuery(...);
 const mutator = { getPeopleList().then(list => { setPeopleList(list); }); }
```

### Ok

Using simple names that states what the variable is holding

```
  const brother = new Person("bro");
  const sister = new Person("sis");
  const family = [ brother, sister ];
  const brothersAbove21 = family.filter(hasMoreThan21Years);
  
  fn hasMoreThan21Years(person: Person): bool { return person.age > 21 }
```

### Good

Using names that states what the variable is holding and what's their intent

```
  const familyMembers = getFamilyMembers();
  const familyMembersThatCanDrinkAlcohol = familyMembers.filter(canDrinkAlcohol);
  
  fn canDrinkAlcohol(person: Person): bool { return person.age > 21 }
```

On the UI, following the bad naming example, a good enough way to name callbacks would be something like:

```
 const sortContactsByLastNameDescencing = { .... };
 
 <Compnent onClick={sortContactsByLastNameDescencing} />
```

### Perfect

It doesn't exist, but "Good" makes a perfectly readable code and is good enough.


## Code Smells

* Having types on the name 
* Having prefixes or sufixes on the names 
* Having acronyms of any sort
* Non final/const variables (might be ok sometimes)
* Names are too generic when values holded are not
* Not stating what the intent is but just what they are (above18YearsOld vs canDrinkAlcohol)


## Intent

The most important fact of all to name things is to look for the intent of the thing you are naming, what is it really doing ? 

If the bussines rules change or if the implementation details change, would it affect the name ? On the sample above, on some countries the age for drinking alcohol is 18, how would that affect our code ? 

On the last one, just the implementation, but not the name of the variable nor method, if that happens to you, congratulations, you found a good enough name for the thing.


Gladly, Juan 🌍🌳

