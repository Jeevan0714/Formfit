# Formfit Implementation Plan

A comprehensive proposal for **Formfit**, an intelligent, scientifically-backed fitness and nutrition platform designed to solve gym-floor anxiety, trainer inconsistency, and personalized client adaptation.

---

## 1. App vs. Web: Detailed Evaluation

To address the core problem (gym-goers facing inconsistent training advice and feeling uncomfortable asking trainers on the gym floor), the platform must be accessible **directly during a workout**.

| Platform | Pros | Cons | Recommendation |
| :--- | :--- | :--- | :--- |
| **Mobile App** (React Native / Flutter) | - **Portable**: Used on the gym floor.<br>- **Camera Access**: Scan machine QR codes for guides.<br>- **Sensors/Integrations**: Syncs with Apple Health / Google Fit.<br>- **Engagement**: Push notifications for workouts, meals, water.<br>- **Offline Mode**: Works in areas with poor gym Wi-Fi. | - Higher initial development effort.<br>- App store approval cycles. | **Primary Platform (100% Recommended)**. A mobile app is essential for in-gym utility, tracking, and real-time exercise adaptation. |
| **Web App** (React/Next.js) | - **Zero Install**: Low friction for first-time onboarding.<br>- **Admin Dashboard**: Easy tool for gym owners or premium coaches.<br>- **Content Consumption**: Easier to read detailed meal plans/recipes on desktop. | - Harder to use actively while lifting.<br>- No native push notifications on iOS. | **Secondary Platform**. Recommended for a landing page, user onboarding, and a potential Admin Dashboard for gym administrators. |

### Proposed Approach
We will build **Formfit** as a **cross-platform Mobile App** (using React Native with Expo) for users, paired with a lightweight **Web Backend & Dashboard** (Next.js) for database management, content entry, and optional trainer monitoring.

---

## 2. Core Features & Scientific Foundation

### A. The Scientific Workout Engine (Customization & Periodization)
1. **Dynamic Injury Mapping**:
   - A logic matrix that links injuries to affected joints and muscle groups.
   - *Example*: If a user has a "Lower Back Herniation," the system automatically flags and replaces spinal-loading exercises (e.g., Barbell Back Squats, Deadlifts) with spine-sparing alternatives (e.g., Goblet Squats, Bulgarian Split Squats, Leg Press) and adds specific core-stability exercises (McGill Big 3).
2. **Mental & Physical Readiness (Daily Auto-Regulation)**:
   - A quick 3-question check-in before starting: *Sleep quality, Soreness, Stress/Energy level*.
   - If energy is low or soreness is high, the engine automatically scales back the volume (sets/reps) or intensity (weight/RPE - Rate of Perceived Exertion) for that session, preventing burnout and injury.
3. **Structured Progression & Muscle Balance**:
   - The workout plan updates weekly to implement **progressive overload** (gradually increasing weight, reps, or altering tempo).
   - Tracks weekly set volume per muscle group to ensure optimal recovery and prevent muscle imbalances (e.g., matching pushing volume with pulling volume).

### B. Personalized Nutrition Engine (Balanced Diet)
1. **TDEE & Macro Calculator**:
   - Calculates Total Daily Energy Expenditure (TDEE) using the Mifflin-St Jeor equation.
   - Adjusts calories based on goals (fat loss, muscle gain, recomp) and dynamic workout activity.
   - Distributes macros based on physical characteristics and preferences (higher protein for recovery, customized fat/carb ratios).
2. **Adaptive Meal Planner**:
   - Provides daily custom meal recommendations based on dietary preferences (vegetarian, vegan, non-veg, keto, etc.) and allergies.
   - Offers simple recipe instructions, shopping lists, and dynamic meal-swapping capabilities.

### C. Self-Guided Gym Experience (Comfort & Confidence)
1. **Discreet Workout Companion**:
   - High-quality, looping video demonstrations for every exercise.
   - Direct, concise form tips focused on safety and execution.
   - *No-shame instructions*: Simple explanations of how to set up gym machines, resolving the barrier for beginners who feel self-conscious asking trainers.
