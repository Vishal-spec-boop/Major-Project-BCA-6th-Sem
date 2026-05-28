# 🎓 BuildBox LMS — Full System Prompt & Page Map

> **Stack:** HTML · CSS · JavaScript · PHP · MySQL  
> **Design Theme:** Warm cream background `#F2EDE3` · Dark sidebar `#1A1915` · Accent orange `#D85A30` · Font: DM Sans + DM Serif Display  
> **Reference UI:** See `dashboard.html` for the established component language, color palette, card styles, sidebar layout, and typography — ALL pages must extend this exact theme.

---

## 📐 Global Design System (Apply to Every Page)

```
Background:     #F2EDE3  (warm cream)
Sidebar:        #1A1915  (near-black)
Cards:          #FFFFFF  with 1px border rgba(0,0,0,0.08), border-radius 14px
Accent:         #D85A30  (burnt orange — buttons, progress, highlights)
Success:        #1D9E75  (green — completed states)
Text Primary:   #1A1915
Text Secondary: #7A7670
Text Muted:     #B0ADA8
Font Display:   DM Serif Display (headings, large numbers)
Font Body:      DM Sans 400/500/600
Border Radius:  14px cards, 8px inputs/badges, 100px pills
Sidebar Width:  220px fixed
Card Padding:   20px
```

**Every page shares:**

- The same fixed left sidebar (role-aware: Student / Teacher / Admin)
- The same top bar (avatar, greeting, search, notification bell)
- The same warm cream page background
- The same card component style
- Smooth `fadeUp` entrance animation on cards (0.4s, staggered)
- PHP session check at top of every protected page

---

## 🗂️ Page Index

| #   | File                    | Role              | Description                              |
| --- | ----------------------- | ----------------- | ---------------------------------------- |
| 1   | `login.php`             | All               | Login page                               |
| 2   | `register.php`          | Student / Teacher | Registration with role selection         |
| 3   | `dashboard.php`         | Student           | Home — progress, tasks, featured courses |
| 4   | `courses.php`           | Student           | Browse, search & filter all courses      |
| 5   | `course-detail.php`     | Student           | Single course page + enroll              |
| 6   | `my-learning.php`       | Student           | Enrolled courses + progress              |
| 7   | `lesson.php`            | Student           | Active lesson viewer                     |
| 8   | `teacher-dashboard.php` | Teacher           | Teacher home — stats + pending status    |
| 9   | `teacher-courses.php`   | Teacher           | Manage uploaded courses                  |
| 10  | `upload-course.php`     | Teacher           | Create new course + lectures             |
| 11  | `admin-dashboard.php`   | Admin             | System overview                          |
| 12  | `admin-teachers.php`    | Admin             | Verify / reject teacher profiles         |
| 13  | `admin-courses.php`     | Admin             | Moderate all courses                     |
| 14  | `admin-students.php`    | Admin             | Manage student accounts                  |
| 15  | `profile.php`           | All               | View / edit own profile                  |

---

## 🔐 Page 1 — `login.php`

**Route:** `/login.php` · **Access:** Public (redirect to dashboard if logged in)

### Prompt

```
Build login.php using the BuildBox LMS design theme.

LAYOUT: Full-page split — left half is a warm cream (#F2EDE3) panel with the
BuildBox logo (dark box icon + "BUILDBOX" wordmark), a large DM Serif Display
heading "Welcome back.", a subtext line, and a centered login card (white,
border-radius 14px, padding 32px, max-width 400px).

RIGHT HALF: Dark panel (#1A1915) with a decorative SVG illustration of abstract
geometric shapes in muted orange tones. Below it, a quote or stat about learning.

FORM FIELDS (in the card):
- Email input (full width, 44px height, rounded 8px border)
- Password input with show/hide toggle eye icon
- "Remember me" checkbox + "Forgot password?" link (right-aligned)
- Submit button: full-width, #D85A30 background, white text, "Log In", 44px height,
  border-radius 8px, hover darken to #C04E28
- Divider "or"
- Link: "Don't have an account? Register →"

PHP: POST to login.php, validate email+password against users table, set
$_SESSION['user_id'], $_SESSION['role'] (student/teacher/admin), redirect to
role-appropriate dashboard. Show inline error on failure (red border on inputs +
error message below form).

DB query: SELECT id, name, role, password_hash, is_verified FROM users WHERE email = ?
For teachers: if is_verified = 0, show special "Pending admin approval" message
instead of logging in.
```

