```go
// A map to store URLs in memory
var urlStore = make(map[string]string)
var mu sync.Mutex // Mutex to handle concurrent access

// Initialize the random seed
func init() {
	rand.Seed(time.Now().UnixNano())
}

// Generates a random string of fixed length (short code)
func generateShortCode(length int) string {
	const charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
	code := make([]byte, length)
	for i := range code {
		code[i] = charset[rand.Intn(len(charset))]
	}
	return string(code)
}

// ShortenURL takes a long URL and returns a short code
func ShortenURL(longURL string) string {
	mu.Lock()
	defer mu.Unlock()

	// Generate a unique short code
	shortCode := generateShortCode(6)
	for urlStore[shortCode] != "" { // Ensure uniqueness
		shortCode = generateShortCode(6)
	}

	// Store the mapping in memory
	urlStore[shortCode] = longURL
	return shortCode
}

// GetOriginalURL retrieves the original URL from a short code
func GetOriginalURL(shortCode string) (string, bool) {
	mu.Lock()
	defer mu.Unlock()

	longURL, exists := urlStore[shortCode]
	return longURL, exists
}

// Main for demonstration
func main() {
	// Shorten a URL
	longURL := "https://example.com"
	shortCode := ShortenURL(longURL)
	fmt.Printf("Short code for %s: %s\n", longURL, shortCode)

	// Retrieve the original URL
	originalURL, exists := GetOriginalURL(shortCode)
	if exists {
		fmt.Printf("Original URL for short code %s: %s\n", shortCode, originalURL)
	} else {
		fmt.Println("Short code not found!")
	}
}
