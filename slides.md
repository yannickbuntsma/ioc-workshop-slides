---
theme: light-icons
background: https://source.unsplash.com/collection/94734566/1920x1080
class: x
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
css: unocss
---

<style>
h1 {
  @apply text-transparent bg-clip-text bg-gradient-to-r from-red-500 to-blue-700;
}
</style>

---
theme: none
layout: intro
image: 'https://images.unsplash.com/photo-1453974336165-b5c58464f1ed?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3546&q=80'
---

  <div class="mb-4 absolute bottom-4 left-12">
    <div class="text-8xl text-white text-opacity-60" style="font-weight:600;" >
      Inversion of control
    </div> 
      <span class="text-5xl text-primary-lighter text-opacity-80" style="font-weight:500;" >
      Provide control with constraints
    </span>
  </div>


---

# Building a function


```ts {all|1|3||4|6|7-9|12|all|15-16}
type Pizza = { id: ID; name: string; toppings: Topping[]; isVegetarian: boolean}

function filterPizzas(pizzas: Pizza[], keepPizzaType?: "veggie" | "tomatoSauce", flipOrder?: boolean) {
  let results = []

  pizzas.forEach((piz) => {
    if (keepPizzaType === "veggie" && piz.isVegetarian) {
      results.push(piz)
    }
  })

  return results
}

// Usage
const allVeggie = filterPizzas(pizzas, "veggie")
```

---

# Three months later...

<style>
h1{
  text-align: center;
  margin-top: 15vh;
  }
  </style>

---


```tsx
// WTF does `true` mean?

filterPizza(pizzas, true)
```

<style>
code{
  font-size: 2rem;
  }
  </style>

<!-- Source code, answer, go about your day. Build features. -->
---


# Extending the function


```ts {all|3|12-14|19-21}
type Pizza = { id: ID; name: string; toppings: Topping[]; isVegetarian: boolean}

function filterPizzas(pizzas: Pizza[], keepPizzaType?: "veggie" | "tomatoSauce", flipOrder?: boolean) {
  let results = []

  pizzas.forEach((piz) => {
    if (keepPizzaType === "veggie" && piz.isVegetarian) {
      results.push(piz)
    }
  })

  if (flipOrder) {
    return results.reverse()
  }

  return results
}

// Usage
const allVeggie = filterPizzas(pizzas, "veggie")
const allVeggieFlipped = filterPizzas(pizzas, "veggie", true)
```
---

# Problem

### What if I want to keep all pizzas and flip the list?


```ts {all}
type Pizza = { id: ID; name: string; toppings: Topping[]; isVegetarian: boolean}

// For reference
function filterPizzas(pizzas: Pizza[], keepPizzaType?: "veggie" | "tomatoSauce", flipOrder?: boolean)

// Not possible... ❌
const flipped = filterPizzas(pizzas, true)

// Meh... 🤢
const flipped = filterPizzas(pizzas, undefined, true)
```

---

# Extending the function - with options


```ts {all|2-5|21-24}
type Pizza = { id: ID; name: string; toppings: Topping[]; isVegetarian: boolean}
type Options = { keepPizzaType?: "veggie" | "tomatoSauce"; flipOrder?: boolean }

function filterPizzas(pizzas: Pizza[], options?: Options) {
  const { keepPizzaType, flipOrder } = options || {}
  let results = []

  pizzas.forEach((piz) => {
    if (keepPizzaType === "veggie" && piz.isVegetarian) {
      results.push(piz)
    }
  })

  if (flipOrder) {
    return results.reverse()
  }

  return results
}

// Super clear
const allVeggie = filterPizzas(pizzas, { keepPizzaType: "veggie" })
// Hooray! 🎉
const flipped = filterPizzas(pizzas, { flipOrder: true })
```

---

# Unlimited extensibility


```ts
const allVeggie = filterPizzas(pizzas, { keepPizzaType: "veggie" })

const flipped = filterPizzas(pizzas, { size: "large" })

const flipped = filterPizzas(pizzas, { crustType: "cheesy" })

const flipped = filterPizzas(pizzas, { priceAbove: 20 })

// Money to spend 💰
const expensiveTastes = filterPizzas(
  pizzas, 
  { 
    priceAbove: 20,
    size: "small",
    keepPizzaType: "vegan",
  }
)
```

---

# Another problem...

### Every time we want to filter on some other property, we have to add it to the options and implement it in the function.

```ts
filterPizzas(
  pizzas, 
  { 
    priceAbove: 20,
    priceBelow: 45,
    size: "small",
    keepPizzaType: "vegan",
    crustType: "cheesy",
    // ...
    // ...
    // ...
    // ...
  }
)
```

---

# Inversion of control

```ts
[0, 1, 2, 3, 4].filter((num) => num > 2)
// [3, 4]
```

### Let the developer using the function take control of e.g. the filtering. 

```ts
const pizzas = [
  { name: "calzone", size: "large" },
  { name: "big american", size: "large" },
  { name: "nutella", size: "small" },
]

filterPizzas(pizzas, (piz) => piz.size === "small")
// [{ name: "nutella", size: "small" }]
```

---

# Inversion of control

```ts {1-9|8-100|all}
const pizzas = [
  { name: "calzone", size: "small", type: "savory" },
  { name: "big american", size: "large", type: "savory" },
  { name: "gelato", size: "small", type: "sweet" },
  { name: "nutella", size: "large", type: "sweet" },
]

filterPizzas(pizzas, (piz) => piz.size === "large" && piz.type === "sweet")
// [{ name: "nutella", size: "large", type: "sweet" },]

function filterPizzas(pizzas, filterFn) {
  let results = []

  pizzas.forEach((piz) => {
    if (filterFn(piz)) {
      results.push(piz)
    }
  })

  return results
}
```

---

# Inversion of control - you do you

```ts
// Specific taste 🤔
filterPizzas(pizzas, (p) => {
  return p.sauce === "tomato" && p.toppings.includes("onions") && p.crustType !== "cheesy"
})

// End of the month 💸
filterPizzas(pizzas, (p) => {
  return p.price <= 5
})

// Treat yourself 🤤
filterPizzas(pizzas, (p) => {
  const isItSunday = new Date().getDay() === 0

  return isItSunday && workoutsThisWeek > 2 && p.calories < 600
})
```

---

# Aan de slag!
