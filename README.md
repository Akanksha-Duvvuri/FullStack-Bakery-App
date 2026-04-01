# 🌌 Celestial Bakery

> *Where the cosmos meets the kitchen — handcrafted pastries inspired by the stars.*

A full-stack bakery e-commerce website built with Next.js, Firebase, and Stripe. Space-themed with immersive CSS animations and a complete ordering system.


---

## 🚀 Tech Stack

| Layer | Technology | Why |
|---|---|---|
| Frontend | Next.js 14 (App Router) | File-based routing, SSR/SSG, API routes |
| Styling | Tailwind CSS | Utility-first, easy responsive design |
| Animations | CSS + Framer Motion | Space effects + scroll/hover interactions |
| Auth | Firebase Auth | Email/password + Google login |
| Database | Firestore | Real-time NoSQL, easy to learn |
| Storage | Firebase Storage | Product images |
| Payments | Stripe | Industry-standard checkout |
| Deployment | Vercel | One-click Next.js deployment |

---

## 📁 Project Structure

```
celestial/
├── app/                        # Next.js App Router
│   ├── layout.tsx              # Root layout (navbar, footer)
│   ├── page.tsx                # Homepage (hero, featured products)
│   ├── menu/
│   │   └── page.tsx            # Full menu / product listing
│   ├── product/
│   │   └── [id]/page.tsx       # Individual product page
│   ├── cart/
│   │   └── page.tsx            # Cart page
│   ├── checkout/
│   │   └── page.tsx            # Stripe checkout
│   ├── orders/
│   │   └── page.tsx            # Order history (auth protected)
│   ├── admin/
│   │   ├── page.tsx            # Admin dashboard
│   │   ├── products/page.tsx   # Manage products
│   │   └── orders/page.tsx     # Manage orders
│   └── api/
│       ├── checkout/route.ts   # Stripe payment intent
│       └── webhook/route.ts    # Stripe webhook handler
│
├── components/
│   ├── ui/                     # Reusable UI components
│   │   ├── Button.tsx
│   │   ├── Card.tsx
│   │   └── Modal.tsx
│   ├── layout/
│   │   ├── Navbar.tsx
│   │   └── Footer.tsx
│   ├── home/
│   │   ├── Hero.tsx            # Animated space hero section
│   │   ├── FeaturedProducts.tsx
│   │   └── StorySection.tsx
│   ├── menu/
│   │   ├── ProductGrid.tsx
│   │   ├── ProductCard.tsx
│   │   └── CategoryFilter.tsx
│   └── animations/
│       ├── StarField.tsx       # Twinkling stars background
│       ├── NebulaBlob.tsx      # Floating nebula effect
│       └── FloatingPlanet.tsx  # Decorative planets
│
├── lib/
│   ├── firebase.ts             # Firebase config & init
│   ├── firestore.ts            # DB helper functions
│   ├── stripe.ts               # Stripe client config
│   └── auth.ts                 # Auth helper functions
│
├── hooks/
│   ├── useAuth.ts              # Auth state hook
│   ├── useCart.ts              # Cart state (Zustand)
│   └── useProducts.ts          # Firestore product fetching
│
├── store/
│   └── cartStore.ts            # Zustand global cart state
│
├── types/
│   └── index.ts                # TypeScript types
│
├── public/
│   └── images/                 # Static images
│
├── .env.local                  # Environment variables (never commit!)
├── tailwind.config.ts
├── next.config.ts
└── package.json
```

---

## 🗺️ Development Roadmap

Work through these phases **in order**. Each phase builds on the last.

---

### Phase 0 — Setup (Day 1)

**Goal:** Working project skeleton you can run locally.

```bash
# 1. Create Next.js project
npx create-next-app@latest celestial --typescript --tailwind --app --eslint

# 2. Install core dependencies
cd celestial
npm install firebase stripe @stripe/stripe-js zustand framer-motion

# 3. Install dev tools
npm install -D @types/stripe
```