---

## 📝 Page 2 — `register.php`

**Route:** `/register.php` · **Access:** Public

### Prompt

```
Build register.php using the BuildBox LMS design theme.

LAYOUT: Same split layout as login.php — left card panel, right dark decorative panel.

ROLE TOGGLE at top of card:
Two pill buttons side by side: "I'm a Student" | "I'm a Teacher"
Active pill: #D85A30 background, white text.
Inactive: white bg, #D85A30 border and text.
Toggle updates a hidden input field `role`.

FORM FIELDS:
- Full Name (text input)
- Email (email input)
- Password (password input with strength indicator bar below — 4 segments,
  fills red → orange → green as password strengthens)
- Confirm Password
- If Teacher role selected: show extra fields:
    - "Your Specialization" (text)
    - "Brief Bio" (textarea, 3 rows)
    - "LinkedIn or Portfolio URL" (text)
  These fields animate into view with a smooth slideDown when Teacher is selected.
- Submit: "Create Account" button — same style as login

VALIDATION: Client-side JS for matching passwords + strength. PHP server-side
validates all fields, checks email uniqueness, hashes password with password_hash().

DB INSERT: INSERT INTO users (name, email, password_hash, role, is_verified, created_at)
For students: is_verified = 1 (auto-approved)
For teachers: is_verified = 0 (awaits admin)

After success: students → redirect to dashboard.php
Teachers → show a full-page warm confirmation card:
  Icon: clock/hourglass in orange
  Heading: "Application Submitted!"
  Body: "Our admin team will review your profile. You'll receive an email once approved."
  Link: "Back to Home"
```

---

## 🏠 Page 3 — `dashboard.php` (Student Home)

**Route:** `/dashboard.php` · **Access:** Student (session required)

### Prompt

```
Build dashboard.php — the student home page. This is the REFERENCE PAGE. All other
pages must match its design exactly.

SIDEBAR (220px, #1A1915):
- BuildBox logo top
- Workspace/school selector dropdown
- Nav links with icons: Home, My Skill Graph, My Bookmarks, Leaderboard
- Divider
- Courses, Lessons, Assessments, Challenges, Career Paths
- Divider
- Settings
- Bottom: Plan badge card (dark bg, plan name "Rookie", Upgrade CTA), Invite a friend button

TOP BAR:
- Left: User avatar circle (initials from DB), "Hi, [name]!" in DM Serif Display,
  subtitle tagline
- Right: Search bar (rounded pill, white bg), notification bell icon, profile icon

MAIN CONTENT (2 columns: flex-1 + 340px right panel):

LEFT:
1. Learning Progress card (white, 14px radius):
   - Left: stats grid (Courses: N, Lessons: N/N, Challenges: N/N), My Bonuses pill badge
   - Right: animated SVG donut chart showing overall % complete,
     percentage in center (DM Serif Display 26px), "Completed" label
2. Featured Courses section (full width below):
   - Header row: "Featured Courses" title + "View all (N) →" pill button
   - 3-column course card grid. Each card:
     * Cover image area (140px tall, colored gradient bg + emoji placeholder)
     * "Most popular" dark pill badge OR in-progress bar overlay
     * Course title, short description, "Xh • Level" meta in orange

RIGHT PANEL:
- Weekly Tasks card: 4 tasks with status icon (green check / grey circle / locked),
  task name, progress %, segmented orange bar (8 dashes, fills proportionally)

PHP data:
- $user from session + users table
- $progress = aggregate from enrollments + lesson_completions
- $featured_courses = SELECT * FROM courses WHERE is_published=1 ORDER BY enrollment_count DESC LIMIT 3
- $weekly_tasks = predefined task list with dynamic completion checks
```

