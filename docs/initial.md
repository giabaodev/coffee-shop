You are an expert Next.js developer. Set up the foundation for a Coffee Shop website.

Tech stack: Next.js (App Router), React 19, TypeScript, Tailwind CSS, ShadcnUI, Zod, Shadcn Form.

## Tasks:

### 1. Create a shared `Flex` component at `components/ui/flex.tsx`
A reusable layout primitive to avoid repeating Tailwind flex classes.

```tsx
// components/ui/flex.tsx
import { cn } from "@/lib/utils";
import { HTMLAttributes } from "react";

type FlexProps = HTMLAttributes<HTMLDivElement> & {
  direction?: "row" | "col";
  align?: "start" | "center" | "end" | "stretch" | "between";
  justify?: "start" | "center" | "end" | "between" | "around" | "evenly";
  wrap?: boolean;
  gap?: 0 | 1 | 2 | 3 | 4 | 5 | 6 | 8 | 10 | 12 | 16;
};
```
Map each prop to Tailwind classes and forward the rest as HTML attributes.

### 2. Create `lib/constants.ts`
Define site-wide content:
- SITE_NAME = "Brew & Co."
- NAV_LINKS: array of { label, href } for: Home, Menu, About, Contact
- SOCIAL_LINKS: Instagram, Facebook, Twitter with icons and hrefs

### 3. Create `lib/types.ts`
Define TypeScript types:
- `MenuItem`: id, name, description, price (number), category, image, badge? ("New" | "Popular" | "Seasonal")
- `Category`: id, label, icon?
- `ContactFormData`: name, email, phone?, message, subject?

### 4. Create `lib/data.ts`
Mock data for:
- 12 menu items across categories: "Espresso", "Cold Brew", "Pastries", "Specialty"
- 4 categories
- 3 testimonials: { id, name, avatar, rating, text, date }

