---
date: 2019-02-01
---

# Points to remember

- Controller should not be involved in heavy operations.
- Contexts create on database using code first approachmust be disposed in order to avoid memory leak.
- A service layer must be created such that it interacts with the controller and database.
- Seperation of concern should be maintained
- the controllers or the UI part should not directly involve bussiness logic
