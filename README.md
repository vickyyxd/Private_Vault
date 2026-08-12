# 🔐 Secret Vault Engineering Calculator

> A futuristic engineering/scientific calculator with a hidden photo & video vault, built entirely with **HTML, CSS, and vanilla JavaScript**.

This project combines a fully functional multi-mode calculator with a discreet media vault. The calculator acts as the primary interface, while a secret PIN sequence can unlock a separate vault interface for temporarily viewing photos and videos.

The entire application is implemented as a **single HTML file**, with the HTML structure, CSS styling, animations, and JavaScript logic contained in the same document.

---

## ✨ Features

### 🧮 Advanced Calculator

The calculator provides three different operating modes:

* **STD** — Standard calculator
* **SCI** — Scientific calculator
* **ENG** — Engineering calculator

The active mode dynamically changes the available calculator buttons without requiring a page reload.

### Standard Mode

Includes:

* Addition `+`
* Subtraction `−`
* Multiplication `×`
* Division `÷`
* Decimal numbers
* Percentage `%`
* Positive/negative toggle `+/−`
* Clear All `AC`
* Clear Entry `CE`
* Result calculation `=`

Division by zero is handled by returning an `Error` state instead of producing an invalid result.

---

## 🔬 Scientific Mode

Scientific mode adds advanced mathematical functions.

### Trigonometry

* `sin`
* `cos`
* `tan`

With **SHIFT**, the buttons become:

* `sin⁻¹`
* `cos⁻¹`
* `tan⁻¹`

The calculator supports both **degrees (DEG)** and **radians (RAD)** for trigonometric calculations.

### Logarithmic Functions

* `log` — base-10 logarithm
* `ln` — natural logarithm
* `log₂` — base-2 logarithm

SHIFT provides inverse exponential operations:

* `10ˣ`
* `eˣ`

### Powers & Roots

* Square root `√x`
* Cube root `∛x`
* Power `xʸ`
* nth root through SHIFT + `xʸ`
* Square `x²` through SHIFT + `√`
* Reciprocal `1/x`
* Absolute value `|x|`

The power system supports operations such as:

```text
2 xʸ 8 =
256
```

and SHIFT can switch the power operation into an nth-root operation.

### Constants

* `π`
* `e`

The calculator uses JavaScript's built-in mathematical constants for these values.

### Other Scientific Functions

* Factorial `n!`
* Percentage `%`
* Sign change `+/−`
* Scientific notation `EE`

Factorial calculations are limited to values from `0` through `170` to avoid numerical overflow.

---

## ⚙️ Engineering Mode

Engineering mode combines scientific functions with calculator memory functionality.

### Memory Functions

| Button | Function                           |
| ------ | ---------------------------------- |
| `MC`   | Memory Clear                       |
| `MR`   | Memory Recall                      |
| `M+`   | Add current value to memory        |
| `M−`   | Subtract current value from memory |

A memory indicator appears in the calculator display when a value is stored.

Engineering mode also provides:

* `sin`
* `cos`
* `tan`
* `log`
* `ln`
* `√x`
* `∛x`
* `π`
* `e`
* `xʸ`
* `1/x`
* `%`
* `n!`
* `log₂`
* `EE`

---

# 🔢 Secret Vault

The most distinctive feature of the project is the hidden media vault.

The visible application looks like an engineering calculator, while the vault interface remains hidden until the correct PIN sequence is entered.

## 🔑 Unlocking the Vault

The default secret PIN is:

```text
1234=
```

To unlock:

1. Open the calculator.
2. Enter:

```text
1 → 2 → 3 → 4 → =
```

3. The calculator recognizes the sequence.
4. The calculator interface is replaced by the vault interface.

The PIN is detected by maintaining a small input buffer and checking whether it ends with the configured `SECRET_PIN`.

---

## 🔧 Changing the PIN

Open the HTML file and find:

```javascript
const SECRET_PIN = '1234=';
```

Change it to your preferred sequence.

For example:

```javascript
const SECRET_PIN = '9876=';
```

You would then unlock the vault with:

