# QuantFlash

A micro-learning web app for quant-finance interview prep built with Next.js and Supabase.

## Features

- 🎯 Spaced repetition flashcards for quant finance concepts
- 🔐 Magic link authentication via Supabase
- 📱 Progressive Web App (PWA) ready
- 💾 PostgreSQL database with Supabase
- 🎨 Clean, responsive UI with Tailwind CSS

## Tech Stack

- **Framework**: Next.js 14 (App Router)
- **Database**: Supabase (PostgreSQL)
- **Authentication**: Supabase Auth
- **Styling**: Tailwind CSS
- **Language**: TypeScript

## Getting Started

### Prerequisites

- Node.js 18+ 
- npm or yarn
- Supabase account

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd quantflash
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Set up Supabase**
   - Create a new project at [supabase.com](https://supabase.com)
   - Go to Settings > API to find your project URL and anon key
   - Copy `.env.example` to `.env.local`:
     ```bash
     cp .env.example .env.local
     ```
   - Fill in your Supabase credentials in `.env.local`:
     ```
     NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
     NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
     ```

4. **Set up the database**
   
   **Option A: Using Supabase Dashboard**
   - Go to your Supabase project dashboard
   - Navigate to SQL Editor
   - Copy and paste the contents of `supabase/migrations/000_init.sql`
   - Run the SQL to create tables and policies

   **Option B: Using Supabase CLI** (if you have it installed)
   ```bash
   supabase db reset
   ```

5. **Seed the database**
   
   You'll need to add a service role key to your `.env.local` for seeding:
   ```
   SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
   ```
   
   Then run the seed script:
   ```bash
   npx tsx supabase/seed.ts
   ```

6. **Start the development server**
   ```bash
   npm run dev
   ```

7. **Open the app**
   Navigate to [http://localhost:3000](http://localhost:3000)

## Usage

1. **Sign up/Sign in**: Use the magic link authentication by entering your email
2. **Study**: Go to `/study` to start practicing with flashcards
3. **Progress**: Answer questions and track your progress with spaced repetition

## Database Schema

### Tables

- **questions**: Stores all flashcard questions
  - `id`: UUID primary key
  - `question_text`: The question content
  - `hint_text`: Short hint (≤120 chars)
  - `answer_text`: Brief answer
  - `long_answer_text`: Detailed explanation
  - `tags`: Array of topic tags
  - `created_at`: Timestamp

- **user_answers**: Tracks user progress
  - `id`: UUID primary key
  - `user_id`: References auth.users
  - `question_id`: References questions
  - `correct`: Boolean (true if answered correctly)
  - `answered_at`: When the question was answered
  - `due_at`: When the question is due for review

- **users**: Managed by Supabase Auth
  - `id`: UUID primary key
  - `email`: User email
  - `created_at`: Timestamp

## Spaced Repetition Logic

The app uses a simplified SM-2 algorithm:
- **Correct answers**: Question due again in 1 day
- **Incorrect answers**: Question due again in 4 hours

## API Routes

- `POST /api/answer`: Submit an answer for a question
  ```json
  {
    "question_id": "uuid",
    "correct": boolean,
    "due_at": "ISO-8601-timestamp"
  }
  ```

## Development

### Project Structure

```
├── app/                    # Next.js App Router
│   ├── api/               # API routes
│   ├── auth/              # Authentication pages
│   ├── study/             # Study page
│   ├── globals.css        # Global styles
│   ├── layout.tsx         # Root layout
│   └── page.tsx           # Home page
├── lib/                   # Utilities
│   └── supabase.ts        # Supabase client configuration
├── supabase/              # Database files
│   ├── migrations/        # SQL migrations
│   └── seed.ts            # Seed script
├── types/                 # TypeScript type definitions
└── seed_data.json         # Question data for seeding
```

### Commands

- `npm run dev`: Start development server
- `npm run build`: Build for production
- `npm run start`: Start production server
- `npm run lint`: Run ESLint

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is licensed under the MIT License.