### 5. Set up Tailwind theme in `tailwind.config.ts`
Extend with coffee-inspired colors:
- primary: warm brown tones (#6B3F2A, #8B5A2B)
- accent: cream/beige (#F5E6D3, #E8D5B7)
- background: off-white (#FAFAF8)
- foreground: dark espresso (#1C1008)

PROMPT 2 — Layout: Navbar & Footer
Create the site-wide layout components for the Coffee Shop website.

## Navbar — `components/layout/navbar.tsx`
- Sticky top navbar with backdrop blur
- Left: Logo with a coffee cup SVG icon + "Brew & Co." text
- Center (desktop): Navigation links with active state using `usePathname`
- Right: "Order Now" Button (ShadcnUI Button, variant="default"), mobile hamburger menu
- Mobile: Sheet (ShadcnUI) sliding drawer with all nav links
- Animate active link with an underline using Tailwind `after:` pseudo-classes
- Use the shared `Flex` component for layout

## Footer — `components/layout/footer.tsx`
- Dark background (bg-foreground text-background)
- 4-column grid (responsive: 2-col tablet, 1-col mobile):
  1. Brand column: Logo, tagline, social icons
  2. Quick Links: Nav links list
  3. Opening Hours: Mon-Fri 7am-8pm, Sat-Sun 8am-9pm
  4. Contact: Address, phone, email with icons (lucide-react)
- Bottom bar: copyright + "Made with ☕" text
- Use `Flex` component throughout

## Root Layout — `app/layout.tsx`
- Import and wrap children with `<Navbar />` and `<Footer />`
- Add Inter + Playfair Display Google Fonts (Inter for body, Playfair Display for headings)
- Apply base background and foreground colors

PROMPT 3 — Homepage (Hero + Features + Menu Preview)
Build the Homepage at `app/page.tsx` with these sections. Use the shared `Flex` component wherever flex layout is needed.

## Section 1: Hero
- Full-viewport height section
- Background: high-quality coffee shop image (use Unsplash URL: https://images.unsplash.com/photo-1501339847302-ac426a4a7cbb?w=1920)
- Dark overlay (bg-black/50)
- Center content:
  - Badge: ShadcnUI Badge, "Est. 2018"
  - H1: "Where Every Cup Tells a Story" — large serif font (Playfair Display), white
  - Subtitle paragraph: warm, inviting tagline
  - Two CTAs: "Explore Menu" (primary Button) and "Our Story" (outline Button, white border)
- Scroll indicator arrow at bottom center (animated bounce)

## Section 2: Feature Cards (3 columns)
- Section title: "Why Choose Us"
- 3 ShadcnUI Cards with icons (lucide-react), title, description:
  1. ☕ "Premium Beans" — sourced from top farms
  2. 🏠 "Cozy Atmosphere" — perfect for work or relaxation
  3. 👨‍🍳 "Expert Baristas" — trained to perfection
- Cards have hover effect: slight shadow lift (Tailwind hover:shadow-xl transition)

## Section 3: Menu Preview
- Section title + subtitle
- Category filter tabs using ShadcnUI `Tabs` component
- Grid of 6 `MenuCard` components (see Prompt 4)
- "View Full Menu" Button linking to /menu

## Section 4: Testimonials
- Section with warm background (accent color)
- 3 testimonial cards in a grid with:
  - Star rating (filled stars using lucide Star icon)
  - Quote text
  - Avatar (use placeholder), name, date
- Use ShadcnUI Card

## Section 5: CTA Banner
- Full-width section with primary color background
- "Ready for Your Next Coffee Break?" heading
- Subtext + "Contact Us" Button (variant="outline")

PROMPT 4 — MenuCard Component + Full Menu Page
## MenuCard Component — `components/menu/menu-card.tsx`
Create a polished card using ShadcnUI Card:
- Image (aspect-square, object-cover, rounded-t)
- Optional Badge overlay (top-right): "New" (blue), "Popular" (orange), "Seasonal" (green)
- CardContent:
  - Flex row: item name (font-semibold) + price (text-primary, font-bold)
  - Description (text-muted-foreground, text-sm, line-clamp-2)
  - "Add to Order" Button (full-width, size="sm") with a Plus icon
- Hover: image scales slightly (hover:scale-105 on image wrapper with overflow-hidden)
- Use `Flex` component for internal layouts

## Full Menu Page — `app/menu/page.tsx`
- Page header with background image + title "Our Menu"
- Sticky filter bar:
  - ShadcnUI Tabs for categories (All, Espresso, Cold Brew, Pastries, Specialty)
  - Search input (ShadcnUI Input) with Search icon
- Responsive grid: 1 col mobile, 2 col tablet, 3 col desktop, 4 col wide
- Render filtered `MenuCard` components based on active tab + search query
- Use `useMemo` for filtering logic
- Empty state: illustration + "No items found" message
- All data from `lib/data.ts`

PROMPT 5 — About Page
Build `app/about/page.tsx` — a storytelling page about the coffee shop.

## Section 1: Hero
- Split layout (2 cols on desktop, stack on mobile):
  - Left: Heading "Our Story", story paragraph, founding year badge, "Meet the Team" anchor link
  - Right: Coffee shop interior image (Unsplash)
- Use `Flex` with direction="col" for mobile stacking

## Section 2: Stats Bar
- Full-width, primary background color
- 4 stats in a row using `Flex` justify="between":
  - 🫘 "500+" Varieties Sourced
  - ⭐ "4.9" Average Rating
  - 👥 "50K+" Happy Customers
  - 📅 "6+ Years" in Business

## Section 3: Our Process
- Title: "From Bean to Cup"
- 4-step process with numbered circles, icon, title, description:
  1. Sourcing → 2. Roasting → 3. Brewing → 4. Serving
- Connect steps with a dashed horizontal line on desktop (hidden on mobile)

## Section 4: Team Grid
- id="meet-the-team"
- 4 team cards: Avatar (large, rounded-full), Name, Role, short bio
- Use ShadcnUI Card + Avatar components

## Section 5: Values
- 3 value cards with icon + title + paragraph
- Warm cream background

PROMPT 6 — Contact Page with Form Validation
Build `app/contact/page.tsx` with full form validation.

## Layout: Split 2-column on desktop
### Left column — Contact Info:
- "Get in Touch" heading
- 3 info cards (ShadcnUI Card) using `Flex`:
  - 📍 Address: 123 Brew Street, Coffee Town
  - 📞 Phone: +1 (555) 123-4567
  - 📧 Email: hello@brewandco.com
- Opening hours table (ShadcnUI Table or simple grid)
- Embedded Google Maps iframe (placeholder with bg-muted + map icon)

### Right column — Contact Form:
Use `react-hook-form` + Zod + Shadcn Form components.

Zod schema (define in `lib/schemas/contact.ts`):
```ts
const contactSchema = z.object({
  name: z.string().min(2, "Name must be at least 2 characters"),
  email: z.string().email("Please enter a valid email"),
  phone: z.string().optional(),
  subject: z.enum(["General", "Catering", "Feedback", "Partnership"]),
  message: z.string().min(10, "Message must be at least 10 characters").max(500),
});
```

Form fields using Shadcn `Form`, `FormField`, `FormItem`, `FormLabel`, `FormControl`, `FormMessage`:
- Name (Input)
- Email (Input)
- Phone (Input, optional)
- Subject (Select with the enum options)
- Message (Textarea, show char count)
- Submit Button: loading state with spinner, disabled during submission

On submit: simulate async API call (setTimeout 1500ms), show ShadcnUI toast on success/error.
Use `Flex` for form layout.

PROMPT 7 — Polish, SEO & Responsive QA
Final polish pass for the Coffee Shop website.

## 1. Metadata — `app/layout.tsx` and each page
Add Next.js Metadata API to each page:
- layout.tsx: default title template "Brew & Co. | %s", description, openGraph
- page.tsx: title "Home", description
- menu/page.tsx: title "Menu"
- about/page.tsx: title "About Us"
- contact/page.tsx: title "Contact"

## 2. Loading UI
Create `loading.tsx` for menu and contact routes:
- Skeleton loaders using ShadcnUI Skeleton component
- MenuCard skeleton: image placeholder + text lines

## 3. Responsive QA Checklist — fix any issues:
- Navbar hamburger on mobile (320px–768px)
- Hero text readable on small screens
- Menu grid collapses to 1 column on mobile
- Contact form full-width on mobile
- Footer 1-column on mobile
- All touch targets ≥ 44px

## 4. Animations
Add subtle entry animations using Tailwind (no extra library):
- Hero content: `animate-fade-in` (define custom keyframe in tailwind.config.ts)
- Cards: stagger with `animation-delay` utility classes
- Feature section cards: `animate-slide-up`

## 5. `components/ui/flex.tsx` final audit
Confirm `Flex` is used in: Navbar, Footer, Hero, Feature Cards, Menu Page header, Contact page columns. Replace any remaining `<div className="flex ...">` patterns with `<Flex ...>`.

## 6. Error boundary
Create `app/error.tsx` and `app/not-found.tsx` with branded designs (coffee-themed illustration, back home button).