```text
9 → 8 → 7 → 6 → =
```

The PIN configuration is intentionally located near the beginning of the JavaScript section for easy modification.

> **Security note:** This is a client-side JavaScript PIN, not a cryptographically secure authentication system. Anyone with access to the source code can inspect or change it.

---

# 🖼️ Photo Vault

After unlocking the vault, users can switch to the **PHOTOS** section.

Features include:

* Select multiple images
* Upload images from the device
* Generate preview cards
* Display filenames
* Show photo count
* Open photos in a full-screen viewer
* Delete individual photos
* Empty-state display when no photos exist

The file input accepts:

```html
<input type="file" accept="image/*" multiple>
```

---

# 🎬 Video Vault

The vault also contains a dedicated **VIDEOS** section.

Features include:

* Select multiple videos
* Video thumbnails/previews
* Video count
* Filename display
* `VID` badge
* Full-screen video playback
* Delete videos individually

The implementation uses:

```html
<input type="file" accept="video/*" multiple>
```

---

# 🗂️ How Media Storage Works

This application does **not** upload media to a server.

When a user selects a photo or video, JavaScript creates a temporary browser Blob URL:

```javascript
URL.createObjectURL(file)
```

The application stores the generated URL and filename in JavaScript arrays:

```javascript
const photoItems = [];
const videoItems = [];
```

The files are therefore handled through temporary browser memory rather than being stored by an application backend.

### Important limitation

The vault is **session-based**.

The project does not implement:

* Database storage
* Cloud storage
* Server-side uploads
* Persistent media storage
* User accounts
* Encryption

Closing/reloading the page will not provide persistent vault storage.

> **Privacy clarification:** The application itself does not save the selected media to its own server or persistent database. However, this should **not** be interpreted as a guarantee of "zero traces" on every browser or operating system.

---

# 🔍 Media Grid

Uploaded files are displayed using a responsive CSS Grid.

The grid automatically determines how many media cards can fit on the screen:

```css
grid-template-columns: repeat(
  auto-fill,
  minmax(150px, 1fr)
);
```

Each media card includes:

* Preview
* Filename
* Delete button
* Hover effects
* Click-to-open behavior

---

# 🖥️ Full-Screen Lightbox

Clicking a media card opens a full-screen viewer.

### Photos

Photos are displayed in a large centered viewer.

### Videos

Videos open with:

* Native controls
* Automatic playback
* Large responsive display

The lightbox can be closed using:

* `✕` button
* Clicking outside the media

---

# ⚡ Panic Mode

The vault includes a **PANIC** button designed to immediately hide the vault.

When activated:

1. The panic overlay appears.
2. The vault is locked.
3. The calculator state is reset.
4. A fake calculator loading screen is displayed.
5. The overlay disappears after approximately 2 seconds.

The displayed message is:

```text
🔢
LOADING CALCULATOR...
```

The panic behavior is implemented entirely on the client side.

---

# 🔒 Lock Button

The **LOCK** button provides a normal way to leave the vault.

It:

* Closes the vault
* Returns to the calculator
* Resets the calculator display
* Clears the PIN input buffer

---

# ⌨️ Keyboard Support

The calculator supports keyboard input.

Supported keyboard mappings include:

| Keyboard Key | Calculator Action |
| ------------ | ----------------- |
| `Enter`      | `=`               |
| `=`          | `=`               |
| `Backspace`  | `CE`              |
| `/`          | Division          |
| `*`          | Multiplication    |
| `-`          | Subtraction       |
| `0–9`        | Number input      |
| `.`          | Decimal           |
| `%`          | Percentage        |
| `+`          | Addition          |

Keyboard input is disabled while the vault is open.

---

# 🎨 UI & Design

The application uses a futuristic dark engineering/terminal-inspired design.

### Visual characteristics

* Dark navy/black background
* Electric cyan highlights
* Amber SHIFT indicator
* Green memory controls
* Red error/clear/delete controls
* Purple mathematical constants
* Glowing calculator display
* Animated grid background
* CRT-style scanning animation
* Button ripple effects
* Hover animations
* Responsive media grid
* Custom scrollbar
* macOS-inspired calculator window header

