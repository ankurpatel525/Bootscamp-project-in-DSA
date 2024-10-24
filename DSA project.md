```python
class TrieNode:
    def __init__(self):
        # Initialize the TrieNode with a dictionary for children and a flag to mark the end of a word
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        # The root of the Trie is an empty TrieNode
        self.root = TrieNode()

    def insert(self, word):
        # Insert a word into the Trie
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word):
        # Check if a word is present in the Trie
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word

    def get_suggestions(self, prefix):
        # Return all words in the Trie that start with the given prefix
        node = self.root
        suggestions = []
        
        # Navigate to the end of the prefix
        for char in prefix:
            if char not in node.children:
                return suggestions
            node = node.children[char]

        # Use DFS to find all words with this prefix
        self._dfs(node, prefix, suggestions)
        return suggestions

    def _dfs(self, node, prefix, suggestions):
        # Perform Depth First Search to find words
        if node.is_end_of_word:
            suggestions.append(prefix)
        for char, child_node in node.children.items():
            self._dfs(child_node, prefix + char, suggestions)

class SpellChecker:
    def __init__(self, dictionary):
        # Initialize the SpellChecker with a Trie containing the words in the dictionary
        self.trie = Trie()
        for word in dictionary:
            self.trie.insert(word)

    def check_word(self, word):
        # Check if the word is correct or suggest alternatives
        if self.trie.search(word):
            return f"'{word}' is correct."
        else:
            suggestions = self.trie.get_suggestions(word)
            if suggestions:
                return f"'{word}' is incorrect. Did you mean: {', '.join(suggestions)}?"
            else:
                return f"'{word}' is incorrect and no suggestions found."

    def real_time_check(self, prefix):
        # Provide suggestions as the user types
        if not prefix:
            return "Start typing a word..."
        suggestions = self.trie.get_suggestions(prefix)
        if suggestions:
            return f"Suggestions: {', '.join(suggestions)}"
        else:
            return "No suggestions found."

# Example Usage
if __name__ == "__main__":
    # Dictionary of words to be added to the Trie
    words = ["hello", "help", "helmet", "hero", "happy", "horizon", "house", "hover", "host", "hotel","try", "sky", "fly", "cry", "dry", "apply", "reply", "supply", "deny",
    "spy", "butterfly", "skyline", "skyscraper", "trying", "frying", "sly", 
    "why", "my", "high", "shy"]

    # Initialize the SpellChecker with the word list
    spell_checker = SpellChecker(words)

    # Simulate real-time input
    def real_time_spell_check():
        print("Real-Time Spell Checker. Type 'exit' to quit.")
        while True:
            user_input = input("You: ").strip().lower()
            if user_input == "exit":
                break
            print(spell_checker.real_time_check(user_input))

    real_time_spell_check()

```

    Real-Time Spell Checker. Type 'exit' to quit.
    

    You:  app
    

    Suggestions: apply
    

    You:  sk
    

    Suggestions: sky, skyline, skyscraper
    


```python

```
