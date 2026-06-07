# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the App

Open `index.html` directly in a browser — no build step, server, or dependencies required.

## Architecture

Everything lives in a single file: `index.html`.

- **Data**: `transactions` array in the inline `<script>` block (line 247). Each entry has `id`, `description`, `amount`, `type` (`"income"` | `"expense"`), `category`, and `date`. Data is in-memory only — changes are lost on page refresh.
- **Rendering**: `render()` recomputes summary totals from all transactions, applies the active type/category filters, and rewrites `#transactions-body` via `innerHTML`. It is called once on load and after every add or filter change.
- **XSS safety**: User-supplied strings are passed through `escape()` (creates a throwaway `div`, sets `textContent`, returns `innerHTML`) before being inserted into the table.
- **Categories**: Hardcoded in two places — the add-form `<select>` and the filter `<select>`. Adding a new category requires updating both.
- **Styling**: Plain CSS in `<style>`. `.income-amount` is green, `.expense-amount` is red; these classes are reused on both summary cards and table cells.

## Known Data Bug

`"Freelance Work"` (id 4) is typed as `"expense"` with category `"salary"` in the seed data — it should likely be `"income"`.
