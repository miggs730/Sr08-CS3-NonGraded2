# Documentation ✨
**2nd Graded Exercise**<br>
*Lawrence Miguel Cereñado*
*| Version 3*

Hello there! This is a documentation.md file containing the steps that I have done for the club signup website to work! This also contains the references I used for making this amazing webpage:> 💓 

<h2> To Do List (For keeping track!) </h2>

- 1️⃣ Get Route for join.hbs file ✅
- 2️⃣ Selecting Options in our HBS file ✅
- 3️⃣ POST Route for the join.hbs file ✅
  - 3️⃣.1️⃣ Read the JSON file ✅
  - 3️⃣.2️⃣ Obtain the data to manipulate in the server JS file ✅
  - 3️⃣.3️⃣ Manipulate the data to update it ✅
  - 3️⃣.4️⃣ Update the JSON file itself ✅
  - 3️⃣.5️⃣ Redirecting to the HBS file ✅
- 4️⃣ Add some users! 
Done! 🎉
## 1️⃣ GET Route for the  join.hbs file

Firstly, we have to resolve some routes. One of those routes involves our join.hbs file.

This get route will help us render our hbs file to display on our web browser! Here's the code I managed to devise to do this get route:
``` javascript
app.get('/join', (req, res) => {
  console.log("Form loaded successfully") 
  try {
    const clubs=  JSON.parse(fs.readFileSync(clubFilePath));
    const clubsList = Object.keys(clubs);
    console.log(clubsList);
    res.render('join',{clubsList});
  }
  catch (error) {
    console.log('Error reading clubs file', error);
    res.status(500).send('Error loading user data');
  }
});
```

Additionally, earlier in the file, I declared a constant named clubFilePath using some FS methods to simplify our path and use it anywhere without writing the whole path. This is an alternative to passing the clubsArray variable.
``` javascript
const clubFilePath = path.join(__dirname, 'data', 'clubs.json');
```

❓  **What does these code do?**
<ol>
<li>This code sets a route to '/join', which is the other Handlebars file we would be using for forms and getting data from the user. </li>
<li>We use the try and catch methods which would help us debug our code for any errors if it happens in any case. We also have terminal logs to make sure everything is working alright. </li>
<li> We parse the JSON object from our clubs.json file then obtain an array from it named clubList  which contains an array of all the clubs' names we would be using. We use Object.keys() for this.</li>
<li> Finally we render the file along with the object for the HBS file to process later on! </li>
</ol>

## 2️⃣ Selecting Options in our join.hbs file
Now that we have our file loaded, we have to address issues in our hbs file. Notice that our select option doesn't work, so we have to address that issue. We can easily fix that through some HBS code using templates! 

``` handlebars
<label for="organization">Preferred Organization / Club:</label>
<select id="preferredClub" name="preferredClub" required>
    <option value = ""> Select a club you prefer! </option>
    {{#each clubsList}} 
    <option value = "">{{this}}</option>
    {{/each}}
</select>
```
Now that we have our file loaded, we have to address issues in our hbs file. Notice that our select option doesn't work, so we have to address that issue. We can easily fix that through some HBS code using templates! 

