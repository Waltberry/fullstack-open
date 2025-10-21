## 0.4 — Creating a new note on the traditional Notes page

```mermaid
sequenceDiagram
    participant browser
    participant server

    Note over browser: User types a note and clicks "Save" (form submit)

    %% 1) Submit form as POST (urlencoded)
    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note
    activate server
    server-->>browser: 302 Found<br/>Location: /exampleapp/notes
    deactivate server

    Note right of browser: Browser follows redirect (PRG pattern)

    %% 2) Reload Notes page (HTML)
    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/notes
    activate server
    server-->>browser: 200 OK (HTML document)
    deactivate server

    %% 3) Fetch CSS
    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate server
    server-->>browser: 200 OK (text/css)
    deactivate server

    %% 4) Fetch JavaScript
    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.js
    activate server
    server-->>browser: 200 OK (application/javascript)
    deactivate server

    Note right of browser: main.js runs and fetches the latest notes as JSON

    %% 5) Fetch data.json via XHR
    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: 200 OK (application/json)
    deactivate server

    Note over browser: JS callback renders the updated notes list into the DOM


## 0.5 - Visiting the SPA page (`/spa`)
```mermaid
sequenceDiagram
    participant browser
    participant server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/spa
    activate server
    server-->>browser: 200 OK (HTML document)
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate server
    server-->>browser: 200 OK (text/css)
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/spa.js
    activate server
    server-->>browser: 200 OK (application/javascript)
    deactivate server

    Note right of browser: spa.js runs and fetches initial data as JSON
    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: 200 OK (application/json)
    deactivate server

    Note over browser: JS renders the notes into the DOM (no full-page reloads later)



## 0.6 — Creating a new note in the SPA
```md
## 0.6: New note in the SPA
```mermaid
sequenceDiagram
    participant browser
    participant server

    Note over browser: User submits the form (no action/method on <form>)
    Note right of browser: JS intercepts submit (e.preventDefault())<br/>updates local state and rerenders immediately

    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note_spa
    activate server
    Note right of browser: Request body = JSON { content, date }<br/>Header: Content-Type: application/json
    server-->>browser: 201 Created
    deactivate server

    Note over browser: No redirect, no extra GETs; UI was already updated