**Tasks:**
- [ ] Create the project with the command above
- [ ] Set up Firebase project at [console.firebase.google.com](https://console.firebase.google.com)
  - Enable Firestore, Authentication (Email + Google), Storage
- [ ] Create Stripe account at [stripe.com](https://stripe.com)
- [ ] Add `.env.local` (see Environment Variables section below)
- [ ] Create basic folder structure (`components/`, `lib/`, `hooks/`, `store/`, `types/`)
- [ ] Push initial commit to GitHub

---

### Phase 1 — Foundation & Design System (Days 2–4)

**Goal:** Nailed-down visual identity, reusable components, and space animations.

**Tasks:**
- [ ] Configure `tailwind.config.ts` with Celestial color palette
- [ ] Add Google Fonts in `app/layout.tsx` (suggested: *Cinzel* for headings, *DM Sans* for body)
- [ ] Build `Navbar.tsx` — logo, nav links, cart icon with item count badge
- [ ] Build `Footer.tsx` — links, social icons, tagline
- [ ] Build base UI components — `Button.tsx`, `Card.tsx`
- [ ] Build `StarField.tsx` — CSS animated twinkling stars (see Animation Guide below)
- [ ] Build `NebulaBlob.tsx` — soft glowing blur blobs using CSS `radial-gradient` + animation

**Tailwind Color Palette to add to `tailwind.config.ts`:**
```js
colors: {
  space: {
    black:  '#07050f',
    deep:   '#0d0b1e',
    purple: '#1a1240',
    nebula: '#6b3fa0',
    star:   '#f5e6c8',
    gold:   '#e8c97a',
    glow:   '#c084fc',
  }
}
```

---

### Phase 2 — Firebase: Auth & Database (Days 5–7)

**Goal:** Working login system and products stored/fetched from Firestore.

**Tasks:**
- [ ] Write `lib/firebase.ts` — initialize Firebase app
- [ ] Write `lib/auth.ts` — `signIn`, `signOut`, `signUp`, `signInWithGoogle` functions
- [ ] Write `hooks/useAuth.ts` — listens to `onAuthStateChanged`, exposes user state
- [ ] Build Login/Signup UI (can be a modal or `/auth` page)
- [ ] Write `lib/firestore.ts` — CRUD functions for products and orders
- [ ] Manually add 5–10 products to Firestore via Firebase Console (no admin UI yet)
- [ ] Write `hooks/useProducts.ts` — fetches all products from Firestore

**Firestore Data Structure:**
```
/products/{productId}
  name: string          → "Nebula Croissant"
  description: string   → "Layered with galaxy butter..."
  price: number         → 4.50  (in dollars)
  category: string      → "pastries" | "cakes" | "drinks"
  imageUrl: string      → Firebase Storage URL
  inStock: boolean      → true
  featured: boolean     → false

/orders/{orderId}
  userId: string
  items: [{ productId, name, price, quantity }]
  total: number
  status: string        → "pending" | "confirmed" | "ready" | "completed"
  stripePaymentId: string
  createdAt: timestamp
```

---

### Phase 3 — Homepage & Menu (Days 8–11)

**Goal:** A stunning homepage and a browsable product menu.

**Tasks:**
- [ ] Build `Hero.tsx` — fullscreen with StarField background, headline, CTA button, floating animation
- [ ] Build `FeaturedProducts.tsx` — fetch featured:true products, display in a card grid
- [ ] Build `StorySection.tsx` — bakery story/about with parallax scroll effect
- [ ] Build `menu/page.tsx` — full product listing
- [ ] Build `ProductCard.tsx` — image, name, price, "Add to Cart" button
- [ ] Build `CategoryFilter.tsx` — filter by category (pastries, cakes, drinks)
- [ ] Build `product/[id]/page.tsx` — individual product detail page

---

### Phase 4 — Cart & State (Days 12–14)

**Goal:** Persistent cart that works across the site.

**Tasks:**
- [ ] Set up Zustand store in `store/cartStore.ts`
  - State: `items[]`, `addItem`, `removeItem`, `updateQuantity`, `clearCart`
- [ ] Write `hooks/useCart.ts` — wrapper around the Zustand store
- [ ] Wire "Add to Cart" buttons on product cards to the store
- [ ] Build `cart/page.tsx` — list cart items, quantities, subtotal, "Proceed to Checkout" button
- [ ] Show cart item count badge in Navbar

**Zustand Cart Store shape:**
```ts
interface CartItem {
  id: string
  name: string
  price: number
  quantity: number
  imageUrl: string
}

interface CartStore {
  items: CartItem[]
  addItem: (item: CartItem) => void
  removeItem: (id: string) => void
  updateQuantity: (id: string, qty: number) => void
  clearCart: () => void
  total: () => number
}
```

---

### Phase 5 — Stripe Payments (Days 15–18)

**Goal:** Working end-to-end checkout flow.

**Stripe Flow Explained:**
```
User clicks "Checkout"
  → Your frontend calls your own API route
    → API route calls Stripe to create a PaymentIntent
      → Returns clientSecret to frontend
        → Stripe.js handles card input & payment
          → On success → Stripe sends webhook to your API
            → Webhook saves order to Firestore
```

**Tasks:**
- [ ] Write `app/api/checkout/route.ts` — creates Stripe PaymentIntent, returns `clientSecret`
- [ ] Build `checkout/page.tsx` — Stripe Elements card form using `@stripe/react-stripe-js`
- [ ] Handle payment success → redirect to confirmation page
- [ ] Write `app/api/webhook/route.ts` — listens for `payment_intent.succeeded`
  - On success: save order to Firestore, clear cart
- [ ] Test end-to-end with Stripe test cards (`4242 4242 4242 4242`)

---

### Phase 6 — Orders & Auth Guards (Days 19–21)

**Goal:** Users can view their order history. Admin pages are protected.

**Tasks:**
- [ ] Build `orders/page.tsx` — fetch orders from Firestore where `userId == currentUser.uid`
- [ ] Create auth guard — redirect to login if not authenticated
- [ ] Build basic `admin/page.tsx` — dashboard with order count, recent orders
- [ ] Build `admin/products/page.tsx` — list products, add/edit/delete
- [ ] Build `admin/orders/page.tsx` — view all orders, update order status
- [ ] Protect admin routes — check for admin role in Firestore user document

---

### Phase 7 — Animations & Polish (Days 22–26)

**Goal:** The site feels alive and space-themed throughout.

**Animations to implement:**
- [ ] **StarField** — hundreds of small `div`s or a `<canvas>`, CSS `@keyframes twinkle` with randomized `animation-delay`
- [ ] **Scroll reveal** — Framer Motion `whileInView` to fade/slide elements in as you scroll
- [ ] **Hero float** — CSS `@keyframes float` on the main headline or planet image
- [ ] **Card hover** — Tailwind `hover:scale-105 transition-transform` + subtle glow with `box-shadow`
- [ ] **Nebula background** — Large blurred radial gradients behind sections, CSS `@keyframes nebulaDrift` slowly moves them
- [ ] **Button shimmer** — CSS `::after` pseudo-element sweeping shimmer on CTA buttons
- [ ] **Page transitions** — Framer Motion `AnimatePresence` wrapping page content

---

### Phase 8 — Deployment (Day 27–28)

**Goal:** Live site on the internet.

**Tasks:**
- [ ] Set up Vercel account, connect GitHub repo
- [ ] Add all environment variables in Vercel dashboard
- [ ] Configure Stripe webhook to point to your live Vercel URL
- [ ] Switch Stripe from test mode to live mode
- [ ] Set Firebase security rules (see below)
- [ ] Test everything on production
- [ ] (Optional) Connect custom domain

**Firebase Security Rules (Firestore):**
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Products: anyone can read, only admins can write
    match /products/{id} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.token.admin == true;
    }
    // Orders: users can read their own, only admins can read all
    match /orders/{id} {
      allow read: if request.auth != null &&
        (resource.data.userId == request.auth.uid || request.auth.token.admin == true);
      allow create: if request.auth != null;
      allow update: if request.auth != null && request.auth.token.admin == true;
    }
  }
}
```

---

## 🔐 Environment Variables

Create `.env.local` in your project root. **Never commit this file.**

```env
# Firebase
NEXT_PUBLIC_FIREBASE_API_KEY=
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=
NEXT_PUBLIC_FIREBASE_PROJECT_ID=
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=
NEXT_PUBLIC_FIREBASE_APP_ID=

