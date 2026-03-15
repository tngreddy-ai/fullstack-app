# Quiz Application - Agent Guide

## Project Overview

This is a **React-based interactive quiz application** that allows users to create, edit, and take quizzes. The application features a modern UI with authentication, quiz form building capabilities, and an engaging quiz-taking experience with animations.

**Note:** This repository currently contains project documentation and planning files only. The actual source code is expected to be added separately.

## Technology Stack

Based on the implementation plans and walkthrough, the project uses:

- **Framework:** React (likely with Vite, based on `npm run dev` usage)
- **Routing:** React Router
- **Styling:** Tailwind CSS
- **Animations:** Framer Motion
- **State Management:** React Context API (AuthContext)
- **Package Manager:** npm

## Project Structure (Expected)

Based on the implementation plan, the source code should be organized as follows:

```
src/
├── components/
│   ├── layout/
│   │   └── Header.jsx          # Navigation header with auth state
│   ├── builder/
│   │   ├── QuizSettingsForm.jsx # Quiz configuration form
│   │   ├── QuestionEditor.jsx   # Individual question editor
│   │   └── EmojiPicker.jsx      # Emoji selection component
│   └── quiz-viewer/
│       ├── QuizQuestionScreen.jsx  # Quiz taking interface
│       ├── QuizFinalScreen.jsx     # Results/final page
│       └── QuizFrontPage.jsx       # Quiz intro screen
├── pages/
│   ├── Auth/
│   │   └── index.jsx           # Sign in / Sign up flip-card page
│   ├── EditQuiz/
│   │   ├── index.jsx           # Create/edit quiz form
│   │   └── QuizPreview.jsx     # Live interactive preview
│   └── Dashboard/              # Protected user dashboard
├── context/
│   └── AuthContext.jsx         # Authentication state management
└── App.jsx                     # Main app with route definitions
```

## Key Features

### 1. Authentication System
- Flip-card interface for Sign In / Sign Up using Framer Motion (`rotateY` animation)
- Mock Google Sign-in integration
- Route protection for authenticated pages (e.g., `/dashboard` redirects to `/login` if unauthenticated)
- Auth state managed via React Context API

### 2. Quiz Form Builder (Create/Edit Quiz)
- **Form Validations:** Robust checking for questions, options, and media URLs
- **Live Preview:** Interactive preview that renders actual quiz components in real-time
- **Emoji Picker:** Custom emoji picker with scale-out animation from trigger icon
- **Media & Music:** Separate rows for "End Gif URL" and "Background Music" inputs
- **Success Animations:** Configurable feedback animations (burst, float, bounce) for correct answers

### 3. Quiz Taking UI
- **Fast Transitions:** Reduced delay timeouts (~50% faster) between questions
- **Interactive "No" Button:** On the final screen, the "No" button:
  - Is styled bright red (`bg-red-500`)
  - Uses spring physics with high stiffness/low damping for snappy escape animation
  - Displays changing text directly on the button as it moves

## Build and Development Commands

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## Development Guidelines

### Code Style
- Use functional React components with hooks
- Use Tailwind CSS for styling (utility-first approach)
- Use Framer Motion for all animations
- Follow existing component organization (components/ vs pages/)

### Component Patterns
- **Layout Components:** Place in `src/components/layout/`
- **Page Components:** Place in `src/pages/{PageName}/`
- **Reusable UI Components:** Place in `src/components/{category}/`
- **Context Providers:** Place in `src/context/`

### Animation Standards
- Use Framer Motion's `motion` components
- For transitions: Use `initial`, `animate`, and `exit` props
- For the flip-card auth: Use `rotateY` transform with `transition={{ duration: 0.6 }}`
- For emoji picker: Use scale animation (`scale: 0` to `scale: 1`) with ease-out

### Form Validation
- Implement `validate()` function in form components
- Check for empty option strings
- Validate media URLs if provided
- Set defaults for new questions (e.g., `feedback_animation: 'burst'`)

## Testing Strategy

**Current Status:** No automated test suites are configured.

### Manual Verification Checklist

Before considering a feature complete, verify:

1. **Authentication Flow:**
   - Visit `/dashboard` unauthenticated → should redirect to `/login`
   - Click between Sign In / Sign Up → card flips smoothly
   - Sign in with mock Google → redirects to dashboard

2. **Quiz Form Builder:**
   - Navigate to `/quiz/new`
   - Test emoji picker animation (should scale from icon)
   - Verify Media & Music inputs are on separate rows
   - Test live preview interactivity (click "Start" to walk through questions)

3. **Quiz Taking:**
   - Open a quiz preview (`/quiz/:id/view`)
   - Verify question transitions are fast
   - On final screen, try clicking "No" button → should dart away quickly in red

## Security Considerations

- **Route Guards:** Always wrap protected routes with authentication checks
- **Mock Authentication:** Currently using mock auth; replace with real backend integration for production
- **Form Validation:** Validate all user inputs on both client and server sides
- **URL Validation:** Sanitize media URLs (End Gif, Background Music) before rendering

## Deployment Notes

- This is a client-side React application
- Configure your deployment platform (Vercel, Netlify, etc.) to:
  1. Run `npm install` and `npm run build`
  2. Serve the `dist/` folder (if using Vite) or `build/` folder (if using CRA)
  3. Enable SPA fallback for React Router (all routes → index.html)

## Future Enhancements (Based on Task History)

- Real backend API integration for authentication and quiz data persistence
- Unit and integration tests (Jest + React Testing Library)
- E2E tests (Playwright or Cypress)
- Accessibility improvements (ARIA labels, keyboard navigation)
- Mobile-responsive optimizations