The project defines its major colors through CSS custom properties, making the theme easy to customize.

---

# 🔤 Fonts

The project uses two Google Fonts:

### Orbitron

Used primarily for:

* Calculator display
* Titles
* Technical branding

### JetBrains Mono

Used primarily for:

* Buttons
* Labels
* Interface text
* Technical UI elements

> Internet access is required for the Google Fonts to load from Google's CDN. The application itself does not require a backend server.

---

# ✨ Animations & Interactions

The interface contains several custom animations.

### Animated Background

A cyan engineering-style grid continuously moves diagonally.

### Display Scan Line

A glowing horizontal line periodically sweeps across the calculator display.

### Button Ripple

Clicking a calculator button creates an expanding circular ripple effect.

### Hover Effects

Buttons and media cards respond to pointer interaction with:

* Border changes
* Glow effects
* Color transitions
* Slight movement

### Vault Transition

The vault fades into view when unlocked and fades away when locked.

### Toast Notifications

Short messages appear at the bottom of the interface after actions such as:

```text
🔓 Vault unlocked
📥 2 photo(s) added
🗑️ Removed
Memory cleared
```

The toast notification automatically disappears after approximately 2.2 seconds.

---

# 🧠 How the Application Works

The project is divided conceptually into two main applications:

```text
┌──────────────────────────────────────┐
│        ENGINEERING CALCULATOR        │
│                                      │
│  STD   SCI   ENG                     │
│                                      │
│  Numbers / Operators / Functions     │
│                                      │
│       Secret PIN Detection           │
└──────────────────┬───────────────────┘
                   │
             Correct PIN
                   │
                   ▼
┌──────────────────────────────────────┐
│             SECRET VAULT             │
│                                      │
│   PHOTOS          VIDEOS              │
│                                      │
│   Upload          Upload              │
│   Preview         Preview             │
│   Delete          Delete              │
│   Lightbox        Lightbox            │
│                                      │
│       LOCK          PANIC             │
└──────────────────────────────────────┘
```

---

# 🔄 Calculator Execution Flow

Every calculator button contains a `data-fn` attribute.

Example:

```html
<button class="btn num" data-fn="7">7</button>
```

When clicked, JavaScript reads the function:

```javascript
handleFn(btn.dataset.fn);
```

The central `handleFn()` function then determines what action should occur.

Its basic flow is:

```text
Button / Keyboard Input
        ↓
   handleFn()
        ↓
 PIN detection?
        ↓
Calculator function
        ↓
Update state
        ↓
Update display
```

---

# 🧮 Calculator State

The calculator maintains several JavaScript state variables:

```javascript
let current = '0';
let prev = '';
let op = '';
let justEvaled = false;
let angleMode = 'DEG';
let shifted = false;
let memory = 0;
let hasMemory = false;
let pinBuf = '';
let currentMode = 'std';
```

These variables control:

* Current display value
* Previous operand
* Current operator
* Evaluation state
* Degree/radian mode
* SHIFT mode
* Calculator memory
* PIN detection
* Active calculator mode

---

# ➗ Mathematical Evaluation

Binary operations are handled by:

```javascript
doOp(a, b, operator)
```

Supported operations include:

```text
+
−
×
÷
^
nth root
```

The `evaluate()` function takes the previous operand, operator, and current operand and calculates the result.

Example:

```text
3 + 4 =
```

Internally:

```text
a = 3
b = 4
operator = +
result = 7
```

The previous expression is also displayed in the calculator history area.

---

# 📁 Project Structure

The current project is intentionally lightweight and can be kept as a single file:

```text
secret-vault-calculator/
│
├── index.html
└── README.md
```

The `index.html` contains:

```text
HTML
├── Calculator UI
├── Vault UI
├── Lightbox
├── Panic Overlay
└── Toast Notifications

CSS
├── Theme
├── Calculator layout
├── Buttons
├── Animations
├── Vault layout
├── Media grid
└── Responsive styling

JavaScript
├── Calculator state
├── Mathematical operations
├── Scientific functions
├── Memory
├── PIN detection
├── Mode switching
├── Keyboard support
├── Vault
├── Media upload
├── Media deletion
├── Lightbox
├── Panic mode
└── Toast notifications
```

