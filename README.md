# 🧩 Minesweeper AI

An AI that plays **Minesweeper** using logical inference. Instead of brute-force model checking, this agent represents knowledge as sentences of the form `{cells} = count` (meaning *exactly* `count` mines among those `cells`) and derives safe moves and mines through logical updates and subset inference.

---
🎮 Play it live: https://peramide.github.io/minesweeper
NB: It may take a few minutes to load! Please exercise patience.
---

## 🧠 Overview<img width="712" height="534" alt="minesweeper_game image" src="https://github.com/user-attachments/assets/02463661-596d-4be2-9326-3367f3548d86" />


Minesweeper is a grid-based puzzle where revealed cells show the number of adjacent mines. This project implements a knowledge-based agent that:

* Tracks known safe cells and known mines.
* Builds logical sentences from revealed cells: `{neighboring unknown cells} = count`.
* Infers new safe cells or mines by:

  * If a sentence's count is 0 → all cells in it are safe.
  * If a sentence's count equals the number of cells → all are mines.
  * Subset inference: if `S1 ⊆ S2`, infer `S2 - S1 = count2 - count1`.
* Chooses safe moves if available; otherwise picks a random valid move.

This representation enables efficient, logical reasoning without enumerating every model.

---

## 📁 Project Structure

* `runner.py` — Provided GUI and game loop. Run this to play or observe AI moves.
* `minesweeper.py` — Contains:

  * `Minesweeper` — game board, mines placement, neighbor counts (fully implemented).
  * `Sentence` — logical sentences (`cells`, `count`) and helper methods (you implement known mines/safes and marking).
  * `MinesweeperAI` — maintains `moves_made`, `mines`, `safes`, and `knowledge` (you implement add_knowledge, make_safe_move, make_random_move).

---

## ✅ Implemented & Expected Behavior

* The AI updates knowledge on every revealed safe cell (the count).
* When new information is learned, all sentences are updated and rechecked for new inferences until no more can be derived.
* The AI will mark and avoid known mines and will prefer safe moves over random guessing.
* The AI may still need to guess when insufficient logical information exists — this is expected.

---

## ⚙️ How to Run

1. Clone the repo:

   ```bash
   git clone https://github.com/yourusername/minesweeper-ai.git
   cd minesweeper-ai
   ```

2. Install requirements (pygame):

   ```bash
   pip3 install -r requirements.txt
   ```

3. Run:

   ```bash
   python runner.py
   ```

4. Use the GUI to play, or press **AI Move** to let the AI act. The console will indicate whether the AI made a logical safe move or a random guess.

---

## 🧩 Key Files & Methods to Check

* `Sentence`:

  * `known_mines()` → return set of cells known to be mines (when `count == len(cells)`).
  * `known_safes()` → return set of cells known to be safe (when `count == 0`).
  * `mark_mine(cell)` / `mark_safe(cell)` → update sentence when a cell becomes known.

* `MinesweeperAI`:

  * `add_knowledge(cell, count)` → add sentence for neighbors, mark cell safe, update knowledge base, infer new safes/mines, add subset-inferred sentences.
  * `make_safe_move()` → return an unplayed safe move or `None`.
  * `make_random_move()` → return a random move that is not already played and not known to be a mine, or `None`.

---

## 🧠 Concepts Demonstrated

* Knowledge representation using sets and counts
* Logical inference via sentence simplification and subset reasoning
* Practical AI for uncertain environments
* Trade-off between perfect deduction and probabilistic guessing

---

## 🔍 Notes & Tips

* Avoid modifying `Minesweeper` or `runner.py` — they’re intended to remain unchanged.
* When updating sentences during inference, be careful not to mutate sets while iterating over them.
* Thoroughly loop inference until no new knowledge is discovered after every `add_knowledge` call.
* The AI is deterministic in its logical deductions but will be nondeterministic for random moves.

---

## 📚 Acknowledgements

**CS50 Introduction to Artificial Intelligence with Python**
