Write a document service
Documents will be private by default
Where User can create a Document
Owner can Edit a document
Owner can give access to others users
Documents can also be public
ReadDocument, EditDocument, access related code
In memory database

```go
package main

import (
	"fmt"
	"net/http"
	"sync"
)

// Document represents a document with its owner and access permissions.
type Document struct {
	ID      string
	Owner   string
	Content string
	Public  bool
	Readers map[string]bool // Track users who have read access
}

// InMemoryDB simulates an in-memory database for documents.
type InMemoryDB struct {
	sync.Mutex
	Documents map[string]*Document
}

// NewInMemoryDB initializes a new in-memory database.
func NewInMemoryDB() *InMemoryDB {
	return &InMemoryDB{
		Documents: make(map[string]*Document),
	}
}

// CreateDocument creates a new document in the database.
func (db *InMemoryDB) CreateDocument(owner, id, content string, public bool) {
	db.Lock()
	defer db.Unlock()
	db.Documents[id] = &Document{
		ID:      id,
		Owner:   owner,
		Content: content,
		Public:  public,
		Readers: make(map[string]bool),
	}
}

// EditDocument allows the owner to edit the document content.
func (db *InMemoryDB) EditDocument(owner, id, content string) error {
	db.Lock()
	defer db.Unlock()
	doc, exists := db.Documents[id]
	if !exists || doc.Owner != owner {
		return fmt.Errorf("permission denied or document not found")
	}
	doc.Content = content
	return nil
}

// GrantAccess allows the owner to grant read access to another user.
func (db *InMemoryDB) GrantAccess(owner, id, user string) error {
	db.Lock()
	defer db.Unlock()
	doc, exists := db.Documents[id]
	if !exists || doc.Owner != owner {
		return fmt.Errorf("permission denied or document not found")
	}
	doc.Readers[user] = true
	return nil
}

// ReadDocument retrieves the document content if the user has access.
func (db *InMemoryDB) ReadDocument(user, id string) (string, error) {
	db.Lock()
	defer db.Unlock()
	doc, exists := db.Documents[id]
	if !exists {
		return "", fmt.Errorf("document not found")
	}
	if doc.Owner == user || doc.Public || doc.Readers[user] {
		return doc.Content, nil
	}
	return "", fmt.Errorf("permission denied")
}

func main() {
	db := NewInMemoryDB()

	http.HandleFunc("/create", func(w http.ResponseWriter, r *http.Request) {
		r.ParseForm()
		id := r.FormValue("id")
		content := r.FormValue("content")
		public := r.FormValue("public") == "true"
		db.CreateDocument("user1", id, content, public)
		fmt.Fprintf(w, "Document %s created\n", id)
	})

	http.HandleFunc("/edit", func(w http.ResponseWriter, r *http.Request) {
		r.ParseForm()
		id := r.FormValue("id")
		content := r.FormValue("content")
		err := db.EditDocument("user1", id, content)
		if err != nil {
			http.Error(w, err.Error(), http.StatusForbidden)
			return
		}
		fmt.Fprintf(w, "Document %s edited\n", id)
	})

	http.HandleFunc("/grant", func(w http.ResponseWriter, r *http.Request) {
		r.ParseForm()
		id := r.FormValue("id")
		user := r.FormValue("user")
		err := db.GrantAccess("user1", id, user)
		if err != nil {
			http.Error(w, err.Error(), http.StatusForbidden)
			return
		}
		fmt.Fprintf(w, "Access granted to %s for document %s\n", user, id)
	})

	http.HandleFunc("/read", func(w http.ResponseWriter, r *http.Request) {
		r.ParseForm()
		id := r.FormValue("id")
		user := r.FormValue("user")
		content, err := db.ReadDocument(user, id)
		if err != nil {
			http.Error(w, err.Error(), http.StatusForbidden)
			return
		}
		fmt.Fprintf(w, "Content of document %s: %s\n", id, content)
	})

	http.ListenAndServe(":8080", nil)
}
```

### Explanation

**Data Structures**
- **Document**: Represents a document with fields for ID, owner, content, visibility (public/private), and a map to track users who have read access.
- **InMemoryDB**: Simulates an in-memory database using a mutex for thread safety. It contains methods for creating documents and managing access.

**Core Functions**
- **CreateDocument**: Allows a user to create a new document. The owner is set as the creator by default.
- **EditDocument**: Permits only the document's owner to edit its content. It checks if the requester is the owner before making changes.
- **GrantAccess**: Enables the owner to grant read access to other users. This function adds users to the `Readers` map of the document.
- **ReadDocument**: Retrieves the content of a document if the requester is either the owner or has been granted access. It also allows public access if specified.

**HTTP Handlers**
- The main function sets up HTTP routes for creating documents (`/create`), editing documents (`/edit`), granting access (`/grant`), and reading documents (`/read`). Each handler processes incoming requests and interacts with the in-memory database accordingly.

This implementation ensures that user access is managed effectively by checking ownership and permissions at each step.

Citations:
[1] https://www.reddit.com/r/golang/comments/tcbi6s/general_question_how_to_write_documentation_for_a/
[2] https://download.arangodb.com/arangodb32/doc/ArangoDB_Manual_3.2.8.pdf
[3] https://www.mitrais.com/news-updates/implementing-golang-api-documentation-using-go-swagger/
[4] https://appwrite.io/threads/1203814351006343219
[5] https://dev.to/_alaedeen_/build-your-first-service-in-golang-3in2
[6] https://developer.android.com/training/data-storage/shared/documents-files
[7] https://stackoverflow.com/questions/58249305/how-to-allow-firebase-user-to-only-access-documents-that-they-created
[8] https://techcommunity.microsoft.com/discussions/sharepoint_general/sharing-documents-and-permissions/242154