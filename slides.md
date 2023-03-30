---
# try also 'default' to start simple
theme: light-icons
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'x'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: true
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# use UnoCSS (experimental)
css: unocss
---

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

# Building a function


```ts {all|1|3||4|6|7-9|12-14|all|14-15}
type Pizza = { id: ID; name: string; toppings: Topping[]; isVegetarian: boolean}

function filterPizzas(pizzas: Pizza[], keepPizzaType?: "veggie" | "vegan", flipResults?: boolean) {
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

function filterPizzas(pizzas: Pizza[], keepPizzaType?: "veggie" | "vegan", flipResults?: boolean) {
  let results = []

  pizzas.forEach((piz) => {
    if (keepPizzaType === "veggie" && piz.isVegetarian) {
      results.push(piz)
    }
  })

  if (flipResults) {
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
function filterPizzas(pizzas: Pizza[], keepPizzaType?: "veggie" | "vegan", keepVegan?: boolean)

// Not possible...
const flipped = filterPizzas(pizzas, true)

// Meh...
const flipped = filterPizzas(pizzas, undefined, true)
```

---

# Extending the function


```ts {all|3|12-14|19-21}
type Pizza = { id: ID; name: string; toppings: Topping[]; isVegetarian: boolean}

function filterPizzas(pizzas: Pizza[], keepPizzaType?: "veggie" | "vegan", flipResults?: boolean) {
  let results = []

  pizzas.forEach((piz) => {
    if (keepPizzaType === "veggie" && piz.isVegetarian) {
      results.push(piz)
    }
  })

  if (flipResults) {
    return results.reverse()
  }

  return results
}

// Usage
const allVeggie = filterPizzas(pizzas, "veggie")
const allVeggieFlipped = filterPizzas(pizzas, "veggie", true)
```
---