# Stripe
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
```

---

## 🎨 Animation Guide (CSS Basics)

### Twinkling Stars
```css
@keyframes twinkle {
  0%, 100% { opacity: 1; transform: scale(1); }
  50%       { opacity: 0.2; transform: scale(0.8); }
}

.star {
  position: absolute;
  width: 2px;
  height: 2px;
  background: white;
  border-radius: 50%;
  animation: twinkle 3s infinite;
  /* Randomize per star with inline style: animation-delay: Xs */
}
```

### Floating Element
```css
@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50%       { transform: translateY(-20px); }
}

.floating { animation: float 6s ease-in-out infinite; }
```

### Nebula Glow Blob
```css
.nebula-blob {
  position: absolute;
  width: 600px;
  height: 600px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(192, 132, 252, 0.15), transparent 70%);
  filter: blur(80px);
  animation: nebulaDrift 20s ease-in-out infinite alternate;
}

@keyframes nebulaDrift {
  from { transform: translate(0, 0) scale(1); }
  to   { transform: translate(60px, 40px) scale(1.1); }
}
```

### Framer Motion Scroll Reveal
```tsx
import { motion } from 'framer-motion'

<motion.div
  initial={{ opacity: 0, y: 40 }}
  whileInView={{ opacity: 1, y: 0 }}
  viewport={{ once: true }}
  transition={{ duration: 0.6, ease: 'easeOut' }}