---

## 🔍 Page 4 — `courses.php` (Browse Courses)

**Route:** `/courses.php` · **Access:** Student

### Prompt

```
Build courses.php — course discovery page using BuildBox LMS theme.

LAYOUT: Same sidebar + topbar. Main area has:

TOP SECTION — Search & Filter bar (white card, full width):
- Large search input (placeholder: "Search courses, topics, teachers...")
  with magnifier icon, #D85A30 focus border
- Filter pills row below the input:
  * Category dropdown (Design, Development, Marketing, Photography...)
  * Level dropdown (Beginner, Intermediate, Advanced)
  * Duration dropdown (Under 5h, 5–20h, 20h+)
  * "Free / Paid" toggle pills
  * "Clear filters" text link (appears only when filters active, in orange)
- Active filters show as removable orange pills (X to dismiss)

RESULTS SECTION:
- Results count: "Showing 24 courses" (muted text, left) + Sort dropdown (right):
  "Most Popular | Newest | Highest Rated"
- Masonry-style 3-column grid of course cards (same card design as dashboard)
  Each card additionally shows: star rating (★ 4.8), enrollment count, teacher avatar+name
- Empty state: if no results, centered illustration (SVG magnifier) + "No courses found"
  message + "Clear filters" button

PAGINATION: Simple pill-style pagination row at bottom center.

PHP:
- Build dynamic WHERE clause from GET params: ?category=&level=&search=&sort=
- SELECT c.*, u.name as teacher_name, COUNT(e.id) as enrolled
  FROM courses c
  JOIN users u ON c.teacher_id = u.id
  LEFT JOIN enrollments e ON e.course_id = c.id
  WHERE c.is_published = 1 [+ dynamic filters]
  GROUP BY c.id
  ORDER BY [sort] LIMIT 12 OFFSET [page*12]
- JS: filter changes submit form via fetch() and re-render results without full reload
```

---

## 📖 Page 5 — `course-detail.php`

**Route:** `/course-detail.php?id=N` · **Access:** Student

### Prompt

```
Build course-detail.php — single course overview + enroll page using BuildBox LMS theme.

LAYOUT: Same sidebar + topbar. Main content is 2 columns (flex ~60% + 340px sticky sidebar).

LEFT COLUMN:
1. Course hero area (white card):
   - Large cover image/banner (300px tall, warm gradient fallback)
   - "Most Popular" or category badge overlay
   - Course title in DM Serif Display 28px
   - Subtitle / short description
   - Meta row: ★ Rating · N enrolled · Xh total · Level badge pill
   - Teacher row: avatar circle, name, specialization, "View profile →"

2. Tabs row (About | Curriculum | Reviews) — underline style active tab in #D85A30:

   ABOUT tab:
   - "What you'll learn" — grid of 2-col bullet points with orange checkmark icons
   - "Requirements" — bulleted list
   - Full course description (expandable "Read more" if >300 chars)

   CURRICULUM tab:
   - Accordion list of lectures. Each row:
     * Lecture number, title, type icon (video/PDF/link), duration
     * Lock icon if not enrolled, play icon if enrolled
   - Section headers if course has sections

   REVIEWS tab:
   - Average rating display (large number + star row + bar chart of 1-5 stars)
   - List of student review cards (avatar, name, date, star rating, comment text)

RIGHT STICKY SIDEBAR (white card, 340px):
- Course thumbnail
- Price (or "Free" badge)
- "Enroll Now" button — full width, #D85A30, 48px, DM Serif Display text
- If already enrolled: "Continue Learning →" green button
- "30-day money back guarantee" note (muted)
- Course includes list (icons): X lectures · X hours video · Certificate · Lifetime access

PHP:
- $course = SELECT * FROM courses WHERE id = ? AND is_published = 1
- $lectures = SELECT * FROM lectures WHERE course_id = ? ORDER BY order_index
- $is_enrolled = SELECT id FROM enrollments WHERE student_id=? AND course_id=?
- POST /enroll: INSERT INTO enrollments (student_id, course_id, enrolled_at) VALUES (...)
```

