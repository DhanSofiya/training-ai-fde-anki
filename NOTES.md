# NOTES

## How I navigated the codebase

## The restaurant metaphor

Imagine the app is a restaurant.

---

**1. Where does a request enter? → routes.py**

routes.py is the **front-of-house / waiter**.

When a customer (your browser) walks in and says "I'd like to see the menu" or "I want to order the pasta," the waiter is the first person they talk to. The waiter listens to the order, writes it down, and knows which kitchen station to send it to.

That is routes.py. Every request comes in through it.

---

**2. Where does it hit the database? → db.py**

db.py is the **pantry / stockroom**.

The waiter does not cook. When an order comes in, someone has to go to the stockroom, pull out the ingredients, and bring them back. db.py is that stockroom — it is where all the actual data (decks, cards, review history) lives and gets retrieved or stored.

---

**3. Where does the response leave? → also routes.py**

routes.py is also the **waiter who brings the food back out**.

The same waiter who took your order is the one who carries the plate from the kitchen back to your table. The stockroom never talks to the customer directly — it only talks to the kitchen or the waiter. The response always goes out through routes.py.

---

**The extra step — services.py**

services.py is the **chef**.

Sometimes the waiter cannot just grab something from the stockroom and hand it over. Someone has to actually cook — combine ingredients, apply logic, calculate something. For example: "which cards are due today?" needs date calculations before anything is fetched. That is the chef's job.

So the full flow for a complex order is:

```
Customer (browser)
    → Waiter takes the order        (routes.py)
    → Waiter tells the Chef         (services.py)
    → Chef fetches from the Pantry  (db.py)
    → Chef prepares the result      (services.py)
    → Waiter brings it to the table (routes.py)
    → Customer receives it          (browser)
```

For simple orders — like "just list all the decks" — the waiter skips the chef and goes straight to the pantry and back. Kinda like when a customer asks the waiter "Today ada tak (insert makanan here)"

---

## What the by-hand bug taught me

====================================================== short test summary info =====================================================
FAILED tests/test_ai.py::test_generate_inserts_and_returns_drafted_cards - assert 404 == 200
FAILED tests/test_routes.py::test_generate_without_key_returns_503 - assert 404 == 503


The app did not recognise the URL.

Ought to check what the 404 number means "not found" where URLs are registered, which is routes.py.

Understand the error type, then check where to fix/said error would come from.

---

## Where Claude Code helped or got in the way

Claude Code identified the correct file (routes.py) and fixed the trailing 's' in the endpoint path correctly on the first try — no guidance needed. I reviewed the diff before accepting and it was clean. That said, I prefer Claude Code to ask for clarification before acting rather than making changes immediately.

---

## What I put in CLAUDE.md and whether it changed agent behavior

I added exact install, run, and test commands, a module-by-module architecture summary (routes.py → services.py → db.py → ai.py → models.py), and three conventions: test database isolation via ANKI_DB_PATH, the optional AI endpoint returning 503 without a key, and keeping the test suite mock-only. Claude Code did reference the architecture correctly after CLAUDE.md was in place. That said, getting agent behaviour to fully meet my standards is a trial-and-error process — I intend to keep refining it over time.
