# NEORENT

Modern car rental platform built with Next.js 15, Supabase, and TypeScript.

## Features

- **User Authentication**: Secure login and registration with Supabase Auth
- **Car Listings**: Browse and search available vehicles
- **Booking System**: Reserve cars with date/time selection
- **Payment Integration**: Secure payment processing
- **User Dashboard**: Manage bookings and profile
- **Admin Panel**: Manage fleet and bookings

## Tech Stack

- **Framework**: Next.js 15 (App Router)
- **Database**: Supabase (PostgreSQL)
- **Authentication**: Supabase Auth
- **Styling**: Tailwind CSS
- **Type Safety**: TypeScript
- **UI Components**: Radix UI + shadcn/ui

## Getting Started

1. **Clone the repository**
   ```bash
   git clone https://github.com/alexsrebernic/neorent.git
   cd neorent
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Set up environment variables**
   ```bash
   cp .env.example .env.local
   ```
   Fill in your Supabase credentials and other required variables.

4. **Run the development server**
   ```bash
   npm run dev
   ```

5. **Open your browser**
   Navigate to [http://localhost:3000](http://localhost:3000)

## Project Structure

```
neorent/
├── app/                    # Next.js 15 app directory
├── components/             # Reusable UI components
├── lib/                   # Utility functions and configurations
├── types/                 # TypeScript type definitions
├── supabase/              # Database migrations and types
└── ai-docs/               # Project documentation
```

## Development

- **Database**: Supabase with Row Level Security (RLS)
- **Authentication**: Server-side auth with Supabase
- **Styling**: Tailwind CSS with custom design system
- **State Management**: Server Components + Server Actions
- **Type Safety**: Strict TypeScript configuration

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

This project is licensed under the MIT License.