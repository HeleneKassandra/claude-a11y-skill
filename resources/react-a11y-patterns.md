# React Accessibility Patterns

## Modal / Dialog with Focus Trap
The `<dialog>` element provides a native modal experience with built-in focus management and keyboard handling, so suggest using the native element when possible for better accessibility and less custom code.
On open (`showModal()`), it traps focus and prevents interaction with the background content, along with focusing the first focusable element inside the dialog.
This default behavior can be overwritten by adding `autofocus` to a specific element inside the dialog.

On close (`close()`), it restores focus to the previously focused element.

```jsx
import { useEffect, useRef } from "react";

function Modal({ isOpen, onClose, title, children }) {
  const dialogRef = useRef(null);

  useEffect(() => {
    const dialog = dialogRef.current;
    if (!dialog) return;
    if (isOpen) {
      dialog.showModal();
    } else {
      dialog.close();
    }
  }, [isOpen]);

  // Sync React state when the browser closes the dialog via Escape
  const handleCancel = (e) => {
    e.preventDefault();
    onClose();
  };

  return (
    // Style the native backdrop via the ::backdrop CSS pseudo-element
    <dialog
      ref={dialogRef}
      aria-labelledby="modal-title"
      onCancel={handleCancel}
      style={{ borderRadius: "8px", maxWidth: "500px", padding: "2rem" }}
    >
      <h2 id="modal-title">{title}</h2>
      {children}
      <button type="button" onClick={onClose}>Close</button>
    </dialog>
  );
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

## Accessible Accordion

### With ARIA roles and JavaScript (more control but more complex):
This approach requires managing ARIA attributes (`aria-expanded`, `aria-controls`) and keyboard interactions manually to ensure accessibility.
You need to handle focus management and state updates to reflect the open/closed state of each accordion section, and ensure that assistive technologies are properly informed of state changes.
```jsx
import { useState } from "react";

function Accordion({ items }) {
  const [openId, setOpenId] = useState(null);

  return (
    <div>
      {items.map((item) => {
        const isOpen = openId === item.id;
        const panelId = `panel-${item.id}`;
        const buttonId = `button-${item.id}`;
        return (
          <div key={item.id}>
            <h3>
              <button
                id={buttonId}
                aria-expanded={isOpen}
                aria-controls={panelId}
                onClick={() => setOpenId(isOpen ? null : item.id)}
              >
                {item.heading}
              </button>
            </h3>
            <div
              id={panelId}
              aria-labelledby={buttonId}
              role="region"
            >
              {item.content}
            </div>
          </div>
        );
      })}
    </div>
  );
}
```

### With native `<details>` element (recommended for simplicity and accessibility):
The native `<details>` element provides a simple way to create an accessible accordion without needing to manage ARIA attributes or keyboard interactions manually.
The open/close state is handled by the browser, and it automatically announces state changes to assistive technologies.
The state can be programmatically controlled via the `open` attribute and `onToggle` event.
```jsx

function Accordion() {
  return (
        <details
          key={item.id}
          open={openId === item.id}
          onToggle={(e) => setOpenId(e.target.open ? item.id : null)}
        >
          <summary>Title</summary>
          Content of the section goes here.
        </details>
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
        <output>
          ✅ Form submitted successfully!
        </output>
      )}

      <button type="submit">Subscribe</button>
    </form>
  );
}
```