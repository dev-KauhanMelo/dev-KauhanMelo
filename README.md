<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0D1117,50:1F6FEB,100:58A6FF&height=180&section=header&text=Kauhan%20Melo&fontSize=52&fontColor=FFFFFF&animation=fadeIn&fontAlignY=36&desc=Software%20Developer%20%C2%B7%20Recife%2C%20Brazil&descAlignY=56&descSize=16" width="100%"/>

<a href="https://www.linkedin.com/in/kauhan-rodrigues-6756a4389/">
  <img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"/>
</a>
<a href="mailto:kauhandev.gpt@gmail.com">
  <img src="https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"/>
</a>

</div>

---

I build software for the school I attend.

That sentence covers most of what's in this profile. I'm a high school student at **ETE Porto Digital**, a public technical school in Recife, Brazil, and nearly every repository here started with someone at that school doing something slowly by hand: a secretary typing hundreds of student emails one at a time, class representatives filling out paper attendance sheets, a teacher who couldn't run a programming exam on lab computers because nothing stopped students from opening another tab.

Some of these systems are in production. Some are honest MVPs with the gaps written down. I try to be clear about which is which.

I also tutor **mathematics** at my school, and I was selected for **Programa Ganhe o Mundo 2026**, a state exchange program, with the United Kingdom as my placement.

---

## Selected work

### [NAEE](https://github.com/dev-KauhanMelo/Sistema-NAEE-Nucleo-de-Avalia-o-e-Execucao-Escolar) · locked-down exam platform for programming tests

An online judge with proctoring, built as a monorepo of three parts that talk to each other. Students solve Python problems inside a kiosk-mode Electron client with a Monaco editor and no access to the rest of the machine. Teachers watch the whole lab from a live web panel: which station is logged in, which question each student is on, how many focus-loss strikes they've accumulated, colour-coded so it reads from across the room. Escalation is progressive rather than binary, warning, then persistent alert, then full screen lock, with a teacher-side unlock button. Submitted code runs against test cases in a self-hosted Judge0 instance.

The exam questions are built on open data from the Recife city government: community solar electrification, delivery routing through the Porto Digital district, water quality monitoring on the Capibaribe river.

**Stack:** Electron · React · TypeScript · Vite · Monaco · Node · Express · Firebase Realtime Database · Judge0 · Docker · Zod · npm workspaces

The part I'd point at first is the *Limitations* section of its README. It says, in writing, that the kiosk lockdown is UX deterrence and not an operating system security boundary, that Task Manager can always kill the process, that the teacher unlock doesn't yet reach the student's screen, and that there is no automated test coverage. Writing that down cost me nothing to hide and made the project honest.

### [Interclasse ETEPD](https://github.com/dev-KauhanMelo/etepd-sistema-interclasse) · live platform for a school sports tournament

Public site with live scores, schedules, standings and bracket visualisation, plus an admin area where judges update matches in real time during the event. Built and shipped for an actual tournament, not a mockup.

The interesting problem was the cheer button. I wanted anyone, unauthenticated, to be able to cheer for a team, without letting anyone rewrite a score. The Firestore security rule allows unauthenticated writes to match documents only if the write touches exactly the two cheer counter fields and increases each by at most one. Everything else on the document stays admin-only. Solving that at the security-rule layer instead of trusting the client was the right call and I'm still pleased with it.

**Stack:** React · Vite · Tailwind · Firebase (Firestore, Auth, Hosting)

### [Ata Digital](https://github.com/dev-KauhanMelo/sistema-ata-etepd) · digital attendance for class representatives

Replaces the paper attendance book. Two representatives per class mark attendance from their phones, with searchable history and auditable corrections.

Most of the work here was in the business rules rather than the code. Attendance is recorded per day, not per lesson. Every tap saves immediately, so a dropped connection or a handover between the two representatives resumes exactly where it left off. After finalising, there's a twenty-minute free edit window before the session locks. Late arrivals and early departures stay editable all day regardless of the lock, always timestamped and attributed. The calendar view is derived, never destructive: no attendance record is ever deleted.

Persistence is local-first behind a services layer, with a Prisma schema already written for the eventual Postgres migration. Deliberate: the school needed something working now, and the screens don't care where the data lives.

**Stack:** React · Vite · Tailwind · Prisma schema · local-first services layer

### An iterative method for the area of a circular segment

I found a convergent iterative construction for the area of a circular segment while working a classroom problem, then went backwards to figure out what I'd actually found. It turned out to be a specific case of Archimedes' method of exhaustion. I formalised the derivation, worked through the convergence, and validated it numerically in GeoGebra and C++ against the closed form, with mentoring from one of my teachers.

Finding out that a mathematician got there 2,200 years earlier was, honestly, the best part.

### [Jarvis Tetraedro](https://github.com/dev-KauhanMelo/jarvis-geometria-3d) · augmented reality solid geometry

A regular tetrahedron rendered over live camera video and manipulated with bare hands. A closed fist grabs the solid and locks it to your hand, so it translates, rotates with your palm and moves in depth as you approach the camera. A thumb-and-index pinch on a vertex pulls it and deforms the solid. Two closed fists resize it. Area, volume and height recompute in real time, including after the solid stops being regular.

Two decisions that mattered. Moving hand detection to its own thread multiplied render fluidity by roughly seven. And the interaction model is *grab on purpose*: with an open hand nothing moves, so you can rest your hand in frame without disturbing the scene, and nothing ever springs back to centre on its own.

