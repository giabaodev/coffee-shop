# Coffee App - Design System

## Typography
*   **Headline Font:** Plus Jakarta Sans
*   **Body Font:** Manrope
*   **Label Font:** Manrope

## Color Palette Reference

**Primary**
*   Primary: `#a68cff`
*   On Primary: `#25006b`
*   Primary Container: `#7c4dff`
*   On Primary Container: `#ffffff`

**Secondary**
*   Secondary: `#b0c6ff`
*   On Secondary: `#003b8e`
*   Secondary Container: `#002359`
*   On Secondary Container: `#79a2ff`

**Tertiary**
*   Tertiary: `#faabff`
*   On Tertiary: `#710083`
*   Tertiary Container: `#f595ff`
*   On Tertiary Container: `#610071`

**Background & Surface**
*   Background: `#0c0e12`
*   On Background: `#e1e6ef`
*   Surface: `#0c0e12`
*   On Surface: `#e1e6ef`
*   Surface Variant: `#21262d`
*   On Surface Variant: `#a7abb4`
*   Surface Container Highest: `#21262d`
*   Surface Container High: `#1c2026`
*   Surface Container: `#161a1f`
*   Surface Container Low: `#101418`

**Error**
*   Error: `#fd6f85`
*   On Error: `#490013`
*   Error Container: `#8a1632`
*   On Error Container: `#ff97a3`


---

# Design System Strategy: Nocturnal Elegance

## 1. Overview & Creative North Star: "The Midnight Alchemist"

This design system is built to transform the routine act of ordering coffee into a ritualistic, premium experience. Our Creative North Star, **"The Midnight Alchemist,"** steers us away from the cold, clinical layouts of standard e-commerce and toward a world of depth, luminescence, and tactile luxury.

To move beyond the "template" look, we leverage **intentional asymmetry** and **overlapping liquid layers**. We reject the rigid 12-column grid in favor of an editorial flow where imagery breaks the container bounds, and glass surfaces float atop deep, cosmic gradients. The goal is a "Liquid Glass" interface that feels as smooth and premium as a perfectly pulled espresso.

---

## 2. Colors & The Surface Philosophy

Our palette is rooted in the transition of light through liquid and glass. We use deep `background` (#0c0e12) and `surface` tones to ground the experience, while `primary` (#a68cff) and `tertiary` (#faabff) accents mimic the ethereal glow of neon or city lights at midnight.

### The "No-Line" Rule
Traditional 1px borders are strictly prohibited for sectioning. We define space through **tonal transitions**.
- **Boundaries:** Use a shift from `surface-container-low` to `surface-container-high` to denote a new functional area.
- **Organic Flow:** Allow the `midnight gradient` (a radial blend of `primary_container` and `secondary_container` at low opacities) to bleed behind content, creating a natural sense of place without hard stops.

### Surface Hierarchy & Nesting
Think of the UI as a physical stack of frosted glass.
- **Foundation:** `surface` (#0c0e12) or `surface_container_lowest` (#000000).
- **Secondary Containers:** Use `surface_container_low` for large content blocks.
- **Interactive Elements:** Use `surface_container_highest` (#21262d) to pull interactive cards toward the user.

### The "Glass & Gradient" Rule
To achieve the signature "Liquid Glass" look, use semi-transparent `surface_variant` fills with a `backdrop-filter: blur(24px)`.
- **Signature Textures:** Apply a subtle linear gradient from `primary` (#a68cff) to `tertiary` (#faabff) at 15% opacity on large glass surfaces to give them a "soul"—a faint chromatic shimmer that reacts to the background.

---

## 3. Typography: Editorial Clarity

We utilize two distinct typefaces to balance modern tech with artisanal craft.

*   **Display & Headlines (Plus Jakarta Sans):** These are our "Voice." With generous letter spacing and bold weights, the `display-lg` (3.5rem) and `headline-lg` (2rem) scales provide a high-contrast, editorial feel that commands attention.
*   **Body & UI (Manrope):** Chosen for its geometric precision and readability. `body-lg` (1rem) handles the narrative, while `label-md` (0.75rem) provides technical clarity for coffee notes and pricing.

**The Hierarchy Rule:** Never center-align long-form text. Use asymmetrical left-alignment with wide margins to create a sense of "whitespace as a luxury."

---

## 4. Elevation & Depth: Tonal Layering

We do not use drop shadows to indicate "elevation." We use **Luminance and Blur**.

*   **The Layering Principle:** A `surface_container_high` card placed on a `surface_dim` background provides all the visual separation required.
*   **Ambient Shadows:** If an element must "float" (like a floating action button or mobile navigation bar), use a 40px blur shadow tinted with `surface_tint` (#a68cff) at 6% opacity. It should look like a glow, not a shadow.
*   **The "Ghost Border" Fallback:** If accessibility requires a stroke, use `outline_variant` (#434850) at **15% opacity**. It should be felt, not seen.
*   **Glassmorphism Specs:** For the "Bubble" elements, use a 1px inner-top-border (highlight) using `on_surface_variant` at 20% to simulate light hitting the edge of a glass pane.

---

## 5. Components

### Buttons & Interaction
- **Primary Action:** A solid `primary` (#a68cff) fill with `on_primary` (#25006b) text. Shape must be `full` (9999px) to match the "liquid bubble" aesthetic.
- **Glass Secondary:** `surface_container_high` with 40% opacity and a `backdrop-blur`.

### Chips (Coffee Tags)
Use the `full` roundedness scale. Selection chips should transition from `surface_container` to a `primary_container` glow when active. No outlines.

### Liquid Cards
Cards should never have a solid background. Use a combination of `surface_container_low` at 70% opacity and a subtle "highlight" gradient in the corner. 
- **Forbidden:** No divider lines between list items in a card. Use `1.5rem` (`md`) vertical spacing to separate items.

### Input Fields
Text inputs use `surface_container_highest` with a `sm` (0.5rem) corner radius. The focus state shouldn't be a border change, but an increase in the backdrop-blur intensity and a subtle `primary_dim` glow.

### Bespoke Component: The "Brew Progress" Bubble
A floating, glassmorphic sphere that uses `tertiary` (#faabff) liquid-fill animations to show the status of an order. It leverages the `xl` (3rem) or `full` roundedness scale.

---

## 6. Do's and Don'ts

### Do
- **Do** use overlapping elements (e.g., a coffee cup PNG floating 20px outside its glass container).
- **Do** utilize the `midnight gradient` to guide the eye toward the primary CTA.
- **Do** keep corner radii consistent—use `xl` (3rem) for main containers and `lg` (2rem) for internal cards to maintain the "bubble" language.

### Don't
- **Don't** use pure white (#FFFFFF) for text unless it's a critical headline. Use `on_surface` (#e1e6ef) to keep the "midnight" mood.
- **Don't** use standard material shadows. They break the liquid-glass illusion.
- **Don't** use 1px solid dividers. If you need separation, use a 24px vertical gap or a subtle change in surface tier.
- **Don't** use sharp corners. Every element should feel like it has been smoothed by water.