The source itself identifies the five primary UI sections as the calculator, vault, lightbox, panic overlay, and toast system.

---

# 🚀 How to Run

No Node.js, Python, PHP, database, or backend is required.

## Method 1 — Open Directly

1. Clone or download the repository.
2. Open `index.html`.
3. The calculator will run directly in your browser.

```bash
git clone https://github.com/YOUR-USERNAME/secret-vault-calculator.git
cd secret-vault-calculator
```

Then open:

```text
index.html
```

in your browser.

---

## Method 2 — VS Code Live Server

For a better development experience:

1. Open the project in VS Code.
2. Install the **Live Server** extension.
3. Right-click `index.html`.
4. Select **Open with Live Server**.

The application will open in your browser.

---

## Method 3 — Local HTTP Server

You can also use Python's built-in server:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

---

# 🌐 Deploy to GitHub Pages

Because this is a static HTML/CSS/JavaScript application, it can be hosted using GitHub Pages.

### 1. Create a repository

Create a GitHub repository such as:

```text
secret-vault-calculator
```

### 2. Add files

```text
index.html
README.md
```

### 3. Push the project

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/secret-vault-calculator.git
git push -u origin main
```

### 4. Enable GitHub Pages

Go to:

```text
Repository
→ Settings
→ Pages
→ Deploy from branch
→ main
→ / (root)
→ Save
```

GitHub will generate a public website URL for the project.

---

# 🛠️ Customization

## Change the PIN

Find:

```javascript
const SECRET_PIN = '1234=';
```

Change it to your desired sequence.

---

## Change the Theme

The primary colors are controlled through CSS variables:

```css
:root {
  --bg: #060a0f;
  --panel: #0b1118;
  --surface: #0f1923;
  --surface2: #162130;

  --cyan: #00e5ff;
  --cyan2: #00b8cc;
  --amber: #ffb300;
  --green: #00e676;
  --red: #ff4444;
  --purple: #bb86fc;
}
```

You can change these values to create your own theme.

---

# 🔐 Security & Privacy

This section is important.

Although the application is designed as a discreet client-side vault, it should **not be considered a secure encrypted vault**.

### What the project does

* Uses a client-side PIN.
* Keeps uploaded media in JavaScript arrays.
* Uses temporary `Blob` URLs.
* Does not send uploaded files to a project backend.
* Does not use a database.
* Does not implement user accounts.
* Does not implement server-side authentication.
* Does not implement encryption.

### What the project does NOT protect against

Someone who can inspect the source code can find:

```javascript
const SECRET_PIN = '1234=';
```

Therefore, the PIN should be considered an **interface unlock mechanism**, not strong security.

The media handling uses temporary object URLs created from selected files rather than persistent application storage.

For genuinely sensitive files, use a properly designed encrypted storage system rather than relying on this project as a security boundary.

---

# 📱 Browser Compatibility

The application relies on standard modern browser APIs including:

* JavaScript
* File API
* `URL.createObjectURL()`
* CSS Grid
* CSS animations
* HTML5 video
* DOM APIs

A modern version of browsers such as:

* Chrome
* Microsoft Edge
* Firefox
* Safari

should be used for the best experience.

---

# 🧩 Technologies Used

| Technology            | Purpose                                     |
| --------------------- | ------------------------------------------- |
| **HTML5**             | Application structure                       |
| **CSS3**              | Layout, theme, animations and responsive UI |
| **JavaScript (ES6+)** | Calculator and vault functionality          |
| **CSS Grid**          | Media gallery layout                        |
| **DOM API**           | Dynamic interface updates                   |
| **File API**          | Local file selection                        |
| **Blob URLs**         | Temporary media previews                    |
| **HTML5 Video**       | Video playback                              |
| **Google Fonts**      | Orbitron & JetBrains Mono                   |

The project intentionally avoids frameworks and external JavaScript libraries. The core application is built using HTML, CSS, and JavaScript.

---

# 📚 Learning Concepts Demonstrated

This project is also useful as a front-end JavaScript learning project.

It demonstrates:

* DOM manipulation
* Event listeners
* JavaScript state management
* Functions
* Conditional logic
* Arrays
* Template literals
* CSS variables
* CSS Grid
* CSS animations
* Dynamic HTML generation
* File input handling
* Blob URLs
* Keyboard event handling
* Mathematical JavaScript APIs
* Responsive UI design
* Modal/lightbox interfaces
* Client-side application architecture

---

# 🔮 Possible Future Improvements

The current implementation is intentionally client-side and lightweight. Potential future improvements include:

### 🔐 Security

* Password hashing
* Web Crypto API encryption
* Encrypted local storage
* Biometric authentication where supported
* Automatic vault timeout
* Failed-attempt protection

### 💾 Storage

* IndexedDB
* Encrypted persistent storage
* File metadata management
* Folder/category organization

### 🧮 Calculator

* Calculation history panel
* More engineering constants
* Complex numbers
* Matrices
* Unit conversion
* Binary/octal/hexadecimal modes
* More advanced equation parsing

### 📱 User Experience

* Improved mobile layout
* Touch gestures
* Custom themes
* Dark/light themes
* More keyboard shortcuts
* Drag-and-drop media upload
* Media search and sorting

### 🗃️ Vault

* Drag-and-drop uploads
* Media filtering
* File size display
* Image zoom
* Video thumbnails
* Multiple-selection delete
* Automatic vault locking

---

# ⚠️ Limitations

The current project has several intentional limitations:

1. **The PIN is stored directly in JavaScript.**
2. **There is no real authentication system.**
3. **There is no encryption.**
4. **Media is handled through temporary browser Blob URLs.**
5. **There is no persistent database.**
6. **Media is not uploaded to a server.**
7. **Refreshing or closing the page does not provide persistent vault storage.**
8. **The project is primarily a client-side demonstration/application.**

These limitations are important if the project is used for anything involving genuinely sensitive information.

---

# 🎯 Project Goals

The project was designed to demonstrate how a single-page browser application can combine:

```text
Functional Calculator
        +