>
  Your content here
</motion.div>
```

---

## 🧠 Key Concepts to Learn Along the Way

| Concept | Where you'll use it | Resource |
|---|---|---|
| Next.js App Router | Every page | [nextjs.org/docs](https://nextjs.org/docs) |
| React Server vs Client Components | Data fetching vs interactivity | Next.js docs → "Server Components" |
| TypeScript basics | Typing props, state, API responses | [typescriptlang.org/docs](https://www.typescriptlang.org/docs/) |
| Tailwind CSS | All styling | [tailwindcss.com/docs](https://tailwindcss.com/docs) |
| CSS Animations | Space effects | MDN Web Docs → CSS Animations |
| Framer Motion | Scroll + page transitions | [framer.com/motion](https://www.framer.com/motion/) |
| Firebase/Firestore | Data, auth | [firebase.google.com/docs](https://firebase.google.com/docs) |
| Zustand | Cart state | [zustand-demo.pmnd.rs](https://zustand-demo.pmnd.rs/) |
| Stripe Elements | Payment UI | [stripe.com/docs/stripe-js](https://stripe.com/docs/stripe-js) |
| Webhook handling | Order confirmation | Stripe docs → Webhooks |

---

## 🌙 Pages Summary

| Route | Description | Auth Required |
|---|---|---|
| `/` | Homepage — hero, featured products, story | No |
| `/menu` | Full product listing with filters | No |
| `/product/[id]` | Product detail page | No |
| `/cart` | Cart review | No |
| `/checkout` | Stripe payment form | Yes |
| `/orders` | Order history | Yes |
| `/admin` | Admin dashboard | Admin only |
| `/admin/products` | Manage products | Admin only |
| `/admin/orders` | Manage all orders | Admin only |

---

## 📦 Suggested Product Categories

- **Nebula Pastries** — croissants, danishes, tarts
- **Galaxy Cakes** — layer cakes, cheesecakes
- **Comet Cookies** — shortbread, macarons, brownies  
- **Stardust Drinks** — lattes, teas, hot chocolates

---

## ✅ Pre-launch Checklist

- [ ] All pages work on mobile (responsive)
- [ ] Images are optimized (use `next/image`)
- [ ] Loading states shown during data fetches
- [ ] Error states handled (product not found, payment failed)
- [ ] Stripe test mode fully tested
- [ ] Firebase security rules deployed
- [ ] `.env.local` is in `.gitignore`
- [ ] Lighthouse score > 80 on all categories

---

## 🛸 Nice-to-Haves (After v1)

- Product search
- Reviews & ratings
- Email confirmations with Resend or SendGrid
- Pre-order / custom cake request form
- Loyalty points system
- PWA support (installable app)

---

*Built with love and stardust. 🌠*


---


This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

This project uses [`next/font`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts) to automatically optimize and load [Geist](https://vercel.com/font), a new font family for Vercel.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details.


Full stack website from scratch, I wont use AI , I will only search on google until I lose my head. 