---

## 📚 Page 6 — `my-learning.php`

**Route:** `/my-learning.php` · **Access:** Student

### Prompt

```
Build my-learning.php — student's enrolled courses tracker using BuildBox LMS theme.

LAYOUT: Same sidebar + topbar.

TOP STAT ROW (3 metric cards in a row, same style as dashboard):
- Courses Enrolled (count)
- Lessons Completed (count)
- Certificates Earned (count)
Cards use white bg, muted label above, large number below in DM Serif Display.

FILTER TABS below stats (pill toggle row):
"All" | "In Progress" | "Completed" | "Not Started"
Active tab: #D85A30 bg, white text. Inactive: white bg, orange border.

COURSE LIST (vertical stack of horizontal cards):
Each enrolled course card (white, 14px radius, full width, flex row):
- Left: course thumbnail (120x80px, rounded 8px)
- Middle: course title, teacher name, "X of Y lessons completed"
  Below: segmented orange progress bar (full width of middle section)
- Right: "% Complete" large number in DM Serif Display + orange
  + "Continue" pill button (#D85A30)
  If 100%: green "Completed ✓" badge instead

PHP:
- SELECT c.*, e.enrolled_at,
    COUNT(lc.id) as completed_lessons,
    (SELECT COUNT(*) FROM lectures WHERE course_id=c.id) as total_lessons
  FROM enrollments e
  JOIN courses c ON e.course_id = c.id
  LEFT JOIN lesson_completions lc ON lc.enrollment_id = e.id
  WHERE e.student_id = ?
  GROUP BY c.id
- Filter via GET param ?status=in_progress|completed|not_started
```

---

## 🎬 Page 7 — `lesson.php`

**Route:** `/lesson.php?id=N` · **Access:** Enrolled Student only

### Prompt

```
Build lesson.php — the active lesson viewer using BuildBox LMS theme.

LAYOUT: This page uses a MODIFIED layout — no left sidebar. Instead:

LEFT PANEL (300px, dark #1A1915, full height):
- Back arrow + course title at top
- Course progress bar (thin orange line showing overall %)
- Scrollable lecture list accordion:
  Each lecture: number, title, duration, type icon
  Current: highlighted with orange left border + white text
  Completed: green checkmark icon
  Locked: grey lock icon

MAIN CONTENT AREA (flex-1, cream bg):
TOP BAR: Breadcrumb (Course → Lesson title), prev/next navigation arrows (right-aligned)

CONTENT CARD (white, 14px radius):
Renders lecture content based on type:
  - TYPE "video": Embed <video> tag or YouTube iframe (16:9, full width, rounded)
  - TYPE "pdf": <iframe> PDF viewer, 600px tall
  - TYPE "link": Styled link card with external arrow icon + description
  - TYPE "text": Rich text content rendered from DB (nl2br / HTML)

BELOW CONTENT:
- Lecture description / notes (collapsible)
- "Mark as Complete" button (full width, #D85A30) — POST to mark_complete.php
  If already complete: green "Completed ✓" button (disabled)
- Comments/Q&A section (simple threaded: student posts question, teacher replies)

PHP:
- Verify enrollment before serving content
- $lecture = SELECT * FROM lectures WHERE id = ?
- Mark complete: INSERT INTO lesson_completions (enrollment_id, lecture_id, completed_at)
  ON DUPLICATE KEY UPDATE completed_at = NOW()
- Auto-advance to next lecture on mark complete (JS redirect)
```

---

## 👨‍🏫 Page 8 — `teacher-dashboard.php`

**Route:** `/teacher-dashboard.php` · **Access:** Teacher (verified only)

### Prompt

