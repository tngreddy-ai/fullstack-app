# UI and Authentication Enhancements Plan

## Goal Description
Implement authentication (Sign In/Sign Up flip card + Google option), fix route protections, enhance the quiz form builder (validations, live interactive preview, emoji picker fixes, layout tweaks), and refine the quiz-taking UI (faster animations, styled "No" button).

## Proposed Changes

### 1. Authentication & Navigation
- **`src/components/layout/Header.jsx`**
  - Fix the "Home" link so it navigates to `/` and scrolls to top smoothly instead of scrolling to a missing element.
  - Remove all "Get Started" buttons when the user is logged out (replace with "Log In" or hide).
- **`src/App.jsx`**
  - Add a route for `/login` mapped to the new `Auth` page.
  - Wrap the `/dashboard` route logic in a component that explicitly checks `user` context and navigates to `/login` if not found.
- **`src/pages/Auth/index.jsx` [NEW]**
  - Create a designated page for authentication.
  - Build a flip-card interface utilizing `framer-motion` (with `rotateY`) to seamlessly toggle between "Sign In" and "Sign Up".
  - Add a "Sign in with Google" mock button.
  - Utilize the existing `AuthContext` to log the user in successfully and seamlessly redirect to `/dashboard`.

### 2. Form Page (Create/Edit Quiz)
- **`src/pages/EditQuiz/index.jsx`**
  - Enhance `validate()` to thoroughly check options strings and media URLs if they exist. Add `feedback_animation: 'burst'` default for new questions.
- **`src/components/builder/QuizSettingsForm.jsx`**
  - Update the "Media & Music" section by replacing `md:grid-cols-2` with `grid-cols-1` to place URLs on separate rows.
  - Add state and the `EmojiPicker` popup triggered from the "Intro message" textarea.
- **`src/components/builder/EmojiPicker.jsx`**
  - Add `framer-motion` to the root container. Change the initial entry animation from sliding to scaling/easing out from the trigger button icon.
- **`src/components/builder/QuestionEditor.jsx`**
  - Add a dropdown to select the `feedback_animation` for the correct emoji (options: "burst", "float", "bounce").
- **`src/pages/EditQuiz/QuizPreview.jsx`**
  - Transform this component from a static "first slide" display into an interactive frame.
  - Embed the viewer screens (`QuizFrontPage`, `QuizQuestionScreen`, etc.) to allow the creator to interactively test the questions out *live* without leaving the editor.

### 3. Quiz Question Pages (Viewer)
- **`src/components/quiz-viewer/QuizQuestionScreen.jsx`**
  - Decrease delay timeouts in `handleAnswer` when clicking options (e.g. from 1000ms to 400ms) to lessen the waiting time between loading questions.
- **`src/components/quiz-viewer/QuizFinalScreen.jsx`**
  - Move the changing text logic (`currentMsg`) directly into the "No" button text label rather than displaying it in a separate `<p>` on the panel.
  - Turn the "No" button background purely red (`bg-red-500 text-white font-bold border-red-600`).
  - Increase the `stiffness` and lower `damping` on the framer-motion escape animation so it darts away snappily.

## Verification Plan

### Automated Tests
- *(No automated test suites found based on current workspace layout. Assuming typical local dev check.)*

### Manual Verification
1. Click the 'Home' navigation button to ensure it navigates cleanly. Open header without 'Get Started'.
2. Visit `/dashboard` to ensure it kicks unauthenticated users to `/login`.
3. In `/login`, verify the flip transition and Google button exist, and sign in.
4. On the Edit Quiz page, try picking an emoji for the Intro Message and watch it ease-out.
5. In the Edit Quiz live preview, ensure clicking "Start" walks through the actual live questions added in the sidebar.
6. Verify the "No" button moves swiftly, is red, and text changes on the button itself in the final screen.