351 tests, none of which require a camera or a GPU. They cover the geometry formulas, gesture computation from synthetic landmarks, smoothing, dead zones and camera reconnection against a fake capture device. Every hardware dependency degrades gracefully instead of crashing: no MediaPipe model falls back to mouse control, no network camera falls back to the local one.

**Stack:** Python · MediaPipe · PyOpenGL · Pygame · NumPy · Pytest

### [Campo Minado ETEPD](https://github.com/dev-KauhanMelo/campo-minado-ETEPD) · Minesweeper, taken more seriously than it needed to be

Board state lives on a FastAPI backend rather than in the browser, so the client can't cheat by reading its own mine layout. Mines aren't placed at board construction, they're drawn on the first reveal, excluding the clicked cell and its neighbours, which guarantees the first click is never a loss and usually opens a cascade. When the board is too small to honour that safe zone, it degrades to protecting just the clicked cell instead of failing.

Rankings persist to Firebase. React 19 front end with difficulty modes, pause, podium and confetti.

**Stack:** FastAPI · Python · React 19 · Vite · Tailwind · Firebase

### [PID line follower](https://github.com/dev-KauhanMelo/seguidor-de-linha-pid-QTR) · Arduino firmware

Two hundred lines of C++ where most of the thinking is in the details. The derivative term is low-pass filtered because raw derivatives are noise amplifiers at speed. The integral is clamped for anti-windup. Loop timing uses real elapsed microseconds with overflow protection rather than assuming a fixed period. Base speed decreases linearly with error, so the robot brakes going into curves instead of overshooting them. The slew rate limiter is deliberately asymmetric, allowing faster deceleration than acceleration.

Sensor calibration is written to EEPROM behind a magic byte, and the start button distinguishes a quick click from a two-second hold: click to reuse the saved calibration, hold to recalibrate. That saves setup time on every competition run.

The comments record that the current gains were tuned for one specific speed range and need revalidation if that range changes. Gains that work at one speed are not gains, they're a coincidence.

**Stack:** C++ · Arduino · QTR-8A sensor array · L298N driver · EEPROM

### [AutoEmail ETEPD](https://github.com/dev-KauhanMelo/School-Email-Generator) · in production

An ETL pipeline that reads enrollment spreadsheets, normalises names, strips accents and special characters, generates standardised institutional emails and persists everything to SQLite with uniqueness guarantees.

This one actually shipped. It generated the accounts for every incoming class of 2026 at my school. The task used to consume entire afternoons of manual typing across hundreds of students and was constantly vulnerable to typos and duplicates. It now takes seconds. The `.gitignore` keeps the real database and the real spreadsheets out of version control, because the input is student personal data.

**Stack:** Python · Pandas · SQLite · OpenPyXL

### [Diversamente](https://github.com/dev-KauhanMelo/site-diversamente) · accessible teaching materials platform

A Flask application where teachers publish adapted teaching materials indexed by disability type, and students find materials matched to their needs. Separate authentication for both roles, password hashing through Werkzeug, SQLAlchemy models with cascade deletes, image upload with extension validation, full profile management including account deletion.

---

## How I work

**I write down what doesn't work yet.** Every serious repository here has a limitations section. Not because the projects are weak, but because a system whose failure modes are documented is one someone else can actually adopt.

**I design for the swap.** The attendance system is local-first behind a services layer, with the Postgres schema already written. The exam platform's submit endpoint returns 501 on purpose until Judge0 is real, rather than pretending.

**I put the constraint where it can't be bypassed.** Anonymous cheer votes are bounded by a database security rule, not by the client. The minesweeper board lives on the server.

**I degrade instead of crashing.** No camera, no model file, no network, empty database: everything has a documented fallback path.

---

## Tools

<div align="center">

<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" height="40" alt="Python"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" height="40" alt="TypeScript"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" height="40" alt="JavaScript"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/cplusplus/cplusplus-original.svg" height="40" alt="C++"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" height="40" alt="React"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" height="40" alt="Node.js"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/express/express-original.svg" height="40" alt="Express"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/fastapi/fastapi-original.svg" height="40" alt="FastAPI"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/flask/flask-original.svg" height="40" alt="Flask"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/electron/electron-original.svg" height="40" alt="Electron"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/tailwindcss/tailwindcss-original.svg" height="40" alt="Tailwind"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/opencv/opencv-original.svg" height="40" alt="OpenCV"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/numpy/numpy-original.svg" height="40" alt="NumPy"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/pytest/pytest-original.svg" height="40" alt="Pytest"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/postgresql/postgresql-original.svg" height="40" alt="PostgreSQL"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/sqlite/sqlite-original.svg" height="40" alt="SQLite"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/prisma/prisma-original.svg" height="40" alt="Prisma"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/firebase/firebase-plain.svg" height="40" alt="Firebase"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original.svg" height="40" alt="Docker"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/git/git-original.svg" height="40" alt="Git"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/linux/linux-original.svg" height="40" alt="Linux"/>&nbsp;&nbsp;
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/arduino/arduino-original.svg" height="40" alt="Arduino"/>

</div>

---

## Currently

- Closing the NAEE MVP: real Judge0 execution, strike persistence so the teacher's unlock actually reaches the student's screen, and teacher authentication.
- Writing up the circular segment method with the full derivation.
- Preparing for my exchange year in the United Kingdom.
- Reading about how computer vision systems fail, which is more interesting than how they work.

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:58A6FF,50:1F6FEB,100:0D1117&height=110&section=footer" width="100%"/>

</div>