```
Build teacher-dashboard.php using BuildBox LMS theme, teacher role sidebar.

TEACHER SIDEBAR (same dark sidebar, different nav items):
- Dashboard, My Courses, Upload Course, Students, Analytics
- Bottom: Profile, Settings, plan badge

If teacher is NOT verified (is_verified = 0):
Show a full-width warm amber banner at top:
  Icon: hourglass, Text: "Your profile is under review by admin. You'll be
  notified once approved.", background: #FAEEDA, text: #854F0B, border: #EF9F27

MAIN CONTENT (only visible if verified):

TOP STATS ROW (4 metric cards):
- Total Courses (count)
- Total Students Enrolled (count)
- Lessons Uploaded (count)
- Average Rating (★ X.X)

MY COURSES PREVIEW (white card):
"Recent Courses" title + "Manage all →" link
Horizontal scroll row of 3 course cards (mini version — image, title, enrolled count,
published/draft badge in green/grey pill)

RECENT ENROLLMENTS (white card, full width):
Table-style list: Student avatar+name | Course | Enrolled date | Progress %
5 rows, "View all students →" link at bottom

PHP:
- $stats = aggregate queries for teacher_id = $_SESSION['user_id']
- $courses = SELECT * FROM courses WHERE teacher_id = ? ORDER BY created_at DESC LIMIT 3
- $enrollments = recent 5 enrollments across teacher's courses
```

---

## ⬆️ Page 9 — `upload-course.php`

**Route:** `/upload-course.php` · **Access:** Verified Teacher

### Prompt

```
Build upload-course.php — multi-step course creation wizard using BuildBox LMS theme.

LAYOUT: Same teacher sidebar + topbar.

STEP INDICATOR at top of content (horizontal stepper):
3 steps shown as numbered circles connected by lines:
  1. Course Info  2. Curriculum  3. Publish
Active step: filled #D85A30 circle, white number. Completed: green checkmark.
Inactive: grey outlined circle.

STEP 1 — Course Info (white card):
- Course Title (text, required)
- Short Description (textarea, 2 rows, max 160 chars with live counter)
- Full Description (rich textarea, 5 rows)
- Category (select dropdown)
- Difficulty Level (pill toggle: Beginner | Intermediate | Advanced)
- Duration (number input + "hours" label)
- Price (number input, 0 = Free)
- Cover Image (styled file upload zone — dashed orange border, "Click or drag image here",
  preview thumbnail appears after selection)
"Next →" button (right-aligned, orange)

STEP 2 — Curriculum (white card):
- "Add Lecture" button (top right, outlined orange)
- Drag-and-drop sortable lecture list. Each lecture row (white card inside card):
  * Order handle (⠿ drag icon, grey)
  * Lecture title input (inline editable)
  * Type selector (pill toggle: Video | PDF | Link | Text)
  * Content field — changes based on type:
      Video: URL input
      PDF: file upload input
      Link: URL input + description input
      Text: textarea
  * Duration input (minutes)
  * Delete icon (red on hover)
- JS: add/remove/reorder lectures dynamically before submitting

STEP 3 — Publish (white card):
- Preview card showing the course as students will see it
- "Save as Draft" (outlined button) | "Publish Course" (filled orange button)

PHP:
- Step progression via $_SESSION['course_draft']
- Final submit: INSERT INTO courses (...) then bulk INSERT INTO lectures (...)
- File uploads stored to /uploads/course-materials/ with unique names
```

---

## 🛡️ Page 10 — `admin-dashboard.php`

**Route:** `/admin-dashboard.php` · **Access:** Admin only

### Prompt