2. **Interactive Logging**:
   - Simple, fast interface to log weight and reps.
   - Rest timer with visual/haptic cues so the user stays focused.

---

## 3. Technology Stack Recommendation

To ensure scalability, modern aesthetics, and rapid development, the following stack is recommended:

```mermaid
graph TD
    User([Gym User]) <-->|React Native App| App[Mobile Frontend: Expo / React Native]
    Admin([Gym Admin / Coaches]) <-->|Next.js Web| Web[Web Admin Dashboard]
    App <-->|HTTPS / WSS| API[Backend API: Node.js / FastAPI]
    Web <-->|HTTPS| API
    API <-->|SQL Client| DB[(Database: PostgreSQL + Prisma)]
    API <-->|Redis Client| Cache[(Cache: Redis)]
    API <-->|API Calls| AI[AI Engine: Gemini API / Rule Engine]
```

### Frontend (Mobile App & Web Admin)
- **Mobile Framework**: **React Native with Expo**. Allows building for iOS & Android simultaneously. Expo provides high-performance components, easy local testing, and fast compilation.
- **Styling**: **NativeWind (Tailwind CSS for React Native)** or **Styled Components** paired with a rich, dark-mode design system.
- **State Management**: **Zustand** (lightweight and highly performant) or **Redux Toolkit**.

### Backend (API Services)
- **Language/Framework**: **FastAPI (Python)** or **Node.js (NestJS)**. 
  - *Recommendation*: **FastAPI (Python)** if we want to run advanced machine learning, biomechanical data models, or LLM-based workout adjustments natively. **NestJS** if we want a highly structured TypeScript ecosystem.
- **Database**: **PostgreSQL** with **Prisma ORM**. Great for handling structured relations (User -> Workouts -> Exercises -> Sets).
- **Authentication**: **Supabase Auth** or **Firebase Auth** (simplifies secure sign-ins, social logins, and password resets).

### Adaptation & Personalization Engine
- **Heuristic Rule Engine**: Hardcoded scientific rules for injury prevention (e.g., knee injury = replace quad-dominant knee-flexion with hip-dominant movements or machine-guided exercises).
- **Generative AI (Gemini 1.5/2.0 API)**: Used to synthesize natural language coaching advice, explain *why* an exercise was swapped, and generate highly custom meal variations based on weird leftovers or strict diets.

---

## 4. User Review Required

> [!IMPORTANT]
> **Aesthetic Theme & Branding**: For a modern gym platform, a premium dark-themed aesthetic with vibrant accents (e.g., neon lime, cyber cyan, or electric orange) provides a motivating, premium feel. We should align on the color theme early.
> 
> **Workout Data Source**: To make the workout and diet plans scientific, we must use a curated database of exercises (with video links/GIFs and primary/secondary muscle indicators) and foods. Do you want to build this curated list manually, scrape public exercise libraries, or integrate with a third-party fitness API?

---

## 5. Open Questions

> [!WARNING]
> 1. **Gym-Specific vs. Universal**: Is this app built for a *specific* gym (syncing with their exact machine brands and trainer schedules), or is it a *universal* product that any gym-goer can download and use in any gym?
> 2. **Trainer Involvement**: Should gym trainers have their own portal in the app to assign workouts to their specific clients, or should the app's algorithm act as the sole "virtual trainer"?
> 3. **Injuries & Medical Disclaimer**: Because we are recommending exercises for people with injuries, we must include a clear medical disclaimer. Are there specific severe injuries you want the app to outright block and recommend a physical therapist for, rather than attempting to auto-program around?

---

## 6. Verification Plan

### Stage 1: Core Algorithm Testing
- Run test cases simulating diverse profiles (e.g., "Beginner, 24yo Female, Lower Back Pain, Vegetarian" vs. "Advanced, 35yo Male, Shoulder Impingement, Keto").
- Verify that generated workouts strictly exclude restricted movements and contain balanced weekly muscle volume.

### Stage 2: Mobile Interface Validation
- Test onboarding flows, logging responsiveness, and offline workout synchronization.
- Test visual layout on multiple device sizes (iOS and Android).
