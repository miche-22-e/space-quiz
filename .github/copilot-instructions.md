# Copilot Instructions for Space Quiz

## Architecture & Project Structure

- **Zero-Dependency Single-File App**: The entire application lives inside `index.html`. Do not introduce node build tools, external CDNs, bundlers, or split CSS/JS files unless requested.
- **Components in `index.html`**:
  - `header.quiz-header`: Mission Control styled HUD branding with pulsing telemetry beacon (`.mc-beacon`).
  - `main#quizCard`: Unified card wrapper containing `#quizView` (active quiz state) and `#resultsView` (completion state).
  - `#quizView`: Displays current progress bar (`.progress-track`), question counter, score counter, category label, question text, dynamic options grid (`#optionsGrid`), explanation box (`#explanationBox`), and next action button (`#nextBtn`).
  - `#resultsView`: Displays confetti canvas (`#confettiCanvas`), flight qualification badge (`#celebrationBadge`), tier-based emoji reaction, accuracy breakdown grid, and restart button (`#restartBtn`).
  - `<script>`: Inlined question data array, state variables (`currentQuestionIndex`, `score`, `answered`), and DOM manipulation functions.

## Development & Verification

- **Running the App**: Open `index.html` directly in any web browser (`file:///.../index.html`) or via the integrated browser canvas. No HTTP server is needed.
- **Testing & Verification**:
  - Use browser canvas actions (`open_canvas`, `invoke_canvas_action` with `read_page`, `click_element`, or `evaluate_javascript`) to test DOM updates, keyboard navigation, score increments, and results rendering.
  - Verify both light and dark mode appearances (`prefers-color-scheme`).

## Key Conventions

- **Theme Support**: Use CSS custom properties defined in `:root` and overridden in `@media (prefers-color-scheme: light)` for all colors, borders, and shadows.
- **Typography**: Base sans-serif stack (`system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`) with 14–16px body size; monospace font family (`--font-mono`) reserved for mission-control telemetry elements, badges, and HUD indicators.
- **State Flow & Answer Handling**:
  - `handleAnswer(selectedIndex, selectedBtn)` disables all option buttons upon selection to freeze choices.
  - Correct selections gain `.correct` (green glow, pop animation); wrong selections gain `.wrong` (red shake animation) while highlighting the correct answer button with `.correct`.
  - Next/Results button receives focus immediately after an answer is submitted to maintain keyboard ergonomics.
- **Results & Celebrations**:
  - Scores $\ge 5$ qualify as passing: triggers the celebratory badge (`.celebration-badge`) and launches the canvas-based particle confetti animation via `launchCelebration()`.
  - Scores $< 5$ show standard cadet/rookie review without confetti.
  - Cancel any active `requestAnimationFrame` loop in `restartQuiz()` before reinitializing.
- **Accessibility**:
  - Keep `#optionsGrid` role set to `radiogroup` and each `.option-btn` role to `radio`.
  - Maintain decorative elements (beacon, confetti canvas) flagged with `aria-hidden="true"`.