```
Build admin-dashboard.php using BuildBox LMS theme, admin role sidebar.

ADMIN SIDEBAR nav items:
Dashboard, Teachers, Students, Courses, Reports, Settings

TOP STATS ROW (4 cards, same metric card style):
- Total Students
- Total Teachers (verified)
- Total Courses (published)
- Pending Approvals (count — shown with orange badge if > 0)

PENDING TEACHERS CARD (urgent — show first if pending > 0):
White card with orange left border (4px, #D85A30).
Title: "Pending Teacher Approvals (N)"
Table rows: Avatar+Name | Email | Specialization | Submitted date | [Approve] [Reject] buttons
Approve: green pill button. Reject: red outlined pill button.
PHP: POST to admin-action.php?action=approve&id=N or reject

RECENT ACTIVITY FEED (white card, right panel):
Vertical timeline of system events:
  - New student registered (blue dot)
  - Teacher approved (green dot)
  - Course published (orange dot)
  - Course flagged (red dot)
Each item: icon dot + text + timestamp (relative: "2 hours ago")

PLATFORM STATS CARD (white card, full width):
Simple bar chart (pure CSS or Chart.js) showing enrollments per month for last 6 months.
Orange bars on cream track background. X-axis: month labels. Y-axis: enrollment count.

PHP:
- All stats from aggregate queries
- Activity log from a system_log table (action, user_id, created_at)
- Chart data: SELECT MONTH(enrolled_at), COUNT(*) FROM enrollments
  GROUP BY MONTH(enrolled_at) ORDER BY MONTH
```

---

## ✅ Page 11 — `admin-teachers.php`

**Route:** `/admin-teachers.php` · **Access:** Admin

### Prompt

```
Build admin-teachers.php — teacher management page using BuildBox LMS theme.

LAYOUT: Admin sidebar + topbar.

TOP FILTER BAR (white card):
- Search input (by name or email)
- Status filter pills: All | Pending | Verified | Rejected
- "Export CSV" button (outlined, right side)

TEACHERS TABLE (white card):
Full-width table with subtle row hover (#F9F6F1):
Columns: Avatar+Name | Email | Specialization | Bio (truncated) | Courses |
         Joined | Status badge | Actions

Status badges:
- Pending: amber pill (#FAEEDA bg, #854F0B text)
- Verified: green pill (#E1F5EE bg, #0F6E56 text)
- Rejected: red pill (#FCEBEB bg, #A32D2D text)

Actions column (icon buttons):
- View profile (eye icon)
- Approve (checkmark, green — only if pending)
- Reject (x icon, red — only if pending/verified)
- Delete (trash, red — with confirmation modal)

CONFIRMATION MODAL (on delete):
Dark overlay. White centered card (400px wide):
  "Are you sure you want to remove [Teacher Name]?"
  Two buttons: "Cancel" (outlined) | "Delete" (red filled)

PHP:
- GET: SELECT * FROM users WHERE role='teacher' [+ filters]
- POST approve: UPDATE users SET is_verified=1 WHERE id=?
  Also: send_notification email (use mail() or flag for SMTP)
- POST reject: UPDATE users SET is_verified=-1 WHERE id=?
- POST delete: DELETE FROM users WHERE id=? (cascade to courses)
```

---

## 🗄️ Database Schema