We use the {{#each}} template in HBS for us to iterate over the clubsList array that we have obtained in Step 1. Then we use {{this}} for the option to show up!

## 3️⃣ POST Route for the join.hbs file 
Setting a POST route is essential for forms! Now that we have the select option part done, we can now adress to the form submission part! Let's set the method of the whole form to `method = POST`.

``` handlebars
    <form action="/submit-form" method="post" class="container"
          onsubmit = "return confirm('Accept Student-Sign-Up')"      
          onreset = "return confirm('Entered data will be lost. Continue?')" 
    > 
    <!-- ... -->
    </form>
```
Then, let's switch over to the `index.js` file.

We will create a POST route using `app.post()`, using this code: 
``` javascript
app.post('/submit-form', (req, res) => {
// ....
}); 
```
Essentially, we have to do four things:

**1. Read the JSON file and Obtaining Form Data**
  
  We can do this through some code:
  ``` javascript
app.post('/submit-form', (req, res) => {
  const {studentID, fullName, birthday, email, mobile, gradeLevel, type, organization, preferredClub, reason} = req.body; 
  try {
  // read existing clubMembers data
    console.log(`Reading existing user data...`);
    const clubMembers = JSON.parse(fs.readFileSync(usersFilePath));
  }
  catch (error) {
    console.log(`Error processing user submission:`, error);
    res.status(500).send(`Error saving user data`)
  }});
  ```
We use req.body so that we can read the body of the HTTP request (form data), which we can access using the said method. We assign the data into an object constant with the properties (this is determined by the properties 'names' in the HTML form).

We then use the try, catch method to allow us to debug the code if necessary.

Finally, we read the existing `user.json` file making use of the FS module's methods.

**2. Manipulating Data**

Now that we have our data from the form, the next thing to do is to somehow insert it in our current `user.json` file. First though, we have to manipulate the data for it to work through some JS methods.

``` javascript
    // assign a constant n
    const n = clubMembers[`students`].length;
    // assign a new value in the array at n
    // this makes a new object in the said array for saving data 
    clubMembers[`students`][n] = {studentID, fullName, birthday, email, mobile, gradeLevel, type, organization, preferredClub, reason};
    console.log(`Saving updates user data...`);
    
```
What this code does is:

- Assigning a constant `n` to the length of the current array. In actuality, this is equal to the cardinality of the last array item plus 1.
- We then insert the object data in the `students` array in the `clubMembers` JSON object. In JS, we can express this through bracket notation.
- We do some updates to the terminal for debugging.

**3. Updating the Data**
``` javascript
    fs.writeFileSync(usersFilePath, JSON.stringify(clubMembers, null, 2));
    console.log(`User data saved successfully`);
    console.log(`Redirecting to info page...`) ;
    res.redirect('/join');
```
Finally, we update the `users.json` file containing the new object with the new data! Then we redirect it to the '/join' page for the user to click the See Members list.

## Some Images of Users
![member list](image-1.png)
``` json
    {
      "studentID": "713321",
      "fullName": "Adam Evans",
      "birthday": "2012-02-11",
      "email": "adamevans@gmail.com",
      "mobile": "9112451222",
      "gradeLevel": "grade9",
      "type": "extern",
      "preferredClub": "Computron",
      "reason": "I like coding! "
    },
    {
      "studentID": "819122",
      "fullName": "John Evans",
      "birthday": "2007-02-11",
      "email": "johnevans@gmail.com",
      "mobile": "71233216",
      "gradeLevel": "grade11",
      "type": "intern",
      "preferredClub": "Alianti",
      "reason": "I wanna play frisbee!"
    }
```

Thank you! :>

## Extras
If you want the GitHub reppsitory link of my project, see this link!

https://github.com/miggs730/Sr08-CS3-NonGraded2/
# 📖 References

Getting started. (n.d.). StackBlitz Docs. https://developer.stackblitz.com/guides/user-guide/getting-started 

How To 1 Minute. (2022, April 16). How to change github repository from private to public 2025 [Video]. YouTube. https://www.youtube.com/watch?v=tEwmIoU1NUg 

Importing projects. (n.d.-a). StackBlitz Docs. https://developer.stackblitz.com/guides/user-guide/importing-projects 

Markdown cheat Sheet | Markdown Guide. (n.d.). https://www.markdownguide.org/cheat-sheet/ 

The Code City. (2024a, April 9). How to Commit and Push to Github from VSCode (2024 Update) [Video]. YouTube. https://www.youtube.com/watch?v=4dkNn93DIx4 

The Code City. (2024b, September 30). How to upload project to GitHub using Visual Studio Code (2024) | Push to GitHub from VSCode [Video]. YouTube. https://www.youtube.com/watch?v=JB7YD7OKm5g 

VS Code | Markdown Guide. (n.d.). https://www.markdownguide.org/tools/vscode/

 W3Schools.com. (n.d.). https://www.w3schools.com/js/js_errors.asp

