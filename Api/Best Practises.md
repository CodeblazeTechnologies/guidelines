# Best Practices

### Whats an Api?
    

### 1. Prefer plural nouns

    Use plural nouns while naming, that will keep things simple.
     (ie. books instead of book)

    *Example:*

                    *GET*                         *POST*                        *PUT*                         *DELETE*
      */books*      to fetch books              to create a book       collective updates of books        delete all books


      */books/12*   to fetch a particular book  (405) Method not allowed    update a particular book      delete a particular book