```sql
-- Users (students, teachers, admin)
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(150) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  role ENUM('student','teacher','admin') DEFAULT 'student',
  is_verified TINYINT DEFAULT 1,  -- 0=pending, 1=approved, -1=rejected
  avatar VARCHAR(255),
  bio TEXT,
  specialization VARCHAR(150),
  portfolio_url VARCHAR(255),
  bonus_points INT DEFAULT 0,
  plan ENUM('rookie','pro','elite') DEFAULT 'rookie',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Courses
CREATE TABLE courses (
  id INT AUTO_INCREMENT PRIMARY KEY,
  teacher_id INT NOT NULL,
  title VARCHAR(200) NOT NULL,
  short_desc VARCHAR(300),
  description TEXT,
  category VARCHAR(80),
  level ENUM('Beginner','Intermediate','Advanced'),
  price DECIMAL(8,2) DEFAULT 0,
  duration_hours INT,
  cover_image VARCHAR(255),
  is_published TINYINT DEFAULT 0,
  enrollment_count INT DEFAULT 0,
  avg_rating DECIMAL(3,2) DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (teacher_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Lectures
CREATE TABLE lectures (
  id INT AUTO_INCREMENT PRIMARY KEY,
  course_id INT NOT NULL,
  title VARCHAR(200) NOT NULL,
  type ENUM('video','pdf','link','text') DEFAULT 'video',
  content TEXT,
  duration_minutes INT DEFAULT 0,
  order_index INT DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (course_id) REFERENCES courses(id) ON DELETE CASCADE
);

-- Enrollments
CREATE TABLE enrollments (
  id INT AUTO_INCREMENT PRIMARY KEY,
  student_id INT NOT NULL,
  course_id INT NOT NULL,
  enrolled_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE KEY unique_enrollment (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES users(id) ON DELETE CASCADE,
  FOREIGN KEY (course_id) REFERENCES courses(id) ON DELETE CASCADE
);

-- Lesson completions
CREATE TABLE lesson_completions (
  id INT AUTO_INCREMENT PRIMARY KEY,
  enrollment_id INT NOT NULL,
  lecture_id INT NOT NULL,
  completed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE KEY unique_completion (enrollment_id, lecture_id),
  FOREIGN KEY (enrollment_id) REFERENCES enrollments(id) ON DELETE CASCADE,
  FOREIGN KEY (lecture_id) REFERENCES lectures(id) ON DELETE CASCADE
);

-- Reviews
CREATE TABLE reviews (
  id INT AUTO_INCREMENT PRIMARY KEY,
  course_id INT NOT NULL,
  student_id INT NOT NULL,
  rating TINYINT NOT NULL CHECK (rating BETWEEN 1 AND 5),
  comment TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE KEY unique_review (course_id, student_id),
  FOREIGN KEY (course_id) REFERENCES courses(id) ON DELETE CASCADE,
  FOREIGN KEY (student_id) REFERENCES users(id) ON DELETE CASCADE
);

-- System log (admin activity feed)
CREATE TABLE system_log (
  id INT AUTO_INCREMENT PRIMARY KEY,
  action VARCHAR(100),
  user_id INT,
  details TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## 🛠️ PHP File Structure

```
/lms/
├── index.php              → redirect to login or dashboard
├── login.php
├── register.php
├── logout.php
├── dashboard.php
├── courses.php
├── course-detail.php
├── my-learning.php
├── lesson.php
├── profile.php
│
├── teacher/
│   ├── dashboard.php      → teacher-dashboard.php
│   ├── courses.php        → teacher-courses.php
│   └── upload.php         → upload-course.php
│
├── admin/
│   ├── dashboard.php      → admin-dashboard.php
│   ├── teachers.php       → admin-teachers.php
│   ├── courses.php        → admin-courses.php
│   └── students.php       → admin-students.php
│
├── includes/
│   ├── db.php             → PDO connection
│   ├── auth.php           → session check + role guard
│   ├── sidebar.php        → shared sidebar (role-aware)
│   └── topbar.php         → shared topbar
│
├── actions/
│   ├── enroll.php         → POST handler
│   ├── mark-complete.php  → POST handler
│   ├── admin-action.php   → approve/reject/delete
│   └── upload-handler.php → file upload logic
│
└── assets/
    ├── css/
    │   └── style.css      → global design system CSS variables + components
    ├── js/
    │   └── main.js        → shared JS (donut chart, animations, filter fetch)
    └── uploads/
        └── course-materials/
```

---

## 🎨 How to Use This README

1. **Copy any page's Prompt block** exactly as written
2. **Paste it** into Claude, v0, Bolt, Lovable, or Cursor
3. **Prefix with:** `"Using the BuildBox LMS design system defined in the Global Design System section above, build the following PHP page:"`
4. The AI will produce a page that visually matches `dashboard.html` exactly

> All prompts are written to produce **production-ready PHP** with real SQL queries, session handling, and role guards — not mockups.
# Major-Project-BCA-6th-Sem
