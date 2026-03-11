# React Accessibility Patterns

## Modal / Dialog with Focus Trap

```jsx
import { useEffect, useRef } from "react";

function Modal({ isOpen, onClose, title, children }) {
  const dialogRef = useRef(null);
  const previousFocusRef = useRef(null);

  useEffect(() => {
    if (isOpen) {
      previousFocusRef.current = document.activeElement;
      dialogRef.current?.focus();
    } else {
      previousFocusRef.current?.focus();
    }
  }, [isOpen]);

  useEffect(() => {
    if (!isOpen) return;
    const handleKeyDown = (e) => {
      if (e.key === "Escape") onClose();
      if (e.key === "Tab") trapFocus(e, dialogRef.current);
    };
    document.addEventListener("keydown", handleKeyDown);
    return () => document.removeEventListener("keydown", handleKeyDown);
  }, [isOpen, onClose]);

  if (!isOpen) return null;

  return (
    <div role="presentation" style={{ position: "fixed", inset: 0, background: "rgba(0,0,0,0.5)" }}>
      <div
        role="dialog"
        aria-modal="true"
        aria-labelledby="modal-title"
        ref={dialogRef}
        tabIndex={-1}
        style={{ background: "#fff", padding: "2rem", borderRadius: "8px", maxWidth: "500px", margin: "10vh auto" }}
      >
        <h2 id="modal-title">{title}</h2>
        {children}
        <button type="button" onClick={onClose}>Close</button>
      </div>
    </div>
  );
}

function trapFocus(e, container) {
  const focusable = container.querySelectorAll(
    'a[href], button:not([disabled]), textarea, input, select, [tabindex]:not([tabindex="-1"])'
  );
  const first = focusable[0];
  const last = focusable[focusable.length - 1];
  if (e.shiftKey ? document.activeElement === first : document.activeElement === last) {
    e.preventDefault();
    (e.shiftKey ? last : first).focus();
  }
}
```

## Accessible Tab Component

```jsx
import { useState, useRef } from "react";

function Tabs({ tabs }) {
  const [activeIndex, setActiveIndex] = useState(0);
  const tabRefs = useRef([]);

  const handleKeyDown = (e, index) => {
    let next = index;
    if (e.key === "ArrowRight") next = (index + 1) % tabs.length;
    if (e.key === "ArrowLeft") next = (index - 1 + tabs.length) % tabs.length;
    if (e.key === "Home") next = 0;
    if (e.key === "End") next = tabs.length - 1;
    if (next !== index) {
      e.preventDefault();
      setActiveIndex(next);
      tabRefs.current[next]?.focus();
    }
  };

  return (
    <div>
      <div role="tablist" aria-label="Content sections">
        {tabs.map((tab, i) => (
          <button
            key={tab.id}
            role="tab"
            id={`tab-${tab.id}`}
            aria-selected={i === activeIndex}
            aria-controls={`panel-${tab.id}`}
            tabIndex={i === activeIndex ? 0 : -1}
            ref={(el) => (tabRefs.current[i] = el)}
            onClick={() => setActiveIndex(i)}
            onKeyDown={(e) => handleKeyDown(e, i)}
          >
            {tab.label}
          </button>
        ))}
      </div>

      {tabs.map((tab, i) => (
        <div
          key={tab.id}
          role="tabpanel"
          id={`panel-${tab.id}`}
          aria-labelledby={`tab-${tab.id}`}
          hidden={i !== activeIndex}
          tabIndex={0}
        >
          {tab.content}
        </div>
      ))}
    </div>
  );
}
```

## Accessible Dropdown Menu