Scientific Functions
        +
Engineering Functions
        +
Client-Side State
        +
File Handling
        +
Media Viewer
        +
Dynamic UI
        +
Animations
        +
Hidden Interface
```

The result is a compact front-end project that demonstrates a wide range of HTML, CSS, and JavaScript concepts in one application.

---

# 👨‍💻 Development

### Clone

```bash
git clone https://github.com/YOUR-USERNAME/secret-vault-calculator.git
```

### Enter directory

```bash
cd secret-vault-calculator
```

### Run

Open `index.html` directly or use:

```bash
python3 -m http.server 8000
```

### Edit

The main application logic is contained inside:

```text
index.html
```

Search for:

```javascript
SECRET_PIN
```

to change the vault PIN.

---

# 🤝 Contributing

Contributions are welcome.

A typical contribution workflow:

```bash
git checkout -b feature/my-feature
```

Make your changes, then:

```bash
git add .
git commit -m "Add my feature"
git push origin feature/my-feature
```

Open a Pull Request on GitHub.

---

# 📄 License

No license is explicitly defined in the supplied project source.

If you plan to publish this repository publicly, add an appropriate license such as MIT before distributing the project.

---

# ⭐ If You Like This Project

If you find the project useful or interesting:

* ⭐ Star the repository
* 🍴 Fork the project
* 🐛 Report bugs
* 💡 Suggest features
* 🔧 Submit pull requests

---

## 🔥 Project Summary

**Secret Vault Engineering Calculator** is a single-file front-end application that looks and behaves like a futuristic engineering calculator while containing a hidden client-side media vault.

It combines:

**HTML + CSS + JavaScript + Calculator Engine + Scientific Functions + Engineering Functions + Memory + Secret PIN + Photo Vault + Video Vault + Lightbox + Panic Mode + Keyboard Support + Animations**

—all without a backend or database.
Vicky