```jsx
import { useState, useRef, useEffect } from "react";

function DropdownMenu({ label, items }) {
  const [open, setOpen] = useState(false);
  const [activeIndex, setActiveIndex] = useState(-1);
  const buttonRef = useRef(null);
  const menuRef = useRef(null);

  useEffect(() => {
    if (!open) { setActiveIndex(-1); return; }
    menuRef.current?.querySelector('[role="menuitem"]')?.focus();
  }, [open]);

  const handleButtonKeyDown = (e) => {
    if (["Enter", " ", "ArrowDown"].includes(e.key)) { e.preventDefault(); setOpen(true); }
    if (e.key === "ArrowUp") { e.preventDefault(); setOpen(true); setActiveIndex(items.length - 1); }
  };

  const handleMenuKeyDown = (e) => {
    const items_els = [...menuRef.current.querySelectorAll('[role="menuitem"]')];
    let next = activeIndex;
    if (e.key === "ArrowDown") next = (activeIndex + 1) % items_els.length;
    if (e.key === "ArrowUp") next = (activeIndex - 1 + items_els.length) % items_els.length;
    if (e.key === "Home") next = 0;
    if (e.key === "End") next = items_els.length - 1;
    if (e.key === "Escape") { setOpen(false); buttonRef.current?.focus(); return; }
    if (e.key === "Tab") { setOpen(false); return; }
    if (next !== activeIndex) { e.preventDefault(); setActiveIndex(next); items_els[next]?.focus(); }
  };

  return (
    <div style={{ position: "relative", display: "inline-block" }}>
      <button
        ref={buttonRef}
        type="button"
        aria-haspopup="menu"
        aria-expanded={open}
        onClick={() => setOpen((o) => !o)}
        onKeyDown={handleButtonKeyDown}
      >
        {label}
      </button>
      {open && (
        <ul
          role="menu"
          ref={menuRef}
          onKeyDown={handleMenuKeyDown}
          style={{ position: "absolute", listStyle: "none", padding: 0, margin: 0, background: "#fff", border: "1px solid #ccc" }}
        >
          {items.map((item, i) => (
            <li key={item.id} role="none">
              <button
                role="menuitem"
                tabIndex={-1}
                onClick={() => { item.onClick(); setOpen(false); buttonRef.current?.focus(); }}
              >
                {item.label}
              </button>
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

## Live Region / Status Announcer Hook

```jsx
import { useEffect, useRef } from "react";

// Reusable hook for screen reader announcements
function useAnnouncer() {
  const ref = useRef(null);
  const announce = (message, priority = "polite") => {
    if (!ref.current) return;
    ref.current.setAttribute("aria-live", priority);
    ref.current.textContent = "";
    // Timeout forces screen readers to re-announce identical messages
    setTimeout(() => { ref.current.textContent = message; }, 50);
  };
  return { ref, announce };
}

// Usage in a component
function SearchResults({ results, loading }) {
  const { ref, announce } = useAnnouncer();

  useEffect(() => {
    if (loading) announce("Loading results...");
    else announce(`${results.length} results found`);
  }, [loading, results.length]);

  return (
    <>
      {/* Visually hidden live region */}
      <div ref={ref} className="sr-only" aria-live="polite" aria-atomic="true" />
      {/* Visual results */}
      {results.map((r) => <div key={r.id}>{r.title}</div>)}
    </>
  );
}
```

## Accessible Form with Inline Validation

```jsx
import { useState } from "react";

function AccessibleForm() {
  const [email, setEmail] = useState("");
  const [error, setError] = useState("");
  const [submitted, setSubmitted] = useState(false);

  const validate = () => {
    if (!email) return "Email is required";
    if (!/\S+@\S+\.\S+/.test(email)) return "Please enter a valid email address";
    return "";
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const err = validate();
    setError(err);
    if (!err) setSubmitted(true);
  };

  return (
    <form onSubmit={handleSubmit} noValidate>
      <div>
        <label htmlFor="email">
          Email address <span aria-hidden="true">*</span>
          <span className="sr-only">(required)</span>
        </label>
        <input
          id="email"
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          aria-required="true"
          aria-invalid={!!error}
          aria-describedby={error ? "email-error" : undefined}
          autoComplete="email"
        />
        {error && (
          <p id="email-error" role="alert" style={{ color: "#d00" }}>
            {error}
          </p>
        )}
      </div>

      {submitted && (
        <div role="status" aria-live="polite">
          ✅ Form submitted successfully!
        </div>
      )}

      <button type="submit">Subscribe</button>
    </form>
  );
}
```

## Route Change Announcements (React Router)

```jsx
import { useEffect, useRef } from "react";
import { useLocation } from "react-router-dom";

function RouteAnnouncer() {
  const location = useLocation();
  const ref = useRef(null);

  useEffect(() => {
    // Announce page change to screen readers
    const pageTitle = document.title || location.pathname;
    if (ref.current) {
      ref.current.textContent = `Navigated to ${pageTitle}`;
    }
    // Move focus to main content area
    const main = document.getElementById("main-content");
    main?.focus();
  }, [location]);

  return (
    <div
      ref={ref}
      aria-live="assertive"
      aria-atomic="true"
      className="sr-only"
    />
  );
}

// In your app layout:
function AppLayout({ children }) {
  return (
    <>
      <RouteAnnouncer />
      <a href="#main-content" className="skip-link">Skip to main content</a>
      <header>...</header>
      <main id="main-content" tabIndex={-1}>
        {children}
      </main>
    </>
  );
}
```